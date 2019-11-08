---
title: Knex.js+PostgreSQL
date: 2019-11-04 16:18:24
tags:
---

# What is Knex.js?

Knex.js is a tool in the Database Tools category of a tech stack.
Knex.js is an open source tool with 10.8K GitHub stars and 1.3K GitHub forks. 

Knex.js is a JavaScript query builder for relational databases including PostgreSQL, MySQL, SQLite3, Oracle, MSSQL, MariaDB and Amazon Redshift. It can be used with callbacks and promises. It supports transactions and connection pooling.

可以將操作資料庫的方式分成三種層次： low-level Database Drivers、middle-level Query Builders、high-level ORMs。

1. Database Drivers
This is basically as low-level as you can get — short of manually generating TCP packets and delivering them to the database. A database driver is going to handle connecting to a database (and sometimes connection pooling). At this level you’re going to be writing raw SQL strings and delivering them to a database, and receiving a response from the database. In the Node.js ecosystem there are many libraries operating at this layer. Here are three popular libraries:

- mysql: MySQL (13k stars / 330k weekly downloads)
- pg: PostgreSQL (6k stars / 520k weekly downloads)
- sqlite3: SQLite (3k stars / 120k weekly downloads)

Each of these libraries essentially works the same way: take the database credentials, instantiate a new database instance, connect to the database, and send it queries in the form of a string and asynchronously handle the result.

2. Query Builders
This is the intermediary level between using the simpler Database Driver module vs a full-fledged ORM. The most notable module which operates at this layer is Knex. This module is able to generate queries for a few different SQL dialects. This module depends on one of the aforementioned libraries — you’ll need to install the particular ones you plan on using with Knex.

- knex: Query Builder (8k stars / 170k weekly downloads)

3. ORMs
ORMs are powerful tools. The ORMs we’ll be examining in this post are able to communicate with SQL backends such as SQLite, PostgreSQL, MySQL, and MSSQL.
This is the highest level of abstraction we’re going to consider. When working with ORMs we typically need to do a lot more configuration ahead of time. The point of an ORM, as the name implies, is to map a record in a relational database to an object (typically, but not always, a class instance) in our application. What this means is that we’re defining the structure of these objects, as well as their relationships, in our application code.
Bust there are some reasons that you should be wary of using an ORM:

(1) **Take time to learn but it is not SQL.** An ORM is going to be written in the same language as the rest of the application, while SQL is a completely different syntax. A lot of people pick up an ORM because they don’t want to take the time to learn the underlying SQL (Structured Query Language).The syntax for a simple read operation varies greatly between these examples. As the operation you’re trying to perform increases in complexity, such as operations involving multiple tables, the ORM syntax will vary from between implementations even more.

(2) **Complex ORM calls can be Inefficient**: Recall that the purpose of an ORM is to take the underlying data stored in a database and map it into an object that we can interact within our application. This often comes with some inefficiencies when we use an ORM to fetch certain data.

(3) **Not all queries can be represented as an ORM operation**. When we need to generate these queries we have to fall back to generating the SQL query by hand. This often means a codebase with heavy ORM usage will still have a few handwritten queries strewn about it. A common situation which doesn’t work too well with ORMs is when a query contains a subquery. 

# Why use Knex.js?

與 SQL 原生語法相近，卻更簡潔、容易使用。語法設計讓參數和查詢字串分別傳遞給 database driver，保護查詢不會受到 SQL injection。

One nicety is that you’re able to programmatically generate dynamic queries in a much more convenient way than if you were to concatenate strings together to form SQL (which often introduces security vulnerabilities).

Parameters and query string are passed separately to database driver protecting query from SQL injection. Other query builder methods always uses binding format internally so they are safe too.

# How to use?

When creating a Knex instance you provide the connection details, along with the dialect you plan on using and are then able to start making queries. The queries you write will **closely resemble the underlying SQL queries**.  
詳細的使用說明可以查詢[官方文件](http://knexjs.org/)

Here is the example of using knex.js to connect postgreSQL:
```
// $ npm install pg knex

const knex = require('knex');
const connection = require('./connection.json');
const client = knex({
  client: 'pg',
  connection
});
```

要小心使用 `knex.raw()`，避免允許植入 SQL 語句。

# What is PostgreSQL? compare with MySQL?

PostgreSQL 和 MySQL 兩個資料庫之間有許多相似之處和重疊，但也有非常明顯的差異，使用 PostgreSQL 的優點：

-Open Source:
PostgreSQL 由 PostgreSQL 全球開發小組（PostgreSQL Global Development Group）開發，PostgreSQL 全球開發組由多個公司和個人貢獻者所組成。它是免費的開源軟體。PostgreSQL 是在 PostgreSQL 使用許可發布的，這是一個自由的開源許可，類似於 BSD 或 MIT 使用許可。

MySQL 開發專案根據 GNU GPL 的條款提供了它的原始碼以及各種各樣的專有協議。它現在由 Oracle 公司擁有，並且提供了幾個商業使用的付費版本。

- ACID Compliance:
ACID（Atomicity，Consistency，Isolation，Durability）是一組資料庫交易安全的特性，ACID 要求確保在單一的資料交易中發生多個更新時也不會在整個系統中遺失數據或發生錯誤。
PostgreSQL 完全符合 ACID 特性，並確保滿足所有要求。

MySQL 只有在使用 InnoDB 和 NDB 叢集儲存引擎時，才符合ACID 標準。

- Performance:
PostgreSQL 被廣泛應用於讀寫速度至關重要且資料需要驗證的大型系統中。 此外，它在商業解決方案中，還支持各種效能優化，（如地理空間資料支持，不需要讀取鎖定的資料一致性支援，如 Oracle、SQL Server）。 整體來看，PostgreSQL 的效能在需要執行複雜查詢的系統中會得到了最好的展現。 PostgreSQL 在 OLTP / OLAP 系統中運行良好，尤其在需要讀/寫速度和需要大量的資料分析時。 PostgreSQL 也適用於商業智慧應用服務，但更適合需要快速讀/寫速度的資料倉庫和資料分析應用服務。

MySQL 被廣泛採用，是由於 Web 應用，只需要一個簡單的資料交易資料庫。然而，在一般的情況下，如果 MySQL 的表現不佳，都是當負載過重或試圖進行複雜查詢的時候。MySQL 在 OLAP / OLTP 系統只需要讀取速度時表現良好。MySQL + InnoDB為 OLTP 場景提供了非常好的讀/寫速度。 整體來說，MySQL在高度資料一致性需求的情況下表現良好。MySQL 是可靠的，並且適用於商業智慧應用服務，因為商業智慧應用服務通常是更重視讀取效能的。

- Security:
PostgreSQL 具有角色並可繼承角色來設定和維護權限。 PostgreSQL 內建支援 SSL連線來加密客戶端/伺服器通訊。 它具有資料列級的安全性。除此之外，PostgreSQL 還附帶了一個名為 SE-PostgreSQL 的增強功能，可以根據 SELinux 安全策略提供額外的存取控制。

MySQL 為所有連線、查詢和用戶可能嘗試執行的其他操作實作了存取控制列表（ACL）的安全性。 對 MySQL 客戶端和伺服器之間的 SSL 加密連接也有支援。

- NoSQL Features/JSON Support:
NoSQL 和 JSON 都非常流行，NoSQL資料庫變得越來越普及。JSON 是一種簡單的資料格式，它允許程式設計師儲存和傳遞跨系統的資料內容、資料列表和 key-value 對應。
PostgreSQL 支援 JSON 和其他 NoSQL 的功能，如內建 XML 支援和 HSTORE 的 key-value 對應。 它還支援將 JSON 資料索引以加快存取速度。

MySQL 具有 JSON 資料類型支援，但沒有其他 NoSQL功能，也不支援 JSON 索引。

- Programming Languages Support:
PostgreSQL 支援各種程式語言，包括：C / C ++，Java，JavaScript，.Net，R，Perl，Python，Ruby，Tcl 等等。 甚至可以在單獨的程序中執行用戶提供的程式（即作為背景執行程式）。

MySQL 一些支援伺服器端不可擴展的程式語言。

- Extensible Type System:
PostgreSQL有幾個專用於延伸套件的功能，可以增加新的資料型別、新的函數功能、新的索引類型。

MySQL 不支援增加延伸功能。

更多比較可以查看 PostgreSQL官方文件：[PostgreSQL vs MySQL](https://faq.postgresql.tw/postgresql-vs-mysql)

PostgreSQL 相對於 MySQL 的優勢：

　　1、在 SQL 的標準實現上要比 MySQL 完善，而且功能實現比較嚴謹；
　　2、存儲過程的功能支持要比 MySQL 好，具備本地緩存執行計劃的能力；
　　3、對表連接支持較完整，優化器的功能較完整，支持的索引類型很多，復雜查詢能力較強；
　　4、PG 主表采用堆表存放，MySQL 采用索引組織表，能夠支持比MySQL更大的數據量。
　　5、PG 的主備復制屬於物理復制，相對於 MySQL 基於 binlog 的邏輯復制，數據的一致性更加可靠，復制性能更高，對主機性能的影響也更小。
　　6、MySQL 的存儲引擎插件化機制，存在鎖機制復雜影響並發的問題，而 PG 不存在。

　　MySQL 相對於 PG 的優勢：

　　1、innodb 的基於回滾段實現的 MVCC 機制，相對 PG 新老數據一起存放的基於 XID 的 MVCC 機制，是占優的。新老數據一起存放，需要定時觸發 VACUUM，會帶來多余的 IO和數據庫對象加鎖開銷，引起數據庫整體的並發能力下降。而且 VACUUM清理不及時，還可能會引發數據膨脹；
　　2、MySQL采用索引組織表，這種存儲方式非常適合基於主鍵匹配的查詢、刪改操作，但是對表結構設計存在約束；
　　3、MySQL的優化器較簡單，系統表、運算符、數據類型的實現都很精簡，非常適合簡單的查詢操作；
　　4、MySQL分區表的實現要優於PG的基於繼承表的分區實現，主要體現在分區個數達到上千上萬後的處理性能差異較大。
　　5、MySQL的存儲引擎插件化機制，使得它的應用場景更加廣泛，比如除了innodb適合事務處理場景外，myisam適合靜態數據的查詢場景。

總體上來說，開源數據庫都不是很完善，商業數據庫oracle在架構和功能方面都還是完善很多的。從應用場景來說，PG更加適合嚴格的企業應用場景（比如金融、電信、ERP、CRM），而MySQL更加適合業務邏輯相對簡單、數據可靠性要求較低的互聯網場景（比如Google、facebook、alibaba）。

雖然有不同的歷史、引擎與工具，不過並沒有明確的參考能夠表明這兩個數據庫哪一個能夠適用於所有情況。很多組織喜歡使用PostgreSQL，因為它的可靠性好，在保護數據方面很擅長，而且是個社區項目，不會陷入廠商的牢籠之中。MySQL更加靈活，提供了更多選項來針對不同的任務進行裁剪。很多時候，對於一個組織來說，對某個軟件使用的熟練程度要比特性上的原因更重要。

# Reference

[Why you should avoid ORMs (with examples in Node.js)](https://blog.logrocket.com/why-you-should-avoid-orms-with-examples-in-node-js-e0baab73fa5/)
[Knex.js](http://knexjs.org/)
[PostgreSQL vs MySQL](https://faq.postgresql.tw/postgresql-vs-mysql)
[PostgreSQL 與 MySQL的比較](https://www.itread01.com/articles/1502570301.html)
