class: middle, center

# 大数据编程模型和使用技巧

## 机器学习与图计算

陈一帅

[yschen@bjtu.edu.cn](mailto:yschen@bjtu.edu.cn)

.footnote[网络智能实验室]

北京交通大学电子信息工程学院

---

# 内容

- .red[机器学习]
- 图计算

---

# 大数据机器学习

- 机器学习已成为云计算应用的核心
- 机器学习最近取得的重大突破来自几个方面
  - 大数据
  - 算法进步
  - 更快计算平台（GPU）

???

Machine learning has become central to applications of cloud computing.

What has enabled these breakthroughs has been a convergence of the availability of big data plus algorithmic advances and faster computers that have made it possible to train even deep neural networks.

---

# 四个基本概念：DataFrame

- 以有效的方式保存矢量和其他结构化数据类型
- 与 Pandas DataFrames 类似，共享一些操作
- 它们是分布式对象，是执行图的一部分
- 可将它们转换为 Pandas DataFrame，就可以用 Python 访问它们

???

four basic concepts.

• DataFrames are containers created from Spark RDDs to hold vectors and other structured types in a manner that permits efficient execution [45].

Spark DataFrames are similar to Pandas DataFrames and share some operations.

They are distributed objects that are part of the execution graph.

You can convert them to Pandas DataFrames to access them in Python.

---

# 四个基本概念：Transformers

- 将一个 DataFrame 转换为另一个 DataFrame 的运算符
- 它们是执行图上的节点，因此在执行整个图之前不会对其进行评估
- 如
  - 将文本文档转换为向量
  - 将 DataFrame 的列从一种形式转换为另一种形式
  - 将 DataFrame 拆分为子集

???

• Transformers are operators that convert one DataFrame to another.

Since they are nodes on the execution graph, they are not evaluated until the entire graph is executed.

如
turn text documents into vectors of real numbers
convert columns of a DataFrame from one form to another
split DataFrames into subsets.

---

# 四个基本概念：Estimators

- 封装 ML 和其他算法
- fit()方法将 DataFrame 和参数传递给学习算法以创建模型

???

该模型现在表示为“变形金刚”。

通过将向量投影到主成分向量上，将其转换为 n 个语法生成器，这些生成器获取文本文档并返回 n 个连续单词的字符串。

分类
聚类

LR 的例子（很不错）

• Estimators encapsulate ML and other algorithms.

As we describe in the following, you can use the fit(...) method to pass a DataFrame and parameters to a learning algorithm to create a model.

The model is now represented as a Transformer.

transform vectors by projecting them onto principal component vectors, to n-gram generators that take text documents and return strings of n consecutive words.

分类
聚类

LR 的例子（很不错）

---

# Pipeline

- 通常是线性的，但也可以是有向无环图
- 链接 Transformers 和 Estimators，指定一个 ML 工作流
- 用 fit() 训练完估算器后，Pipeline 就是一个模型，具有 transform() 方法，可对新案例进行预测

???

• A Pipeline (usually linear, but can be a directed acyclic graph) links Transformers and Estimators to specify an ML workflow.

Pipelines inherit the fit(...) method from the contained estimator. Once the estimator is trained, the pipeline is a model and has a transform(...) method that can be used to push new cases through the pipeline to make predictions.

---

# Spark MLib

- 步骤 1
  - 输入数据分为两个子集：训练数据与测试数据
  - 在进入计算或学习引擎之前，两者都存储在数据存储器中
- 步骤 2
  - 数据预处理，例如过滤，挖掘，数据聚合，特征提取，模式识别以及某些转换操作
- 步骤 3
  - 使用云计算和存储资源的学习引擎
  - 包括数据清理，模型训练以及在监督下向模型开发的转变。
- 步骤 4
  - 学习模型的构建，适应环境满足预测或分类等学习目标的环境问题
- 步骤 5
  - 通过制定决策或预测进行的训练和测试阶段
  - 通过反馈进行培训，以提高模型性能。然后，输出预测结果（如果令人满意）

???

“据报道，MLlib 比 Apache Mahout 使用的基于磁盘的实现快 9 倍。这是基于 MLlib 开发人员在 Mahout 本身获得 Spark 接口之前针对交替最小二乘（ALS）实现而完成的基准测试。

“ Spark ML 标准化了 ML 算法的 API，使将多种算法组合到单个管道或工作流程中变得更加容易。我们介绍以下使用用于 ML 的 Spark API 的关键概念：
ML 数据集：Spark ML 使用 Spark SQL 中的 SchemaRDD 作为数据集，它可以保存各种数据类型。例如，一个数据集具有存储文本，特征向量，真实标签和预测的不同列。”

“变压器：变压器是一种算法，可以将一个 SchemaRDD 转换为另一个。例如，ML 模型将具有特征的 RDD 转换为具有预测的另一个 RDD。
估计器：该算法可以适合 SchemaRDD 来生成变压器。例如，学习算法在数据集上训练并生成预测模型。
管道：管道将多个变换器和估计器链接在一起，以指定 ML 工作流。
参数：现在所有的变换器和估计器在指定参数时共享一个公共 API。”

- 机器学习 pipeline

“Step 1: Input data are divided into two subsets: the training data vs. the test data. Both are stored in the data storage before entering the compute or learning engine.
Step 2: This stage involves data preprocessing operations like filtering, mining, data aggregation, feature extraction, pattern recognition, and some transformation operations.
Step 3: This stage is the learning engine using cloud compute and storage resources. Major operations include data cleaning, model training, and transformation toward the model development under supervision.
Step 4: This stage is aimed at learning model construction and fitting with the problem of environment meeting the learning objectives for prediction or classification, etc.
Step 5: This is the training and testing stage by making decisions or predictions. The training is carried out with feedback to improve the model performance. Then, output the predictive results, if satisfactory.”

“MLlib was reported nine times faster than the disk-based implementation used by Apache Mahout. This was based on benchmarks completed by the MLlib developers against the alternating least squares (ALS) implementations before Mahout itself gained a Spark interface.”

“Spark ML standardizes APIs for ML algorithms to make it easier to combine multiple algorithms into a single pipeline or workflow. We introduce below key concepts of using the Spark APIs for ML:
ML data set: Spark ML uses the SchemaRDD from Spark SQL as a data set which can hold a variety of data types. For example, a data set has different columns storing text, feature vectors, true labels, and predictions.”

“Transformer: A transformer is an algorithm which can transform one SchemaRDD into another. For example, an ML model transforms an RDD with features into another RDD with predictions.
Estimator: This algorithm can be fit on a SchemaRDD to produce a transformer. For example, a learning algorithm trains on a data set and produces a prediction model.
Pipeline: A pipeline chains multiple transformers and estimators together to specify an ML workflow.
Parameter: All transformers and estimators now share a common API in specifying parameters.”

DAG pipeline

“管道的阶段被指定为有序数组。下面给出的示例全部用于线性管道，即，其中每个阶段使用前一阶段产生的数据的管道。只要数据流图形成 DAG，就可以创建非线性管道。当前根据每个阶段的输入和输出列名称隐式指定该图。如果管道形成 DAG，则必须按拓扑顺序指定阶段。由于管道可以对具有各种类型的数据集进行操作，因此它们不能使用编译时类型检查。管道和管道模型会在实际运行管道之前进行运行时检查。这种检查是使用数据集架构完成的，数据集架构是 SchemaRDD 中各列的数据类型的描述。”

“A pipeline’s stage is specified as an ordered array. The examples given below are all for linear pipelines, i.e., pipelines in which each stage uses data produced by the previous stage. It is possible to create nonlinear pipelines as long as the data flow graph forms a DAG. This graph is currently specified implicitly based on the input and output column names of each stage. If the pipeline forms a DAG, then the stages must be specified in topological order. Since pipelines can operate on data sets with varied types, they cannot use compile-time type checking. Pipelines and pipeline models instead do runtime checking before actually running the pipeline. This type of checking is done using the data set schema, a description of the data types of columns in the SchemaRDD.”

---

# 交叉验证

- 机器学习中的一项重要任务是模型选择，或使用数据为给定任务找到最佳模型或参数。这也称为 Tunning
  - Pipeline 可以轻松地一次调整整个 Pipeline，不必分别调整其中的每个元素，简化了模型选择
  - MLlib 支持使用 CrossValidator 类进行模型选择，该类具有一个估计器，一组 ParamMap 和一个评估器

---

# 交叉验证

- CrossValidator 首先将数据集划分为一组 folds，它们将被用作单独的训练和测试数据集
  - 如 k = 3 folds，就会生成 3 对（训练，测试）数据集对，每对使用三分之二的数据用于训练，另外三分之一的数据用于测试。
- CrossValidator 遍历 ParamMaps 集。对于每个 ParamMap，它训练给定的估算器并对其进行评估，选择产生最佳评估指标的 ParamMap 作为最佳模型
- 最后，CrossValidator 使用最佳的 ParamMap 和整个数据集来训练最终的估算器

???

“An important task in ML is model selection, or using data to find the best model or parameters for a given task. This is also called tuning. Pipelines facilitate model selection by making it easy to tune an entire pipeline at once, rather than tuning each element in the pipeline separately.
Currently, Spark MLlib supports model selection using the CrossValidator class, which takes an estimator, a set of ParamMaps, and an evaluator. CrossValidator begins by splitting the data set into a set of folds, which are used as separate training and test data sets, e.g., with k = 3 folds, CrossValidator will generate 3 (training, test) data set pairs, each of which uses two thirds of the data for training and one third for testing. CrossValidator iterates through the set of ParamMaps. For each ParamMap, it trains the given estimator and evaluates it using the given evaluator. The ParamMap which produces the best evaluation metric is selected as the best model. CrossValidator finally fits the estimator using the best ParamMap and the entire data set.”

---

# 示例

- 创建 DataFrame，包含由矢量表示的标签和多个特征

```py
df = sqlContext.createDataFrame
    (data, [“label”, “features”])
```

- 设置算法参数。在这里，我们将 LR 的迭代次数设为 10

```py
lr = LogisticRegression(maxIter = 10)
```

---

# 示例

- 从数据中训练模型

```py
model = lr.fit(df)
```

- 将数据集送入训练好的模型，预测每个点的标签，显示结果

```py
model.transform(df).show()
```

???

“This DataFrame contains the label and # features represented by a vector.

df = sqlContext.createDataFrame (data, [“label”, “features”])

Set parameters for the algorithm. Here, we limit the number of iterations to 10.

lr = LogisticRegression(maxIter = 10)

Fit the model to the data.

model = lr.fit(df)

Given a data set, predict each point’s label, and show the results.

model.transform(df).show()

---

# 内容

- 机器学习
- .red[图计算]

---

# Spark GraphX

- Spark Core 支持的分布式图计算框架
- 提供了表达图形计算的 API，可对 Pregel 抽象进行建模
- 为这种抽象提供了优化的运行时支持。”
- 将提取，转换，加载（ETL）函数，探索性分析和迭代图计算统一
- 能够对 RDD 进行有效的转换和图连接
- 用户可以使用 Pregel API 编写自定义的迭代图算法

???

“GraphX is a distributed graph processing framework supported by Spark Core. It provides an API for expressing graph computation that can model the Pregel abstraction. It also provides an optimized runtime support for this abstraction.”

“Spark GraphX unifies the extract, transform, load (ETL) functions, exploratory analysis, and iterative graph computation as a single system. One can view the same data as both graphs and collections. The package supports transform and join on graphs with RDDs efficiently. The user can write custom iterative graph algorithms using Pregel API. ”

---

# GraphX

- 将图计算嵌入分布式数据流框架中
- 将图计算提炼到特定的 join-map-groupBy 数据流模式
- 通过将图计算减少到特定的模式，用户可以确定系统优化的关键路径

---

# 提供的图算法

- PageRank
- 连通图
- 标签传播
- SVD++
- 强连接组件
- 三角形计数

???

“algorithms such as PageRank, connected components, label propagation, SVD++, strongly connected components, and triangle count.
The growing scale and importance of graph data are driven by the development of numerous graph processing systems including Pregel and PowerGraph. By exposing specialized abstractions backed by graph-specific optimizations, these systems can naturally express and efficiently execute iterative graph algorithms like PageRank and community detection on graphs with billions of vertices and edges. The GraphX was built as a library on top of Spark (Figure 8.20). This was done by encoding graphs as collections and then expressing GraphX API on top of standard dataflow operators.”

“GraphX offers a general method to embed graph computation within distributed dataflow frameworks. The system distills graph computation to a specific join—map—groupBy dataflow pattern. By reducing graph computation to a specific pattern, the user can identify the critical path for system optimization. This differs from some general-purpose distributed dataflow frameworks—MapReduce, Spark, Dryad—we have discussed so far. Some rich dataflow operators—map, reduce, groupBy, join—are well suited for analyzing unstructured and tabular data.
In general, directly implementing iterative graph algorithms using dataflow operators is a nontrivial task, often requiring multiple stages of complex joins. Graph processing systems represent graph-structured data as property graphs, which associate user-defined properties with each vertex and edge. The properties can include metadata (e.g., user profiles and time stamps) and program state (e.g., the PageRank of vertices or inferred affinities). property graphs derived from natural phenomena such as social networks and web graphs that are often highly skewed and follow power-law degree distributions with several orders of magnitude more edges than vertices.”

图形处理系统应用了一系列图形划分算法，以最大程度地减少通信和平衡计算。冈萨雷斯等。 [6]。证明了顶点切割分区在许多大型自然图上表现良好。顶点切割分区以最小化每个顶点切割次数的方式将边缘均匀分配给机器。 ”

“图形分析用于图形视图的流水线处理。 ”

“这个图形分析管道消除了学习和支持多个系统或编写数据交换格式和管道以在系统之间移动的需求。管道支持对大型图形进行迭代切片，变换和计算，以及在管道的各个阶段共享数据结构。反馈路径用于改进分析模型。图形计算在性能和可伸缩性方面的收益转化为更紧密的分析反馈循环，从而使工作流程更高效。要采用 GraphX，用户可以访问 Apache Spark 开源项目的网站。”

“Graph processing systems apply a range of graph-partitioning algorithms to minimize communication and balance computation. Gonzalez et al. [6]. demonstrates that vertex-cut partitioning performs well on many large natural graphs. Vertex-cut partitioning evenly assigns edges to machines in a way that minimizes the number of times each vertex is cut. ”

“Graph analytics for pipelined processing of graph views. ”

“This graph analytics pipeline eliminates the need to learn and support multiple systems or write data interchange formats and plumbing to move between systems. The pipeline supports iteratively slice, transform, and compute on large graphs as well as to share data-structures across stages of the pipeline. The feedback path is used to improve the analytic model. The gains in performance and scalability for graph computation translate to a tighter analytics feedback loop and therefore a more efficient work flow. To adopt GraphX, the users go to the website of Apache Spark open-source project.”

图表征

“属性图用于 GraphX 编程。这是一个图形表示模型，在逻辑上表示为一对顶点和边属性集合。顶点集合包含由顶点标识符唯一键入的顶点属性。在 GraphX 系统中，顶点标识符是 64 位整数，可以从外部（例如，用户 ID）或通过将哈希函数应用于顶点属性（例如，页面 URL）派生。边缘集合包含由源和目标顶点标识符键入的边缘属性。图 8.22 显示了分布式图形表示的概念。

图 8.22
分布式图形表示。资料来源：Gonzalez 等人，“ Powergraph：自然图上的分布式图-并行计算”，第 10 届 USENIX 研讨会，OSDI’12，USENIX 协会：17-30。

“图（左侧）表示为顶点和边集合（右侧）。通过应用分区功能（例如 2D 分区）将边缘划分为三个边缘分区。顶点由顶点标识符划分。 GraphX 与这些顶点共同分区，维护着一个路由表，该路由表对每个顶点的边缘分区进行了编码。如果顶点 6 和相邻边沿（用虚线显示）被限制在图形中（例如子图），则通过更新位掩码将它们从相应的集合中删除，从而实现索引重用。

顶点集合由顶点标识符进行哈希分区。为了支持跨顶点集合的频繁联接，顶点存储在每个分区内的本地哈希索引中。边缘集合通过用户定义的分区功能进行水平分区。 GraphX 支持顶点分割分区，从而最大程度地减少了社交网络和网络图等自然图中的通信。

GraphX 编程抽象通过引入一小组专门的图形运算符扩展了 Spark 数据流运算符，如表 8.8 所示。 GraphX 继承了 Spark 的不变性，因此所有图运算符从逻辑上创建新集合，而不是破坏性地修改现有集合。”

“The property graph is used in GraphX programming. This is a graph representation model, which is logically represented as a pair of vertex and edge property collections. The vertex collection contains the vertex properties uniquely keyed by the vertex identifier. In the GraphX system, vertex identifiers are 64-bit integers which may be derived externally (e.g., user ids) or by applying a hash function to the vertex property (e.g., page URL). The edge collection contains the edge properties keyed by the source and destination vertex identifiers. Figure 8.22 shows the concept of distributed graph representation.

Figure 8.22
Distributed graph representation.

Source: Gonzalez et al., “Powergraph: Distributed Graph-Parallel Computation on Natural Graphs,” 10th USENIX Symposium, OSDI’12, USENIX Association: 17–30.”

“The graph (on the left) is represented as a vertex and an edge collection (on the right). The edges are divided into three edge partitions by applying a partition function (e.g., 2D partitioning). The vertices are partitioned by vertex identifiers. Copartitioned with the vertices, GraphX maintains a routing table encoding the edge partitions for each vertex. If vertex 6 and adjacent edges (shown with dotted lines) are restricted from the graph (e.g., by subgraph), they are removed from the corresponding collection by updating the bitmasks thereby enabling index reuse.
The vertex collection is hash-partitioned by the vertex identifiers. To support frequent joins across vertex collections, vertices are stored in a local hash index within each partition. The edge collection is horizontally partitioned by a user-defined partition function. GraphX enables vertex-cut partitioning, which minimizes communication in natural graphs such as social networks and web graphs.
The GraphX programming abstraction extends the Spark dataflow operators by introducing a small set of specialized graph operators, summarized in Table 8.8. GraphX inherits the immutability of Spark and therefore all graph operators logically create new collections rather than destructively modifying existing ones.”

---

# 例：计算用户 PageRank

- Spark GraphX PageRank 的社交网络数据集示例

  - 在 graphx/data/users.txt 中提供了一组用户
  - 在 graphx/data/followers.txt 中提供了一组用户之间的关系

- 将边缘作为图形加载

```scala
val graph = GraphLoader
  .edgeListFile(sc, “graphx/data/followers.txt”)
```

---

# 例：计算用户 PageRank

- 运行 PageRank

```
val ranks = graph.pageRank(0.0001).vertices
```

- Join 用户名和 Rank

```
val users = sc.textFile(“graphx/data/users.txt”)
  .map {line => val fields
    = line.split(“,”) (fields(0).toLong, fields(1))}
val ranksByUsername = users.join(ranks)
  .map {case (id, (username, rank)) => (username, rank)}
```

- 打印结果

```
println(ranksByUsername.collect().mkString(“\n”)),

```

???

“The GraphOps calls these algorithms directly. Spark GraphX provides an example of a social network data set that we can run PageRank. A set of users is given in graphx/data/users.txt, and a set of relationships between users is given in graphx/data/followers.txt. Spark computes the PageRank of each user as follows:

- Load the edges as a graph.

```
val graph = GraphLoader.edgeListFile(sc, “graphx/data/followers.txt”)
```

- Run PageRank

```
val ranks = graph.pageRank(0.0001).vertices
```

- Join the ranks with the usernames

```
val users = sc.textFile(“graphx/data/users.txt”).map {line => val fields = line.split(“,”) (fields(0).toLong, fields(1))}
```

, // Join the ranks with the usernames.

```
val ranksByUsername = users.join(ranks).map {case (id, (username, rank))
=> (username, rank)}
```

// Print the result.”

```
println(ranksByUsername.collect().mkString(“\n”)),

```

---

# mrTriplet

- 图并行计算 mrTriplet 的过程
  - mrTriplets 运算符是三元组视图上 map 和 groupBy 数据流运算符的合成
  - 计算每个顶点用户的较老关注者的数量
  - 用户定义的 map 函数会应用于每个三元组，生成一个值
  - 然后使用用户定义的二元聚合函数在目标顶点将其聚合

```
val graph: Graph[User, Double]
def mapUDF(t: Triplet[User, Double]) =
    if (t.src.age > t.dst.age) 1 else 0
def reduceUDF(a: Int, b: Int): Int = a + b
val seniors: Collection[(Id, Int)] =
    graph.mrTriplets(MapUDF, reduceUDF)
```

???

如下所示：

选择 t.dstId，reduceF（mapF（t））AS msgSum
从三元组 AS t GROUP BY t.dstId
在此操作中，mrTriplets 运算符将生成一个集合，其中包含由目标顶点标识符键入的入站消息的总和。例如，在图 8.24 中，我们使用 mrTriplets 运算符来计算一个集合，其中包含社交网络中每个用户的较旧关注者的数量。由于生成的集合包含图形中顶点的子集，因此它可以重用与原始顶点集合相同的索引。

“process of graph parallel computation. Logically, the mrTriplets operator is the composition of the map and groupBy datafow operators on the triplets view. Figure 8.24 shows the property graph and associated mrTriplets operations as specified in the following Scala code:

Figure 8.24
Compute the number of older followers of each vertex user.

```
val graph: Graph[User, Double]
def mapUDF(t: Triplet[User, Double]) =
    if (t.src.age > t.dst.age) 1 else 0
def reduceUDF(a: Int, b: Int): Int = a + b
val seniors: Collection[(Id, Int)] =
    graph.mrTriplets(MapUDF, reduceUDF)
```

“The user-defined map function is applied to each triplet, yielding a value which is then aggregated at the destination vertex using the user-defined binary aggregation function as illustrated in the following:

SELECT t.dstId, reduceF(mapF(t)) AS msgSum
FROM triplets AS t GROUP BY t.dstId

In this operation, the mrTriplets operator produces a collection containing the sum of the inbound messages keyed by the destination vertex identifier. For example, in Figure 8.24, we use the mrTriplets operator to compute a collection containing the number of older followers for each user in a social network. Because the resulting collection contains a subset of the vertices in the graph, it can reuse the same indices as the original vertex collection.■”

???

# 商业系统

- Azure Data Lake Analytics
- Amazon Athena analytics platform.
- Google called Cloud Datalab

---

# 小结

- 机器学习
- 图计算

---

# 探索练习

- Spark 机器学习
- Spark 图计算
