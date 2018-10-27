---
layout: default
title: Home
---

I'm a PhD student in [Brown CS](https://cs.brown.edu).  

My sensei are [Stan Zdonik](https://cs.brown.edu/~sbz/), [Seny Kamara](https://cs.brown.edu/~seny/), and [Tarik Moataz](https://cs.brown.edu/~tmoataz/). I also collaborate with Ugur Cetintemel, Carsten Binnig and Tim Kraska in the Database Group, [Emanuel Zgraggen](http://emanuelzgraggen.com/) in the Graphics Group, and Eli Upfal and Lorenzo De Stefani in the Theory Group.

- [Curriculum vitae](https://zheguang.github.io/cv/cv.pdf)
- [Google scholar](https://goo.gl/DR8pSa)
- [DBLP](http://dblp.uni-trier.de/pers/hd/z/Zhao:Zheguang)
- [LinkedIn](https://www.linkedin.com/in/samuelzhao)
- [Github](https://github.com/zheguang)


### What's on?
- I'm building encrypted data systems that are provably secure at [Sifr Systems](http://sifrsystems.com) with some [fantastic people](http://sifrsystems.com/#team). We're much more secure than [CryptDB](https://css.csail.mit.edu/cryptdb/).
- I maintain an external advisory role for [Blockchain Warehouse](https://www.blockchainwarehouse.com).  This gig gets me to talk about blockchain for fun and profit.
- I provide consultation on machine learning at [Critical Future](http://www.criticalfutureglobal.com).


### Some old gigs

- Microsoft Research & AI, 2017
- Intel Labs, 2015
- Hadapt (Acquired by Teradata), 2013-14
- WalmartLabs, 2012

### Some open-source projects

- [Searchable encryption for mobile messaging in Signal](https://github.com/encryptedsystems/Searchable-Signal-Android)
- [Macau: statistical hypothesis testing based on resampling](https://github.com/zheguang/macau)
- [Machine learning algorithms in Spark](https://github.com/zheguang/spark-study/tree/master/study/src/main/scala/edu/brown/cs/sparkstudy)
- [Consistency control for machine learning algorithms](https://github.com/zheguang/babel)
- [R-tree in Rust](https://github.com/zheguang/rtree)
- [Spark performance analysis tool](https://github.com/zheguang/spark-perftool)
- [VoltDB on non-volatile memory](https://github.com/zheguang/voltdb)

Research
========

I am interested in the theories and designs of big data systems that are intelligent and safe. My research spans a broad area covering cryptography, data science/machine learning, and big data systems.

In this spirit I have dabbled in constraint learning for puzzle-solving AI, false-discovery control in data science, approximate data structures for visualization, database design on hybrid memory, consistency control for stochastic machine learning algorithms, and searchable encryption on mobile text messaging.

Security and Cryptography
-------------------------

**[Signal Search](http://esl.cs.brown.edu/blog/signal)**.   
Engelman, Kamara, Moataz and Zhao.   
April 2017.
[[Software](https://github.com/encryptedsystems/Searchable-Signal-Android)]

Data Science
------------
**[Behavior of Large Random Graph](https://zheguang.github.io/research/random_graph.pdf).**   
Zhao, supervised by Prof. Paul Dupius.   
Randomized Algorithms for Counting, Integration and Optimzation, Brown University, April 2017.


**[Investigating the Effect of the Multiple Comparisons Problem in Visual Analysis](https://zheguang.github.io/research/risk-chi.pdf)**.   
Zgraggen, Zhao, Zeleznik, and Kraska.   
[CHI][1], April 2018.
[[Video](http://emanuelzgraggen.com/assets/video/risk.mp4)], [[Software](https://github.com/zheguang/macau)], [[Review on Medium](https://medium.com/hci-design-at-uw/multiple-perspectives-on-the-multiple-comparisons-problem-in-visual-analysis-df7493818bbd)]

**[Controlling False Discoveries During Interactive Data Exploration](https://zheguang.github.io/research/risk-sigmod.pdf)**.   
Zhao, De Stefani, Zgraggen, Binnig, Upfal and Kraska.   
[SIGMOD][2], May 2017.
[[Review on Medium](https://medium.com/hci-design-at-uw/multiple-perspectives-on-the-multiple-comparisons-problem-in-visual-analysis-df7493818bbd)]

**[Safe Visual Data Exploration](https://zheguang.github.io/research/risk-sigmod-demo.pdf)**.  
Zhao, Zgraggen, De Stefani, Binnig, Upfal and Kraska.   
[SIGMOD Demo][2], May 2017.

**[Towards Sustainable Insights, or Why Polygamy is Bad for You](https://zheguang.github.io/research/risk-cidr.pdf)**.   
Binnig, De Stefani, Kraska, Upfal, Zgraggen and Zhao.   
[CIDR][3], January 2017.
[[Code](https://github.com/zheguang/rand-db)], [[Review by Adrian Coyler](https://blog.acolyer.org/2017/01/25/toward-sustainable-insights-or-why-polygamy-is-bad-for-you/)]

**[Towards a Benchmark for Interactive Data Exploration](https://zheguang.github.io/research/ide-bench.pdf).**   
Eichmann, Zgraggen, Zhao, Binnig, Kraska.   
[IEEE Data Engineering Bulletin][4], 2016.

**[VisTrees: Fast Indexes for Interactive Data Exploration.](https://zheguang.github.io/research/vistree.pdf)**   
El-Hindi, Zhao, Binnig and Kraska.   
[SIGMOD HILDA][5], June 2016.

Systems
-------

**[Bridging the Gap between HPC and Big Data frameworks](https://zheguang.github.io/research/hpc-big-data.pdf)**.   
Anderson, Smith, Sundaram, Capota, Zhao, Dulloor, Satish and Willke.   
[VLDB][6], 2017.
[[Spark performance tool](https://github.com/zheguang/spark-perftool)]
[[Machine learning code](https://github.com/zheguang/spark-study/tree/master/study/src/main/scala/edu/brown/cs/sparkstudy)]

**[Larger-than-memory Data Management on Modern Storage Hardware for In-memory OLTP Database Systems.](https://zheguang.github.io/research/nvm-anticache.pdf)**   
Ma, Arulraj, Zhao, Pavlo, Dulloor, Giardino, Parkhurst, Gardner, Doshi and Zdonik.   
[SIGMOD DaMoN][7], June 2016.

**[Data Tiering in Heterogeneous Memory Systems.](https://zheguang.github.io/research/nvm-data-tiering.pdf)**   
Dulloor, Roy, Zhao, Sundaram, Satish, Sankaran, Jackson and Schwan.   
[EuroSys][8], April 2016.
[[Code](https://github.com/zheguang/voltdb/tree/sam-redo-tag)]

[1]: https://chi2018.acm.org/
[2]: http://sigmod2017.org/
[3]: http://cidrdb.org/cidr2017/index.html
[4]: http://sites.computer.org/debull/A16dec/issue1.htm
[5]: http://hilda.io/2016/
[6]: http://www.vldb.org/2017/
[7]: http://daslab.seas.harvard.edu/damon2016/
[8]: http://eurosys16.doc.ic.ac.uk/
