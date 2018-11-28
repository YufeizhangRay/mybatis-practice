# mybatis-practice
Mybatis框架学习实践  
  
什么是`Mybatis`？  

>`Mybatis`是支持自定义`SQL`、存储过程和高级映射的一流持久化框架，它几乎消除了所有的`JDBC`代码，可以使用简单的XML或注释来配置`Java POJOS`（朴素的`Java`对象）到数据库记录的映射关系。  
  
`Mybatis` 配置文件：  
>`mybatis-config.xml`： 此文件作为 `MyBatis` 的全局配置文件，配置了 `MyBatis` 的运行环境等信息。  
`xxxMapper.xml`： 即 `SQL` 映射文件，文件中配置了操作数据库的 `SQL` 语句。此文件需要在 `mybatis-config.xml` 中加载。(也可以采用`注解`的形式替代`xml`，但是各有利弊，开发者可以选择互补使用。)  
  
与`Hibernate`的对比：  
>`Hibernate`：`Hibernate`是当前最流行的`ORM`框架之一，对`JDBC`提供了较为完整的封装。`Hibernate`的`O/R Mapping`实现了`POJO` 和数据库表之间的映射，以及`SQL`的自动生成和执行。  
`Mybatis`：`Mybatis`同样也是非常流行的`ORM`框架，主要着力点在于 `POJO `与` SQL `之间的映射关系。然后通过映射配置文件，将`SQL`所需的参数，以及返回的结果字段映射到指定` POJO `。相对`Hibernate“O/R”`而言，`Mybatis` 是一种`“Sql Mapping”`的`ORM`实现。  
  
>`Mybatis`：小巧、方便、高效、简单、直接、半自动化   
`Hibernate`：强大、方便、高效、复杂、间接、全自动化  
  
Mybatis的插件：  
>`Mybatis-Generator`  
>>此插件可以帮助开发者根据数据库表的信息自动生成相应的映射`POJO`类以及`Mapper`文件，开发者只需制定好相关的规则即可。这个过程与`Hibernate`的过程相反，`Hibernate`是根据`POJO`类以及`xml`文件为开发者自动去数据库生成相应的映射表。
  
>`Mybatis-PageHelper`  
>>一个非常好用的分页插件，可以帮助开发者迅速而简便的实现分页查询功能。  
当然开发这也可定义自己的插件。  
  
Mybatis在使用中需要注意的问题：  
>在`N+1`问题：  
>>官方文档不建议使用嵌套的`select`语句的形式，是因为这会导致所谓的`N+1`问题。  
当需要查询教师及其所指导的学生（一个教师可指导多个学生）信息时，我们会这么做：先用一条`SQL`语句（“`N+1`问题”中的`1`）查询教师的信息，即  
      `select * from teacher  `
      此时可查询出多条（记为`N`）教师记录。为了进一步查询出教师指导的学生的信息，需要针对每一条教师记录，生成一条`SQL`语句，即  
      `select * from student where supervisor_id=?`  
      以上`SQL`语句中的`“?”`就代表了每个教师的`id`。显而易见，这样的语句被生成了`N条`（“`N+1`问题”中的`N`）。这样在整个过程中，就总共执行了`N+1`条`SQL`语句，即`N+1`次数据库查询。而数据库查询通常是应用程序性能的瓶颈，一般应尽量减少数据库查询的次数，那么这种方式就会大大降低系统的性能。     
>为了解决这个问题，可采取`MyBatis`的`延迟加载机制`。  
