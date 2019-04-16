**통신QM Unit 이호영 선임(09340)**

<Exercise. Explore RDDs Using the Spark Shell>
-------------------------
**1. Pre-requisite to the exercise**


```
* 강의자료 : http://bit.ly/2Z3HEBY

* VMware Workstation 실행

* /home/training/training_materials/devsh/scripts/catchup.sh 실행 -> 5번 수행

* pyspark / spark-shell 실행 확인

* home Directory에서 pyspark 실행환경 변경
	- cd ~
	- vi .bashrc
	- 주석변경
	- source .bashrc
	- pyspark
	- jupyter 오픈 확인
```


**2. Playing with RDD**

1. SparkContext
```
> sc
<pyspark.context.SparkContext at 0x7f5fe4529a10>
> sc.version
u'1.6.0'
```

2. Reading file from local file-system
```
> myrdd = sc.textFile("file:/home/training/training_materials/data/frostroad.txt")
> myrdd.take(2)
[u'Two roads diverged in a yellow wood,', u'And sorry I could not travel both']

> myrdd.collect()
[u'Two roads diverged in a yellow wood,',
 u'And sorry I could not travel both',
 u'And be one traveler, long I stood',
 u'And looked down one as far as I could',
 u'To where it bent in the undergrowth;',
 u'',
 u'Then took the other, as just as fair,',
 u'And having perhaps the better claim,',
 u'Because it was grassy and wanted wear;',
 u'Though as for that the passing there',
 u'Had worn them really about the same,',
 u'',
 u'And both that morning equally lay',
 u'In leaves no step had trodden black.',
 u'Oh, I kept the first for another day!',
 u'Yet knowing how way leads on to way,',
 u'I doubted if I should ever come back.',
 u'',
 u'I shall be telling this with a sigh',
 u'Somewhere ages and ages hence:',
 u'Two roads diverged in a wood, and I--',
 u'I took the one less traveled by,',
 u'And that has made all the difference.']
```

3. Reading file from HDFS

  3-1. 사용할 데이터
```
training@localhost weblogs]$ hdfs dfs -ls /loudacre/weblogs
Found 182 items
-rw-rw-rw-   1 training supergroup     521343 2019-04-14 21:28 /loudacre/weblogs/2013-09-15.log
-rw-rw-rw-   1 training supergroup     484079 2019-04-14 21:28 /loudacre/weblogs/2013-09-16.log
-rw-rw-rw-   1 training supergroup     527399 2019-04-14 21:28 /loudacre/weblogs/2013-09-17.log
-rw-rw-rw-   1 training supergroup     485105 2019-04-14 21:28 /loudacre/weblogs/2013-09-18.log
-rw-rw-rw-   1 training supergroup     508553 2019-04-14 21:28 /loudacre/weblogs/2013-09-19.log
-rw-rw-rw-   1 training supergroup     492134 2019-04-14 21:28 /loudacre/weblogs/2013-09-20.log
-rw-rw-rw-   1 training supergroup     489117 2019-04-14 21:28 /loudacre/weblogs/2013-09-21.log
-rw-rw-rw-   1 training supergroup     535780 2019-04-14 21:28 /loudacre/weblogs/2013-09-22.log
-rw-rw-rw-   1 training supergroup     501768 2019-04-14 21:28 /loudacre/weblogs/2013-09-23.log
-rw-rw-rw-   1 training supergroup     489344 2019-04-14 21:28 /loudacre/weblogs/2013-09-24.log
-rw-rw-rw-   1 training supergroup     487857 2019-04-14 21:28 /loudacre/weblogs/2013-09-25.log
-rw-rw-rw-   1 training supergroup     523186 2019-04-14 21:28 /loudacre/weblogs/2013-09-26.log
-rw-rw-rw-   1 training supergroup     506195 2019-04-14 21:28 /loudacre/weblogs/2013-09-27.log
-rw-rw-rw-   1 training supergroup     492600 2019-04-14 21:28 /loudacre/weblogs/2013-09-28.log
-rw-rw-rw-   1 training supergroup     481479 2019-04-14 21:28 /loudacre/weblogs/2013-09-29.log
-rw-rw-rw-   1 training supergroup     525367 2019-04-14 21:28 /loudacre/weblogs/2013-09-30.log
-rw-rw-rw-   1 training supergroup     484920 2019-04-14 21:28 /loudacre/weblogs/2013-10-01.log
-rw-rw-rw-   1 training supergroup     527452 2019-04-14 21:28 /loudacre/weblogs/2013-10-02.log
-rw-rw-rw-   1 training supergroup     521262 2019-04-14 21:28 /loudacre/weblogs/2013-10-03.log
-rw-rw-rw-   1 training supergroup     498950 2019-04-14 21:28 /loudacre/weblogs/2013-10-04.log
-rw-rw-rw-   1 training supergroup     510510 2019-04-14 21:28 /loudacre/weblogs/2013-10-05.log
-rw-rw-rw-   1 training supergroup     489554 2019-04-14 21:28 /loudacre/weblogs/2013-10-06.log
-rw-rw-rw-   1 training supergroup     489616 2019-04-14 21:28 /loudacre/weblogs/2013-10-07.log
-rw-rw-rw-   1 training supergroup     523298 2019-04-14 21:28 /loudacre/weblogs/2013-10-08.log
-rw-rw-rw-   1 training supergroup     529434 2019-04-14 21:28 /loudacre/weblogs/2013-10-09.log
-rw-rw-rw-   1 training supergroup     521604 2019-04-14 21:28 /loudacre/weblogs/2013-10-10.log
-rw-rw-rw-   1 training supergroup     524138 2019-04-14 21:28 /loudacre/weblogs/2013-10-11.log
-rw-rw-rw-   1 training supergroup     517782 2019-04-14 21:28 /loudacre/weblogs/2013-10-12.log
-rw-rw-rw-   1 training supergroup     522873 2019-04-14 21:28 /loudacre/weblogs/2013-10-13.log
-rw-rw-rw-   1 training supergroup     496593 2019-04-14 21:28 /loudacre/weblogs/2013-10-14.log
-rw-rw-rw-   1 training supergroup     495224 2019-04-14 21:28 /loudacre/weblogs/2013-10-15.log
-rw-rw-rw-   1 training supergroup     521024 2019-04-14 21:28 /loudacre/weblogs/2013-10-16.log
-rw-rw-rw-   1 training supergroup     500818 2019-04-14 21:28 /loudacre/weblogs/2013-10-17.log
-rw-rw-rw-   1 training supergroup     501154 2019-04-14 21:28 /loudacre/weblogs/2013-10-18.log
-rw-rw-rw-   1 training supergroup     527543 2019-04-14 21:28 /loudacre/weblogs/2013-10-19.log
-rw-rw-rw-   1 training supergroup     518589 2019-04-14 21:28 /loudacre/weblogs/2013-10-20.log
-rw-rw-rw-   1 training supergroup     512893 2019-04-14 21:28 /loudacre/weblogs/2013-10-21.log
-rw-rw-rw-   1 training supergroup     518030 2019-04-14 21:28 /loudacre/weblogs/2013-10-22.log
-rw-rw-rw-   1 training supergroup     492463 2019-04-14 21:28 /loudacre/weblogs/2013-10-23.log
-rw-rw-rw-   1 training supergroup     492054 2019-04-14 21:28 /loudacre/weblogs/2013-10-24.log
-rw-rw-rw-   1 training supergroup     533207 2019-04-14 21:28 /loudacre/weblogs/2013-10-25.log
-rw-rw-rw-   1 training supergroup     503777 2019-04-14 21:28 /loudacre/weblogs/2013-10-26.log
-rw-rw-rw-   1 training supergroup     503420 2019-04-14 21:28 /loudacre/weblogs/2013-10-27.log
-rw-rw-rw-   1 training supergroup     524048 2019-04-14 21:28 /loudacre/weblogs/2013-10-28.log
-rw-rw-rw-   1 training supergroup     510800 2019-04-14 21:28 /loudacre/weblogs/2013-10-29.log
-rw-rw-rw-   1 training supergroup     523180 2019-04-14 21:28 /loudacre/weblogs/2013-10-30.log
-rw-rw-rw-   1 training supergroup     524277 2019-04-14 21:28 /loudacre/weblogs/2013-10-31.log
-rw-rw-rw-   1 training supergroup     969586 2019-04-14 21:28 /loudacre/weblogs/2013-11-01.log
-rw-rw-rw-   1 training supergroup    1061868 2019-04-14 21:28 /loudacre/weblogs/2013-11-02.log
-rw-rw-rw-   1 training supergroup    1093742 2019-04-14 21:28 /loudacre/weblogs/2013-11-03.log
-rw-rw-rw-   1 training supergroup     956153 2019-04-14 21:28 /loudacre/weblogs/2013-11-04.log
-rw-rw-rw-   1 training supergroup    1085429 2019-04-14 21:28 /loudacre/weblogs/2013-11-05.log
-rw-rw-rw-   1 training supergroup    1071334 2019-04-14 21:28 /loudacre/weblogs/2013-11-06.log
-rw-rw-rw-   1 training supergroup    1038115 2019-04-14 21:28 /loudacre/weblogs/2013-11-07.log
-rw-rw-rw-   1 training supergroup    1078367 2019-04-14 21:28 /loudacre/weblogs/2013-11-08.log
-rw-rw-rw-   1 training supergroup     970029 2019-04-14 21:28 /loudacre/weblogs/2013-11-09.log
-rw-rw-rw-   1 training supergroup    1095755 2019-04-14 21:28 /loudacre/weblogs/2013-11-10.log
-rw-rw-rw-   1 training supergroup     977765 2019-04-14 21:28 /loudacre/weblogs/2013-11-11.log
-rw-rw-rw-   1 training supergroup     970149 2019-04-14 21:28 /loudacre/weblogs/2013-11-12.log
-rw-rw-rw-   1 training supergroup    1039116 2019-04-14 21:28 /loudacre/weblogs/2013-11-13.log
-rw-rw-rw-   1 training supergroup    1081771 2019-04-14 21:28 /loudacre/weblogs/2013-11-14.log
-rw-rw-rw-   1 training supergroup     982380 2019-04-14 21:28 /loudacre/weblogs/2013-11-15.log
-rw-rw-rw-   1 training supergroup    1009713 2019-04-14 21:28 /loudacre/weblogs/2013-11-16.log
-rw-rw-rw-   1 training supergroup    1086494 2019-04-14 21:28 /loudacre/weblogs/2013-11-17.log
-rw-rw-rw-   1 training supergroup     978348 2019-04-14 21:28 /loudacre/weblogs/2013-11-18.log
-rw-rw-rw-   1 training supergroup    1083789 2019-04-14 21:28 /loudacre/weblogs/2013-11-19.log
-rw-rw-rw-   1 training supergroup    1009470 2019-04-14 21:28 /loudacre/weblogs/2013-11-20.log
-rw-rw-rw-   1 training supergroup    1057086 2019-04-14 21:28 /loudacre/weblogs/2013-11-21.log
-rw-rw-rw-   1 training supergroup     975288 2019-04-14 21:28 /loudacre/weblogs/2013-11-22.log
-rw-rw-rw-   1 training supergroup    1063118 2019-04-14 21:28 /loudacre/weblogs/2013-11-23.log
-rw-rw-rw-   1 training supergroup    1048393 2019-04-14 21:28 /loudacre/weblogs/2013-11-24.log
-rw-rw-rw-   1 training supergroup     974776 2019-04-14 21:28 /loudacre/weblogs/2013-11-25.log
-rw-rw-rw-   1 training supergroup     962392 2019-04-14 21:28 /loudacre/weblogs/2013-11-26.log
-rw-rw-rw-   1 training supergroup     976234 2019-04-14 21:28 /loudacre/weblogs/2013-11-27.log
-rw-rw-rw-   1 training supergroup     976027 2019-04-14 21:28 /loudacre/weblogs/2013-11-28.log
-rw-rw-rw-   1 training supergroup    1081953 2019-04-14 21:28 /loudacre/weblogs/2013-11-29.log
-rw-rw-rw-   1 training supergroup    1006926 2019-04-14 21:28 /loudacre/weblogs/2013-11-30.log
-rw-rw-rw-   1 training supergroup    1014795 2019-04-14 21:28 /loudacre/weblogs/2013-12-01.log
-rw-rw-rw-   1 training supergroup    1093271 2019-04-14 21:28 /loudacre/weblogs/2013-12-02.log
-rw-rw-rw-   1 training supergroup    1068328 2019-04-14 21:28 /loudacre/weblogs/2013-12-03.log
-rw-rw-rw-   1 training supergroup    1070066 2019-04-14 21:28 /loudacre/weblogs/2013-12-04.log
-rw-rw-rw-   1 training supergroup    1028342 2019-04-14 21:28 /loudacre/weblogs/2013-12-05.log
-rw-rw-rw-   1 training supergroup    1031373 2019-04-14 21:28 /loudacre/weblogs/2013-12-06.log
-rw-rw-rw-   1 training supergroup    1018063 2019-04-14 21:28 /loudacre/weblogs/2013-12-07.log
-rw-rw-rw-   1 training supergroup    1039394 2019-04-14 21:28 /loudacre/weblogs/2013-12-08.log
-rw-rw-rw-   1 training supergroup    1101053 2019-04-14 21:28 /loudacre/weblogs/2013-12-09.log
-rw-rw-rw-   1 training supergroup    1009430 2019-04-14 21:28 /loudacre/weblogs/2013-12-10.log
-rw-rw-rw-   1 training supergroup    1061891 2019-04-14 21:28 /loudacre/weblogs/2013-12-11.log
-rw-rw-rw-   1 training supergroup    1030641 2019-04-14 21:28 /loudacre/weblogs/2013-12-12.log
-rw-rw-rw-   1 training supergroup    1028946 2019-04-14 21:28 /loudacre/weblogs/2013-12-13.log
-rw-rw-rw-   1 training supergroup     985819 2019-04-14 21:28 /loudacre/weblogs/2013-12-14.log
-rw-rw-rw-   1 training supergroup    1080787 2019-04-14 21:28 /loudacre/weblogs/2013-12-15.log
-rw-rw-rw-   1 training supergroup    1004498 2019-04-14 21:28 /loudacre/weblogs/2013-12-16.log
-rw-rw-rw-   1 training supergroup    1078566 2019-04-14 21:28 /loudacre/weblogs/2013-12-17.log
-rw-rw-rw-   1 training supergroup    1069630 2019-04-14 21:28 /loudacre/weblogs/2013-12-18.log
-rw-rw-rw-   1 training supergroup    1050695 2019-04-14 21:28 /loudacre/weblogs/2013-12-19.log
-rw-rw-rw-   1 training supergroup    1108042 2019-04-14 21:28 /loudacre/weblogs/2013-12-20.log
-rw-rw-rw-   1 training supergroup     986692 2019-04-14 21:28 /loudacre/weblogs/2013-12-21.log
-rw-rw-rw-   1 training supergroup    1045723 2019-04-14 21:28 /loudacre/weblogs/2013-12-22.log
-rw-rw-rw-   1 training supergroup     990531 2019-04-14 21:28 /loudacre/weblogs/2013-12-23.log
-rw-rw-rw-   1 training supergroup    1059352 2019-04-14 21:28 /loudacre/weblogs/2013-12-24.log
-rw-rw-rw-   1 training supergroup     980457 2019-04-14 21:28 /loudacre/weblogs/2013-12-25.log
-rw-rw-rw-   1 training supergroup    1054182 2019-04-14 21:28 /loudacre/weblogs/2013-12-26.log
-rw-rw-rw-   1 training supergroup    1017134 2019-04-14 21:28 /loudacre/weblogs/2013-12-27.log
-rw-rw-rw-   1 training supergroup    1064484 2019-04-14 21:28 /loudacre/weblogs/2013-12-28.log
-rw-rw-rw-   1 training supergroup    1022151 2019-04-14 21:28 /loudacre/weblogs/2013-12-29.log
-rw-rw-rw-   1 training supergroup    1070391 2019-04-14 21:28 /loudacre/weblogs/2013-12-30.log
-rw-rw-rw-   1 training supergroup     992528 2019-04-14 21:28 /loudacre/weblogs/2013-12-31.log
-rw-rw-rw-   1 training supergroup    1057534 2019-04-14 21:28 /loudacre/weblogs/2014-01-01.log
-rw-rw-rw-   1 training supergroup    1053566 2019-04-14 21:28 /loudacre/weblogs/2014-01-02.log
-rw-rw-rw-   1 training supergroup     981998 2019-04-14 21:28 /loudacre/weblogs/2014-01-03.log
-rw-rw-rw-   1 training supergroup    1100898 2019-04-14 21:28 /loudacre/weblogs/2014-01-04.log
-rw-rw-rw-   1 training supergroup    1012644 2019-04-14 21:28 /loudacre/weblogs/2014-01-05.log
-rw-rw-rw-   1 training supergroup    1094868 2019-04-14 21:28 /loudacre/weblogs/2014-01-06.log
-rw-rw-rw-   1 training supergroup    1094753 2019-04-14 21:28 /loudacre/weblogs/2014-01-07.log
-rw-rw-rw-   1 training supergroup    1037422 2019-04-14 21:28 /loudacre/weblogs/2014-01-08.log
-rw-rw-rw-   1 training supergroup    1032385 2019-04-14 21:28 /loudacre/weblogs/2014-01-09.log
-rw-rw-rw-   1 training supergroup     977679 2019-04-14 21:28 /loudacre/weblogs/2014-01-10.log
-rw-rw-rw-   1 training supergroup     993697 2019-04-14 21:28 /loudacre/weblogs/2014-01-11.log
-rw-rw-rw-   1 training supergroup    1073307 2019-04-14 21:28 /loudacre/weblogs/2014-01-12.log
-rw-rw-rw-   1 training supergroup    1048789 2019-04-14 21:28 /loudacre/weblogs/2014-01-13.log
-rw-rw-rw-   1 training supergroup    1001350 2019-04-14 21:28 /loudacre/weblogs/2014-01-14.log
-rw-rw-rw-   1 training supergroup    1048617 2019-04-14 21:28 /loudacre/weblogs/2014-01-15.log
-rw-rw-rw-   1 training supergroup     996816 2019-04-14 21:28 /loudacre/weblogs/2014-01-16.log
-rw-rw-rw-   1 training supergroup    1044827 2019-04-14 21:28 /loudacre/weblogs/2014-01-17.log
-rw-rw-rw-   1 training supergroup     972872 2019-04-14 21:28 /loudacre/weblogs/2014-01-18.log
-rw-rw-rw-   1 training supergroup     975136 2019-04-14 21:28 /loudacre/weblogs/2014-01-19.log
-rw-rw-rw-   1 training supergroup    1020891 2019-04-14 21:28 /loudacre/weblogs/2014-01-20.log
-rw-rw-rw-   1 training supergroup    1011424 2019-04-14 21:28 /loudacre/weblogs/2014-01-21.log
-rw-rw-rw-   1 training supergroup    1109304 2019-04-14 21:28 /loudacre/weblogs/2014-01-22.log
-rw-rw-rw-   1 training supergroup     976011 2019-04-14 21:28 /loudacre/weblogs/2014-01-23.log
-rw-rw-rw-   1 training supergroup    1061764 2019-04-14 21:28 /loudacre/weblogs/2014-01-24.log
-rw-rw-rw-   1 training supergroup    1072204 2019-04-14 21:28 /loudacre/weblogs/2014-01-25.log
-rw-rw-rw-   1 training supergroup    1065979 2019-04-14 21:28 /loudacre/weblogs/2014-01-26.log
-rw-rw-rw-   1 training supergroup    1096363 2019-04-14 21:28 /loudacre/weblogs/2014-01-27.log
-rw-rw-rw-   1 training supergroup    1083865 2019-04-14 21:28 /loudacre/weblogs/2014-01-28.log
-rw-rw-rw-   1 training supergroup    1012968 2019-04-14 21:28 /loudacre/weblogs/2014-01-29.log
-rw-rw-rw-   1 training supergroup    1081064 2019-04-14 21:28 /loudacre/weblogs/2014-01-30.log
-rw-rw-rw-   1 training supergroup    1101529 2019-04-14 21:28 /loudacre/weblogs/2014-01-31.log
-rw-rw-rw-   1 training supergroup    1053099 2019-04-14 21:28 /loudacre/weblogs/2014-02-01.log
-rw-rw-rw-   1 training supergroup    1067044 2019-04-14 21:28 /loudacre/weblogs/2014-02-02.log
-rw-rw-rw-   1 training supergroup    1003003 2019-04-14 21:28 /loudacre/weblogs/2014-02-03.log
-rw-rw-rw-   1 training supergroup    1017259 2019-04-14 21:28 /loudacre/weblogs/2014-02-04.log
-rw-rw-rw-   1 training supergroup    1005783 2019-04-14 21:28 /loudacre/weblogs/2014-02-05.log
-rw-rw-rw-   1 training supergroup    1065257 2019-04-14 21:28 /loudacre/weblogs/2014-02-06.log
-rw-rw-rw-   1 training supergroup     996107 2019-04-14 21:28 /loudacre/weblogs/2014-02-07.log
-rw-rw-rw-   1 training supergroup    1035289 2019-04-14 21:28 /loudacre/weblogs/2014-02-08.log
-rw-rw-rw-   1 training supergroup    1065078 2019-04-14 21:28 /loudacre/weblogs/2014-02-09.log
-rw-rw-rw-   1 training supergroup    1044403 2019-04-14 21:28 /loudacre/weblogs/2014-02-10.log
-rw-rw-rw-   1 training supergroup    1057235 2019-04-14 21:28 /loudacre/weblogs/2014-02-11.log
-rw-rw-rw-   1 training supergroup    1091711 2019-04-14 21:28 /loudacre/weblogs/2014-02-12.log
-rw-rw-rw-   1 training supergroup    1096726 2019-04-14 21:28 /loudacre/weblogs/2014-02-13.log
-rw-rw-rw-   1 training supergroup    1072065 2019-04-14 21:28 /loudacre/weblogs/2014-02-14.log
-rw-rw-rw-   1 training supergroup    1010563 2019-04-14 21:28 /loudacre/weblogs/2014-02-15.log
-rw-rw-rw-   1 training supergroup    1056078 2019-04-14 21:28 /loudacre/weblogs/2014-02-16.log
-rw-rw-rw-   1 training supergroup     994481 2019-04-14 21:28 /loudacre/weblogs/2014-02-17.log
-rw-rw-rw-   1 training supergroup    1019882 2019-04-14 21:28 /loudacre/weblogs/2014-02-18.log
-rw-rw-rw-   1 training supergroup     992774 2019-04-14 21:28 /loudacre/weblogs/2014-02-19.log
-rw-rw-rw-   1 training supergroup     992721 2019-04-14 21:28 /loudacre/weblogs/2014-02-20.log
-rw-rw-rw-   1 training supergroup    1024612 2019-04-14 21:28 /loudacre/weblogs/2014-02-21.log
-rw-rw-rw-   1 training supergroup    1060506 2019-04-14 21:28 /loudacre/weblogs/2014-02-22.log
-rw-rw-rw-   1 training supergroup    1017947 2019-04-14 21:28 /loudacre/weblogs/2014-02-23.log
-rw-rw-rw-   1 training supergroup    1104389 2019-04-14 21:28 /loudacre/weblogs/2014-02-24.log
-rw-rw-rw-   1 training supergroup    1018667 2019-04-14 21:28 /loudacre/weblogs/2014-02-25.log
-rw-rw-rw-   1 training supergroup    1076821 2019-04-14 21:28 /loudacre/weblogs/2014-02-26.log
-rw-rw-rw-   1 training supergroup    1090745 2019-04-14 21:28 /loudacre/weblogs/2014-02-27.log
-rw-rw-rw-   1 training supergroup     975382 2019-04-14 21:28 /loudacre/weblogs/2014-02-28.log
-rw-rw-rw-   1 training supergroup    1004630 2019-04-14 21:28 /loudacre/weblogs/2014-03-01.log
-rw-rw-rw-   1 training supergroup    1005688 2019-04-14 21:28 /loudacre/weblogs/2014-03-02.log
-rw-rw-rw-   1 training supergroup     981270 2019-04-14 21:28 /loudacre/weblogs/2014-03-03.log
-rw-rw-rw-   1 training supergroup     968023 2019-04-14 21:28 /loudacre/weblogs/2014-03-04.log
-rw-rw-rw-   1 training supergroup    1006960 2019-04-14 21:28 /loudacre/weblogs/2014-03-05.log
-rw-rw-rw-   1 training supergroup    1014931 2019-04-14 21:28 /loudacre/weblogs/2014-03-06.log
-rw-rw-rw-   1 training supergroup    1103088 2019-04-14 21:28 /loudacre/weblogs/2014-03-07.log
-rw-rw-rw-   1 training supergroup    1001566 2019-04-14 21:28 /loudacre/weblogs/2014-03-08.log
-rw-rw-rw-   1 training supergroup     995517 2019-04-14 21:28 /loudacre/weblogs/2014-03-09.log
-rw-rw-rw-   1 training supergroup    1091560 2019-04-14 21:28 /loudacre/weblogs/2014-03-10.log
-rw-rw-rw-   1 training supergroup    1038692 2019-04-14 21:28 /loudacre/weblogs/2014-03-11.log
-rw-rw-rw-   1 training supergroup    1047117 2019-04-14 21:28 /loudacre/weblogs/2014-03-12.log
-rw-rw-rw-   1 training supergroup     997473 2019-04-14 21:28 /loudacre/weblogs/2014-03-13.log
-rw-rw-rw-   1 training supergroup     994147 2019-04-14 21:28 /loudacre/weblogs/2014-03-14.log
-rw-rw-rw-   1 training supergroup    1074872 2019-04-14 21:28 /loudacre/weblogs/2014-03-15.log
```

  3-2. RDD로 변환
```
> logfiles = "/loudacre/weblogs/*"
> logsRDD = sc.textFile(logfiles)
```

  3-3. jpg파일과 관련된 웹로그만 긁어보자
```
> jpglogsRDD = logsRDD.filter(lambda line: ".jpg" in line)
> jpglogsRDD.take(10)
[u'217.150.149.167 - 4712 [15/Sep/2013:23:56:06 +0100] "GET /ronin_s4.jpg HTTP/1.0" 200 5552 "http://www.loudacre.com"  "Loudacre Mobile Browser MeeToo 1.0"',
 u'104.184.210.93 - 28402 [15/Sep/2013:23:42:53 +0100] "GET /titanic_2200.jpg HTTP/1.0" 200 19466 "http://www.loudacre.com"  "Loudacre Mobile Browser MeeToo 2.0"',
 u'37.91.137.134 - 36171 [15/Sep/2013:23:39:33 +0100] "GET /ronin_novelty_note_3.jpg HTTP/1.0" 200 7432 "http://www.loudacre.com"  "Loudacre Mobile Browser iFruit 3"',
 u'177.43.223.203 - 90653 [15/Sep/2013:23:31:17 +0100] "GET /ifruit_3.jpg HTTP/1.0" 200 19578 "http://www.loudacre.com"  "Loudacre Mobile Browser Sorrento F31L"',
 u'19.250.65.76 - 44388 [15/Sep/2013:23:31:10 +0100] "GET /sorrento_f24l.jpg HTTP/1.0" 200 5730 "http://www.loudacre.com"  "Loudacre Mobile Browser iFruit 3A"',
 u'134.72.143.150 - 24554 [15/Sep/2013:23:13:42 +0100] "GET /sorrento_f24l.jpg HTTP/1.0" 200 703 "http://www.loudacre.com"  "Loudacre Mobile Browser iFruit 1"',
 u'48.202.252.134 - 24990 [15/Sep/2013:23:12:00 +0100] "GET /ifruit_3a.jpg HTTP/1.0" 200 1730 "http://www.loudacre.com"  "Loudacre Mobile Browser Titanic 2000"',
 u'100.30.199.161 - 9834 [15/Sep/2013:23:06:14 +0100] "GET /sorrento_f40l.jpg HTTP/1.0" 200 15995 "http://www.loudacre.com"  "Loudacre Mobile Browser iFruit 1"',
 u'58.46.139.19 - 10399 [15/Sep/2013:23:03:16 +0100] "GET /ronin_novelty_note_3.jpg HTTP/1.0" 200 17725 "http://www.loudacre.com"  "Loudacre Mobile Browser Titanic 1100"',
 u'135.41.174.97 - 58228 [15/Sep/2013:22:29:56 +0100] "GET /sorrento_f31l.jpg HTTP/1.0" 200 14615 "http://www.loudacre.com"  "Loudacre Mobile Browser iFruit 3"']

> sc.textFile(logfiles).filter(lambda line: ".jpg" in line).count()
64978

> logsRDD.map(lambda line: len(line)).take(5)
[151, 143, 154, 147, 160]

> # 아래의 경우 RDD의 각각의 Record는 배열형태
> logsRDD.map(lambda line: line.split(' ')).take(5)
[[u'3.94.78.5',
  u'-',
  u'69827',
  u'[15/Sep/2013:23:58:36',
  u'+0100]',
  u'"GET',
  u'/KBDOC-00033.html',
  u'HTTP/1.0"',
  u'200',
  u'14417',
  u'"http://www.loudacre.com"',
  u'',
  u'"Loudacre',
  u'Mobile',
  u'Browser',
  u'iFruit',
  u'1"'],
 [u'3.94.78.5',
  u'-',
  u'69827',
  u'[15/Sep/2013:23:58:36',
  u'+0100]',
  u'"GET',
  u'/theme.css',
  u'HTTP/1.0"',
  u'200',
  u'3576',
  u'"http://www.loudacre.com"',
  u'',
  u'"Loudacre',
  u'Mobile',
  u'Browser',
  u'iFruit',
  u'1"'],
 [u'19.38.140.62',
  u'-',
  u'21475',
  u'[15/Sep/2013:23:58:34',
  u'+0100]',
  u'"GET',
  u'/KBDOC-00277.html',
  u'HTTP/1.0"',
  u'200',
  u'15517',
  u'"http://www.loudacre.com"',
  u'',
  u'"Loudacre',
  u'Mobile',
  u'Browser',
  u'Ronin',
  u'S1"'],
 [u'19.38.140.62',
  u'-',
  u'21475',
  u'[15/Sep/2013:23:58:34',
  u'+0100]',
  u'"GET',
  u'/theme.css',
  u'HTTP/1.0"',
  u'200',
  u'13353',
  u'"http://www.loudacre.com"',
  u'',
  u'"Loudacre',
  u'Mobile',
  u'Browser',
  u'Ronin',
  u'S1"'],
 [u'129.133.56.105',
  u'-',
  u'2489',
  u'[15/Sep/2013:23:58:34',
  u'+0100]',
  u'"GET',
  u'/KBDOC-00033.html',
  u'HTTP/1.0"',
  u'200',
  u'10590',
  u'"http://www.loudacre.com"',
  u'',
  u'"Loudacre',
  u'Mobile',
  u'Browser',
  u'Sorrento',
  u'F00L"']]
  
> ipsRDD = logsRDD.map(lambda line: line.split(' ')[0])
> ipsRDD.take(5)
[u'3.94.78.5',
 u'3.94.78.5',
 u'19.38.140.62',
 u'19.38.140.62',
 u'129.133.56.105']
 
> for ip in ipsRDD.take(10) : print ip
3.94.78.5
3.94.78.5
19.38.140.62
19.38.140.62
129.133.56.105
129.133.56.105
217.150.149.167
217.150.149.167
217.150.149.167
217.150.149.167


> ipsRDD.saveAsTextFile("/loudacre/iplist")
[training@localhost weblogs]$ hdfs dfs -ls /loudacre/iplist/
Found 183 items
-rw-rw-rw-   1 training supergroup          0 2019-04-14 21:40 /loudacre/iplist/_SUCCESS
-rw-rw-rw-   1 training supergroup      49265 2019-04-14 21:39 /loudacre/iplist/part-00000
-rw-rw-rw-   1 training supergroup      45854 2019-04-14 21:39 /loudacre/iplist/part-00001
-rw-rw-rw-   1 training supergroup      50031 2019-04-14 21:39 /loudacre/iplist/part-00002
-rw-rw-rw-   1 training supergroup      45898 2019-04-14 21:39 /loudacre/iplist/part-00003
-rw-rw-rw-   1 training supergroup      48070 2019-04-14 21:39 /loudacre/iplist/part-00004
-rw-rw-rw-   1 training supergroup      46430 2019-04-14 21:39 /loudacre/iplist/part-00005
-rw-rw-rw-   1 training supergroup      46177 2019-04-14 21:39 /loudacre/iplist/part-00006
-rw-rw-rw-   1 training supergroup      50720 2019-04-14 21:39 /loudacre/iplist/part-00007
-rw-rw-rw-   1 training supergroup      47314 2019-04-14 21:39 /loudacre/iplist/part-00008
-rw-rw-rw-   1 training supergroup      46282 2019-04-14 21:39 /loudacre/iplist/part-00009
-rw-rw-rw-   1 training supergroup      45998 2019-04-14 21:39 /loudacre/iplist/part-00010
-rw-rw-rw-   1 training supergroup      49261 2019-04-14 21:39 /loudacre/iplist/part-00011
-rw-rw-rw-   1 training supergroup      47918 2019-04-14 21:39 /loudacre/iplist/part-00012
-rw-rw-rw-   1 training supergroup      46580 2019-04-14 21:39 /loudacre/iplist/part-00013
-rw-rw-rw-   1 training supergroup      45576 2019-04-14 21:39 /loudacre/iplist/part-00014
-rw-rw-rw-   1 training supergroup      49698 2019-04-14 21:39 /loudacre/iplist/part-00015
-rw-rw-rw-   1 training supergroup      45627 2019-04-14 21:39 /loudacre/iplist/part-00016
-rw-rw-rw-   1 training supergroup      49712 2019-04-14 21:39 /loudacre/iplist/part-00017
-rw-rw-rw-   1 training supergroup      49193 2019-04-14 21:39 /loudacre/iplist/part-00018
-rw-rw-rw-   1 training supergroup      47206 2019-04-14 21:39 /loudacre/iplist/part-00019
-rw-rw-rw-   1 training supergroup      48088 2019-04-14 21:39 /loudacre/iplist/part-00020
-rw-rw-rw-   1 training supergroup      46346 2019-04-14 21:39 /loudacre/iplist/part-00021
-rw-rw-rw-   1 training supergroup      46079 2019-04-14 21:39 /loudacre/iplist/part-00022
-rw-rw-rw-   1 training supergroup      49463 2019-04-14 21:39 /loudacre/iplist/part-00023
-rw-rw-rw-   1 training supergroup      50071 2019-04-14 21:39 /loudacre/iplist/part-00024
-rw-rw-rw-   1 training supergroup      49298 2019-04-14 21:39 /loudacre/iplist/part-00025
-rw-rw-rw-   1 training supergroup      49523 2019-04-14 21:39 /loudacre/iplist/part-00026
-rw-rw-rw-   1 training supergroup      48979 2019-04-14 21:39 /loudacre/iplist/part-00027
-rw-rw-rw-   1 training supergroup      49241 2019-04-14 21:39 /loudacre/iplist/part-00028
-rw-rw-rw-   1 training supergroup      46977 2019-04-14 21:39 /loudacre/iplist/part-00029
-rw-rw-rw-   1 training supergroup      47085 2019-04-14 21:39 /loudacre/iplist/part-00030
-rw-rw-rw-   1 training supergroup      49301 2019-04-14 21:39 /loudacre/iplist/part-00031
-rw-rw-rw-   1 training supergroup      47146 2019-04-14 21:39 /loudacre/iplist/part-00032
-rw-rw-rw-   1 training supergroup      47439 2019-04-14 21:39 /loudacre/iplist/part-00033
-rw-rw-rw-   1 training supergroup      49843 2019-04-14 21:39 /loudacre/iplist/part-00034
-rw-rw-rw-   1 training supergroup      48852 2019-04-14 21:39 /loudacre/iplist/part-00035
-rw-rw-rw-   1 training supergroup      48425 2019-04-14 21:39 /loudacre/iplist/part-00036
-rw-rw-rw-   1 training supergroup      48696 2019-04-14 21:39 /loudacre/iplist/part-00037
-rw-rw-rw-   1 training supergroup      46612 2019-04-14 21:39 /loudacre/iplist/part-00038
-rw-rw-rw-   1 training supergroup      46337 2019-04-14 21:39 /loudacre/iplist/part-00039
-rw-rw-rw-   1 training supergroup      50269 2019-04-14 21:39 /loudacre/iplist/part-00040
-rw-rw-rw-   1 training supergroup      47601 2019-04-14 21:39 /loudacre/iplist/part-00041
-rw-rw-rw-   1 training supergroup      47440 2019-04-14 21:39 /loudacre/iplist/part-00042
-rw-rw-rw-   1 training supergroup      49665 2019-04-14 21:39 /loudacre/iplist/part-00043
-rw-rw-rw-   1 training supergroup      48386 2019-04-14 21:39 /loudacre/iplist/part-00044
-rw-rw-rw-   1 training supergroup      49493 2019-04-14 21:39 /loudacre/iplist/part-00045
-rw-rw-rw-   1 training supergroup      49556 2019-04-14 21:39 /loudacre/iplist/part-00046
-rw-rw-rw-   1 training supergroup      91745 2019-04-14 21:39 /loudacre/iplist/part-00047
-rw-rw-rw-   1 training supergroup     100562 2019-04-14 21:39 /loudacre/iplist/part-00048
-rw-rw-rw-   1 training supergroup     103214 2019-04-14 21:39 /loudacre/iplist/part-00049
-rw-rw-rw-   1 training supergroup      90439 2019-04-14 21:39 /loudacre/iplist/part-00050
-rw-rw-rw-   1 training supergroup     102547 2019-04-14 21:39 /loudacre/iplist/part-00051
-rw-rw-rw-   1 training supergroup     101134 2019-04-14 21:39 /loudacre/iplist/part-00052
-rw-rw-rw-   1 training supergroup      98024 2019-04-14 21:39 /loudacre/iplist/part-00053
-rw-rw-rw-   1 training supergroup     101902 2019-04-14 21:39 /loudacre/iplist/part-00054
-rw-rw-rw-   1 training supergroup      91723 2019-04-14 21:39 /loudacre/iplist/part-00055
-rw-rw-rw-   1 training supergroup     103461 2019-04-14 21:39 /loudacre/iplist/part-00056
-rw-rw-rw-   1 training supergroup      92419 2019-04-14 21:39 /loudacre/iplist/part-00057
-rw-rw-rw-   1 training supergroup      91577 2019-04-14 21:39 /loudacre/iplist/part-00058
-rw-rw-rw-   1 training supergroup      98231 2019-04-14 21:39 /loudacre/iplist/part-00059
-rw-rw-rw-   1 training supergroup     102310 2019-04-14 21:39 /loudacre/iplist/part-00060
-rw-rw-rw-   1 training supergroup      92409 2019-04-14 21:39 /loudacre/iplist/part-00061
-rw-rw-rw-   1 training supergroup      95437 2019-04-14 21:39 /loudacre/iplist/part-00062
-rw-rw-rw-   1 training supergroup     102634 2019-04-14 21:39 /loudacre/iplist/part-00063
-rw-rw-rw-   1 training supergroup      92643 2019-04-14 21:39 /loudacre/iplist/part-00064
-rw-rw-rw-   1 training supergroup     102446 2019-04-14 21:39 /loudacre/iplist/part-00065
-rw-rw-rw-   1 training supergroup      95294 2019-04-14 21:39 /loudacre/iplist/part-00066
-rw-rw-rw-   1 training supergroup     100129 2019-04-14 21:39 /loudacre/iplist/part-00067
-rw-rw-rw-   1 training supergroup      92308 2019-04-14 21:39 /loudacre/iplist/part-00068
-rw-rw-rw-   1 training supergroup     100248 2019-04-14 21:39 /loudacre/iplist/part-00069
-rw-rw-rw-   1 training supergroup      99069 2019-04-14 21:39 /loudacre/iplist/part-00070
-rw-rw-rw-   1 training supergroup      92053 2019-04-14 21:39 /loudacre/iplist/part-00071
-rw-rw-rw-   1 training supergroup      91013 2019-04-14 21:39 /loudacre/iplist/part-00072
-rw-rw-rw-   1 training supergroup      92198 2019-04-14 21:39 /loudacre/iplist/part-00073
-rw-rw-rw-   1 training supergroup      92462 2019-04-14 21:39 /loudacre/iplist/part-00074
-rw-rw-rw-   1 training supergroup     102237 2019-04-14 21:39 /loudacre/iplist/part-00075
-rw-rw-rw-   1 training supergroup      95082 2019-04-14 21:39 /loudacre/iplist/part-00076
-rw-rw-rw-   1 training supergroup      95717 2019-04-14 21:39 /loudacre/iplist/part-00077
-rw-rw-rw-   1 training supergroup     103245 2019-04-14 21:39 /loudacre/iplist/part-00078
-rw-rw-rw-   1 training supergroup     100830 2019-04-14 21:39 /loudacre/iplist/part-00079
-rw-rw-rw-   1 training supergroup     101057 2019-04-14 21:39 /loudacre/iplist/part-00080
-rw-rw-rw-   1 training supergroup      97285 2019-04-14 21:39 /loudacre/iplist/part-00081
-rw-rw-rw-   1 training supergroup      97191 2019-04-14 21:39 /loudacre/iplist/part-00082
-rw-rw-rw-   1 training supergroup      96072 2019-04-14 21:39 /loudacre/iplist/part-00083
-rw-rw-rw-   1 training supergroup      97886 2019-04-14 21:39 /loudacre/iplist/part-00084
-rw-rw-rw-   1 training supergroup     103754 2019-04-14 21:39 /loudacre/iplist/part-00085
-rw-rw-rw-   1 training supergroup      95227 2019-04-14 21:39 /loudacre/iplist/part-00086
-rw-rw-rw-   1 training supergroup     100376 2019-04-14 21:39 /loudacre/iplist/part-00087
-rw-rw-rw-   1 training supergroup      97188 2019-04-14 21:39 /loudacre/iplist/part-00088
-rw-rw-rw-   1 training supergroup      96733 2019-04-14 21:39 /loudacre/iplist/part-00089
-rw-rw-rw-   1 training supergroup      92797 2019-04-14 21:39 /loudacre/iplist/part-00090
-rw-rw-rw-   1 training supergroup     101908 2019-04-14 21:39 /loudacre/iplist/part-00091
-rw-rw-rw-   1 training supergroup      94873 2019-04-14 21:39 /loudacre/iplist/part-00092
-rw-rw-rw-   1 training supergroup     101805 2019-04-14 21:39 /loudacre/iplist/part-00093
-rw-rw-rw-   1 training supergroup     100864 2019-04-14 21:39 /loudacre/iplist/part-00094
-rw-rw-rw-   1 training supergroup      98878 2019-04-14 21:39 /loudacre/iplist/part-00095
-rw-rw-rw-   1 training supergroup     104473 2019-04-14 21:39 /loudacre/iplist/part-00096
-rw-rw-rw-   1 training supergroup      92999 2019-04-14 21:39 /loudacre/iplist/part-00097
-rw-rw-rw-   1 training supergroup      98649 2019-04-14 21:39 /loudacre/iplist/part-00098
-rw-rw-rw-   1 training supergroup      93301 2019-04-14 21:39 /loudacre/iplist/part-00099
-rw-rw-rw-   1 training supergroup      99921 2019-04-14 21:39 /loudacre/iplist/part-00100
-rw-rw-rw-   1 training supergroup      92481 2019-04-14 21:39 /loudacre/iplist/part-00101
-rw-rw-rw-   1 training supergroup      99558 2019-04-14 21:39 /loudacre/iplist/part-00102
-rw-rw-rw-   1 training supergroup      95850 2019-04-14 21:39 /loudacre/iplist/part-00103
-rw-rw-rw-   1 training supergroup     100134 2019-04-14 21:39 /loudacre/iplist/part-00104
-rw-rw-rw-   1 training supergroup      96173 2019-04-14 21:39 /loudacre/iplist/part-00105
-rw-rw-rw-   1 training supergroup     101061 2019-04-14 21:39 /loudacre/iplist/part-00106
-rw-rw-rw-   1 training supergroup      93851 2019-04-14 21:39 /loudacre/iplist/part-00107
-rw-rw-rw-   1 training supergroup      99674 2019-04-14 21:39 /loudacre/iplist/part-00108
-rw-rw-rw-   1 training supergroup      99583 2019-04-14 21:39 /loudacre/iplist/part-00109
-rw-rw-rw-   1 training supergroup      92949 2019-04-14 21:39 /loudacre/iplist/part-00110
-rw-rw-rw-   1 training supergroup     104021 2019-04-14 21:39 /loudacre/iplist/part-00111
-rw-rw-rw-   1 training supergroup      95607 2019-04-14 21:39 /loudacre/iplist/part-00112
-rw-rw-rw-   1 training supergroup     103379 2019-04-14 21:39 /loudacre/iplist/part-00113
-rw-rw-rw-   1 training supergroup     103024 2019-04-14 21:39 /loudacre/iplist/part-00114
-rw-rw-rw-   1 training supergroup      97907 2019-04-14 21:39 /loudacre/iplist/part-00115
-rw-rw-rw-   1 training supergroup      97361 2019-04-14 21:39 /loudacre/iplist/part-00116
-rw-rw-rw-   1 training supergroup      92279 2019-04-14 21:39 /loudacre/iplist/part-00117
-rw-rw-rw-   1 training supergroup      93950 2019-04-14 21:39 /loudacre/iplist/part-00118
-rw-rw-rw-   1 training supergroup     101163 2019-04-14 21:39 /loudacre/iplist/part-00119
-rw-rw-rw-   1 training supergroup      99148 2019-04-14 21:39 /loudacre/iplist/part-00120
-rw-rw-rw-   1 training supergroup      94291 2019-04-14 21:39 /loudacre/iplist/part-00121
-rw-rw-rw-   1 training supergroup      98946 2019-04-14 21:39 /loudacre/iplist/part-00122
-rw-rw-rw-   1 training supergroup      94118 2019-04-14 21:39 /loudacre/iplist/part-00123
-rw-rw-rw-   1 training supergroup      98767 2019-04-14 21:39 /loudacre/iplist/part-00124
-rw-rw-rw-   1 training supergroup      91600 2019-04-14 21:39 /loudacre/iplist/part-00125
-rw-rw-rw-   1 training supergroup      91759 2019-04-14 21:39 /loudacre/iplist/part-00126
-rw-rw-rw-   1 training supergroup      96115 2019-04-14 21:39 /loudacre/iplist/part-00127
-rw-rw-rw-   1 training supergroup      95529 2019-04-14 21:39 /loudacre/iplist/part-00128
-rw-rw-rw-   1 training supergroup     104761 2019-04-14 21:39 /loudacre/iplist/part-00129
-rw-rw-rw-   1 training supergroup      92029 2019-04-14 21:39 /loudacre/iplist/part-00130
-rw-rw-rw-   1 training supergroup     100009 2019-04-14 21:39 /loudacre/iplist/part-00131
-rw-rw-rw-   1 training supergroup     101173 2019-04-14 21:39 /loudacre/iplist/part-00132
-rw-rw-rw-   1 training supergroup     100757 2019-04-14 21:39 /loudacre/iplist/part-00133
-rw-rw-rw-   1 training supergroup     103584 2019-04-14 21:39 /loudacre/iplist/part-00134
-rw-rw-rw-   1 training supergroup     102016 2019-04-14 21:39 /loudacre/iplist/part-00135
-rw-rw-rw-   1 training supergroup      95823 2019-04-14 21:39 /loudacre/iplist/part-00136
-rw-rw-rw-   1 training supergroup     101843 2019-04-14 21:39 /loudacre/iplist/part-00137
-rw-rw-rw-   1 training supergroup     103870 2019-04-14 21:39 /loudacre/iplist/part-00138
-rw-rw-rw-   1 training supergroup      99117 2019-04-14 21:39 /loudacre/iplist/part-00139
-rw-rw-rw-   1 training supergroup     100483 2019-04-14 21:39 /loudacre/iplist/part-00140
-rw-rw-rw-   1 training supergroup      94534 2019-04-14 21:39 /loudacre/iplist/part-00141
-rw-rw-rw-   1 training supergroup      95677 2019-04-14 21:39 /loudacre/iplist/part-00142
-rw-rw-rw-   1 training supergroup      94979 2019-04-14 21:39 /loudacre/iplist/part-00143
-rw-rw-rw-   1 training supergroup     100460 2019-04-14 21:39 /loudacre/iplist/part-00144
-rw-rw-rw-   1 training supergroup      93876 2019-04-14 21:39 /loudacre/iplist/part-00145
-rw-rw-rw-   1 training supergroup      97586 2019-04-14 21:39 /loudacre/iplist/part-00146
-rw-rw-rw-   1 training supergroup     100628 2019-04-14 21:39 /loudacre/iplist/part-00147
-rw-rw-rw-   1 training supergroup      98568 2019-04-14 21:39 /loudacre/iplist/part-00148
-rw-rw-rw-   1 training supergroup      99519 2019-04-14 21:39 /loudacre/iplist/part-00149
-rw-rw-rw-   1 training supergroup     103016 2019-04-14 21:39 /loudacre/iplist/part-00150
-rw-rw-rw-   1 training supergroup     103437 2019-04-14 21:39 /loudacre/iplist/part-00151
-rw-rw-rw-   1 training supergroup     101090 2019-04-14 21:39 /loudacre/iplist/part-00152
-rw-rw-rw-   1 training supergroup      95280 2019-04-14 21:39 /loudacre/iplist/part-00153
-rw-rw-rw-   1 training supergroup      99609 2019-04-14 21:39 /loudacre/iplist/part-00154
-rw-rw-rw-   1 training supergroup      93732 2019-04-14 21:39 /loudacre/iplist/part-00155
-rw-rw-rw-   1 training supergroup      96405 2019-04-14 21:39 /loudacre/iplist/part-00156
-rw-rw-rw-   1 training supergroup      93427 2019-04-14 21:39 /loudacre/iplist/part-00157
-rw-rw-rw-   1 training supergroup      93610 2019-04-14 21:39 /loudacre/iplist/part-00158
-rw-rw-rw-   1 training supergroup      96596 2019-04-14 21:39 /loudacre/iplist/part-00159
-rw-rw-rw-   1 training supergroup      99904 2019-04-14 21:39 /loudacre/iplist/part-00160
-rw-rw-rw-   1 training supergroup      95665 2019-04-14 21:39 /loudacre/iplist/part-00161
-rw-rw-rw-   1 training supergroup     103957 2019-04-14 21:39 /loudacre/iplist/part-00162
-rw-rw-rw-   1 training supergroup      96046 2019-04-14 21:39 /loudacre/iplist/part-00163
-rw-rw-rw-   1 training supergroup     101422 2019-04-14 21:39 /loudacre/iplist/part-00164
-rw-rw-rw-   1 training supergroup     102844 2019-04-14 21:39 /loudacre/iplist/part-00165
-rw-rw-rw-   1 training supergroup      92003 2019-04-14 21:39 /loudacre/iplist/part-00166
-rw-rw-rw-   1 training supergroup      94551 2019-04-14 21:39 /loudacre/iplist/part-00167
-rw-rw-rw-   1 training supergroup      94874 2019-04-14 21:39 /loudacre/iplist/part-00168
-rw-rw-rw-   1 training supergroup      92896 2019-04-14 21:39 /loudacre/iplist/part-00169
-rw-rw-rw-   1 training supergroup      91539 2019-04-14 21:39 /loudacre/iplist/part-00170
-rw-rw-rw-   1 training supergroup      94904 2019-04-14 21:39 /loudacre/iplist/part-00171
-rw-rw-rw-   1 training supergroup      95772 2019-04-14 21:39 /loudacre/iplist/part-00172
-rw-rw-rw-   1 training supergroup     103791 2019-04-14 21:39 /loudacre/iplist/part-00173
-rw-rw-rw-   1 training supergroup      94535 2019-04-14 21:39 /loudacre/iplist/part-00174
-rw-rw-rw-   1 training supergroup      93885 2019-04-14 21:39 /loudacre/iplist/part-00175
-rw-rw-rw-   1 training supergroup     103140 2019-04-14 21:39 /loudacre/iplist/part-00176
-rw-rw-rw-   1 training supergroup      98011 2019-04-14 21:39 /loudacre/iplist/part-00177
-rw-rw-rw-   1 training supergroup      98670 2019-04-14 21:39 /loudacre/iplist/part-00178
-rw-rw-rw-   1 training supergroup      93761 2019-04-14 21:39 /loudacre/iplist/part-00179
-rw-rw-rw-   1 training supergroup      93654 2019-04-14 21:39 /loudacre/iplist/part-00180
-rw-rw-rw-   1 training supergroup     101291 2019-04-14 21:40 /loudacre/iplist/part-00181
```

**3. Bonus Exercise**

Use RDD transformations to create a dataset consisting of the IP
address and corresponding user ID for each request for an HTML file.
(Disregard requests for other file types). The user ID is the third field in
each log file line.

<Example>
165.32.101.206/8
100.219.90.44/102
182.4.148.56/173
246.241.6.175/45395
175.223.172.207/4115


```
> bonusRDD = logsRDD.map(lambda line: line.split(' ')[0] + '/' + line.split(' ')[2])
> bonusRDD.take(2)
[u'3.94.78.5/69827', u'3.94.78.5/69827']
```

