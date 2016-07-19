# MapReduce-简介
[英文原文](http://www.tutorialspoint.com/map_reduce/map_reduce_introduction.htm)

MapReduce是一个软件框架，基于它编写出来的应用可以以并行计算的方式在多个计算机节点上处理大量的数据。MapReduce为分析海量复杂的数据提供了分析能力。

###什么是大数据？
大数据是大量数据的集合，数据量之大以至于用传统的计算方法无法处理如此庞大的数据。比如，Facebook和Youtube在日常中搜集和管理的大量数据就属于大数据的范畴。大数据不仅仅是指数据的规模和数量庞大，它通常还包括以下一个或多个方面：处理数据的速度、数据的种类、体积以及复杂度。

###为什么是MapReduce?
传统的企业系统有一个中央服务器来保存和处理数据。下图为传统的企业系统的原理图。传统的模型不适合处理海量的数据，也不适用于标准的数据库。而且，中央处理系统在同时处理多个文件的时候遇到了瓶颈。
![](..\images\traditional_enterprise_system_view.jpg)
Google使用了一个叫MapReduce的算法解决了这个瓶颈。MapReduce把一个任务拆分成了多个小任务，并把子任务分配到多台计算机上进行工作。最终，每台计算机上的计算结果会被搜集起来并合并成最终的结果。
![](..\images\centralized_system.jpg)

###MapReduce是如何工作的？
MapReduce算法包含两部分重要的任务：Map和Reduce.  

* Map任务把一个数据集转化成另一个数据集，单独的元素会被拆分成键值对(key-value pairs).
* Reduce任务把Map的输出作为输入，把这些键值对的数据合并成一个更小的键值对数据集.

让我们通过下图了解一下MapReduce每个阶段的工作，并理解他们的重要性。  
![](..\images\phases.jpg)

* **Input Phase** - 在本阶段我们使用一个Record Reader对输入文件中的每一条数据转换为键值对的形式，并把这些处理好的数据发送给Mapper。
* **Map** - Map是是用户自定义的一个函数，此函数接收一系列的键值对数据并对它们进行处理，最后生成0个或多个键值对数据。
* **Intermediate Keys** - 由mapper生成的键值对数据被称为中间状态的键值对。
* **Shuffle and Sort** - Reducer任务通常以Shuffle（搅动）和Sort（排序）开始。程序把分好组的键值对数据下载到本机，Reducer会在本机进行运行。这些独立的键值对数据会按照键值进行排序并形成一个较大的数据序列，数据序列中键值相等的键值对数据会被分在相同的一组，这样易于在Reducer任务中进行迭代操作。
* **Reducer** - Reducer任务把分好组的键值对数据作为输入，并且对每一个键值对都执行Reducer函数。在这个阶段，程序会以不同的方式对数据进行合并、筛选。一旦执行完毕，Reducer会生成0个或多个键值对数据，并提供给最后一个处理步骤。
* **Output Phase** - 在输出阶段，通过record writer把从Reducer函数输出的键值对数据按照一定的格式写入到文件中。

让我们通过下图来进一步了解Map和Reduce这两个任务是如何工作的。
![](..\images\mapreduce_work.jpg)

###MapReduce例子
让我们以一个真实的例子来理解MapReduce的威力。Twitter每天都会收到50亿条（有那么多？）推特，约每秒3000条。下图展示了Twitter是如何利用MapReduce来管理这些数据的。
![](..\images\mapreduce_example.jpg)
从上述插图中我们可以看到MapReduce执行了以下这些行为 -

* **Tokenize** - 处理器把推文以键值对的形式存放在maps中。
* **Filter** - 把不想要的数据从maps中剔除，把筛选好的数据以键值对的形式保存。
* **Count** - 对每个单词生成一个计数器。
* **Aggregate Counter** - Prepares an aggregate of similar counter values into small manageable units.
