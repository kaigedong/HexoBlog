---
title: 无root安装UCSC genome browser
date: 2018-12-09 13:25:32
tags:
---



<!--more-->

+ `rsync`查看大小

  ```
  rsync -hna --stats rsync://hgdownload.soe.ucsc.edu/gbdb/ | egrep "Number of files:|total size is"
  ```




## 参考

+ https://genome.ucsc.edu/goldenpath/help/mirrorManual.html
+ http://oliverelliott.org/article/bioinformatics/tut_genomebrowser/
+ http://blog.sciencenet.cn/blog-723745-569746.html
+ http://emb-bioinfo.fsaa.ulaval.ca/bioinfo/html/admin/mirror.html
+ file:///Users/kaigedong/Desktop/UCSC%20Genome%20Browser%E5%AE%89%E8%A3%85.pdf
+ [Installation of a UCSC Genome Browser on a local machine ("mirror")](http://genome.ucsc.edu/goldenPath/help/mirror.html#manual-installation-instructions)
+  [Genome Browser source code](https://genome-store.ucsc.edu/)