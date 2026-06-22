# Pros and cons of document databases

Mongo is a document database, and one of its most characteristic features is that it is schemaless, i.e., the database has only a very limited awareness of what kind of data is stored in its collections. The schema of the database exists only in the program code, which interprets the data in a specific way, e.g., by identifying that some of the fields are references to objects in another collection.

MongoDB 是一种文档数据库，其最显著的特点之一是无模式（schemaless），即数据库本身对集合中存储的数据结构几乎没有感知。数据库的模式仅存在于程序代码中，由代码负责以特定方式解释数据——例如，识别某些字段是对另一个集合中对象的引用。

There are both advantages and disadvantages to schemalessness. One advantage is the flexibility it offers: since there is no need to define a schema at the database level, application development can be faster and easier in certain cases, and defining and modifying the schema requires little effort in any case. The problems with schemalessness are related to error susceptibility: everything is left to the programmer. The database itself has no way of checking whether the data in it is consistent, i.e., whether all mandatory fields have values, whether the reference type fields refer to existing and correct types of objects, etc.

无模式既有优点也有缺点。

- 优点在于其灵活性：由于无需在数据库层面定义模式，在某些情况下可以加快并简化应用开发，且定义和修改模式几乎不需要额外代价。

- 缺点则与容错性差有关：一切都依赖程序员自己把控。数据库本身无法验证其中的数据是否一致——例如，是否所有必填字段都有值、引用类型字段是否指向已存在且类型正确的对象等等。

The relational databases rely heavily on the existence of a schema, and the advantages and disadvantages of schematic databases are almost the opposite of those of non-schematic databases.

关系型数据库则高度依赖模式的存在，其优缺点与无模式数据库几乎正好相反。