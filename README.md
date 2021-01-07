[![screw-logo](https://images.gitee.com/uploads/images/2020/0728/155335_59a712d2_1407605.png)](https://github.com/pingfangushi/screw)

💕 企业级开发过程中，一颗永不生锈的螺丝钉。

[![LGPL3](https://img.shields.io/badge/license-LGPL3-blue.svg) ](https://github.com/pingfangushi/screw/blob/master/LICENSE)[![wiki](https://img.shields.io/badge/link-wiki-green.svg?style=flat-square) ](https://github.com/pingfangushi/screw)[![Maven Central](https://img.shields.io/maven-central/v/cn.smallbun.screw/screw-core) ](https://search.maven.org/search?q=cn.smallbun.screw)[![JDK Version](https://img.shields.io/badge/JDK-1.8+-green.svg) ](https://gitee.com/leshalv/screw/#)[![JDK Version](https://img.shields.io/badge/MAVEN-3.0+-green.svg)](https://gitee.com/leshalv/screw/#)

> 🚀 screw (螺丝钉) 英:[`skruː`] ~ 简洁好用的数据库表结构文档生成工具



官方地址

https://gitee.com/leshalv/screw/

##  数据库支持

-  MySQL

-  MariaDB

-  TIDB

-  Oracle

-  SqlServer

-  PostgreSQL

-  Cache DB（2016）

  

##  特点

- 简洁、轻量、设计良好
- 多数据库支持
- 多种格式文档
- 灵活扩展
- 支持自定义模板





## 使用方式

### 普通方式

- **引入依赖**

```xml
<dependency>
    <groupId>cn.smallbun.screw</groupId>
    <artifactId>screw-core</artifactId>
    <version>${lastVersion}</version>
 </dependency>
```

 

```java
/**
 * 文档生成
 */
void documentGeneration() {
   //数据源
   HikariConfig hikariConfig = new HikariConfig();
   hikariConfig.setDriverClassName("com.mysql.cj.jdbc.Driver");
   hikariConfig.setJdbcUrl("jdbc:mysql://127.0.0.1:3306/database");
   hikariConfig.setUsername("root");
   hikariConfig.setPassword("password");
   //设置可以获取tables remarks信息
   hikariConfig.addDataSourceProperty("useInformationSchema", "true");
   hikariConfig.setMinimumIdle(2);
   hikariConfig.setMaximumPoolSize(5);
   DataSource dataSource = new HikariDataSource(hikariConfig);
   //生成配置
   EngineConfig engineConfig = EngineConfig.builder()
         //生成文件路径
         .fileOutputDir(fileOutputDir)
         //打开目录
         .openOutputDir(true)
         //文件类型
         .fileType(EngineFileType.HTML)
         //生成模板实现
         .produceType(EngineTemplateType.freemarker)
         //自定义文件名称
         .fileName("自定义文件名称").build();

   //忽略表
   ArrayList<String> ignoreTableName = new ArrayList<>();
   ignoreTableName.add("test_user");
   ignoreTableName.add("test_group");
   //忽略表前缀
   ArrayList<String> ignorePrefix = new ArrayList<>();
   ignorePrefix.add("test_");
   //忽略表后缀    
   ArrayList<String> ignoreSuffix = new ArrayList<>();
   ignoreSuffix.add("_test");
   ProcessConfig processConfig = ProcessConfig.builder()
         //指定生成逻辑、当存在指定表、指定表前缀、指定表后缀时，将生成指定表，其余表不生成、并跳过忽略表配置	
		 //根据名称指定表生成
		 .designatedTableName(new ArrayList<>())
		 //根据表前缀生成
		 .designatedTablePrefix(new ArrayList<>())
		 //根据表后缀生成	
		 .designatedTableSuffix(new ArrayList<>())
         //忽略表名
         .ignoreTableName(ignoreTableName)
         //忽略表前缀
         .ignoreTablePrefix(ignorePrefix)
         //忽略表后缀
         .ignoreTableSuffix(ignoreSuffix).build();
   //配置
   Configuration config = Configuration.builder()
         //版本
         .version("1.0.0")
         //描述
         .description("数据库设计文档生成")
         //数据源
         .dataSource(dataSource)
         //生成配置
         .engineConfig(engineConfig)
         //生成配置
         .produceConfig(processConfig)
         .build();
   //执行生成
   new DocumentationExecute(config).execute();
}
```