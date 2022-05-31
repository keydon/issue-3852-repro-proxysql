# issue-3852-repro-proxysql


Prerequisites: Working Docker Environment

1. Clone this Repro
2. Build components with
```bash
 docker-compose build
 ```
3. Build the spring sample app with
```bash
 docker-compose up spring-client-builder
 ```
4. Start mariadb
```bash
 docker-compose up -d mariadb
 ```
5. Start proxysql
```bash
 docker-compose up -d proxysql
 ```
6. Start sample-app
```bash
 docker-compose up spring-client-runner
 ```

Expected: 
App should be connect without issues

Actual Stacktrace:
```

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.7.0)

2022-05-31 12:19:44.118  INFO 1 --- [           main] com.example.demo.DemoApplication         : Starting DemoApplication using Java 17.0.3 on 27a7337a938c with PID 1 (/app/classes started by root in /)
2022-05-31 12:19:44.122  INFO 1 --- [           main] com.example.demo.DemoApplication         : No active profile set, falling back to 1 default profile: "default"
2022-05-31 12:19:44.559  INFO 1 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JDBC repositories in DEFAULT mode.
2022-05-31 12:19:44.566  INFO 1 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 4 ms. Found 0 JDBC repository interfaces.
2022-05-31 12:19:44.939  INFO 1 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
2022-05-31 12:19:44.947  INFO 1 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2022-05-31 12:19:44.947  INFO 1 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.63]
2022-05-31 12:19:45.015  INFO 1 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2022-05-31 12:19:45.015  INFO 1 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 856 ms
2022-05-31 12:19:45.320  INFO 1 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Starting...
2022-05-31 12:19:46.372 ERROR 1 --- [           main] com.zaxxer.hikari.pool.HikariPool        : HikariPool-1 - Exception during pool initialization.

java.sql.SQLNonTransientConnectionException: (conn=3) Initialization command fail
	at org.mariadb.jdbc.export.ExceptionFactory.createException(ExceptionFactory.java:281) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.export.ExceptionFactory.create(ExceptionFactory.java:347) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.postConnectionQueries(StandardClient.java:374) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.<init>(StandardClient.java:205) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.Driver.connect(Driver.java:64) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.Driver.connect(Driver.java:83) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.Driver.connect(Driver.java:27) ~[mariadb-java-client-3.0.4.jar:na]
	at com.zaxxer.hikari.util.DriverDataSource.getConnection(DriverDataSource.java:138) ~[HikariCP-4.0.3.jar:na]
	at com.zaxxer.hikari.pool.PoolBase.newConnection(PoolBase.java:364) ~[HikariCP-4.0.3.jar:na]
	at com.zaxxer.hikari.pool.PoolBase.newPoolEntry(PoolBase.java:206) ~[HikariCP-4.0.3.jar:na]
	at com.zaxxer.hikari.pool.HikariPool.createPoolEntry(HikariPool.java:476) ~[HikariCP-4.0.3.jar:na]
	at com.zaxxer.hikari.pool.HikariPool.checkFailFast(HikariPool.java:561) ~[HikariCP-4.0.3.jar:na]
	at com.zaxxer.hikari.pool.HikariPool.<init>(HikariPool.java:115) ~[HikariCP-4.0.3.jar:na]
	at com.zaxxer.hikari.HikariDataSource.getConnection(HikariDataSource.java:112) ~[HikariCP-4.0.3.jar:na]
	at org.springframework.jdbc.datasource.DataSourceUtils.fetchConnection(DataSourceUtils.java:159) ~[spring-jdbc-5.3.20.jar:5.3.20]
	at org.springframework.jdbc.datasource.DataSourceUtils.doGetConnection(DataSourceUtils.java:117) ~[spring-jdbc-5.3.20.jar:5.3.20]
	at org.springframework.jdbc.datasource.DataSourceUtils.getConnection(DataSourceUtils.java:80) ~[spring-jdbc-5.3.20.jar:5.3.20]
	at org.springframework.jdbc.core.JdbcTemplate.execute(JdbcTemplate.java:330) ~[spring-jdbc-5.3.20.jar:5.3.20]
	at org.springframework.data.jdbc.repository.config.DialectResolver$DefaultDialectProvider.getDialect(DialectResolver.java:108) ~[spring-data-jdbc-2.4.0.jar:2.4.0]
	at org.springframework.data.jdbc.repository.config.DialectResolver.lambda$getDialect$0(DialectResolver.java:78) ~[spring-data-jdbc-2.4.0.jar:2.4.0]
	at java.base/java.util.stream.ReferencePipeline$3$1.accept(Unknown Source) ~[na:na]
	at java.base/java.util.ArrayList$ArrayListSpliterator.tryAdvance(Unknown Source) ~[na:na]
	at java.base/java.util.stream.ReferencePipeline.forEachWithCancel(Unknown Source) ~[na:na]
	at java.base/java.util.stream.AbstractPipeline.copyIntoWithCancel(Unknown Source) ~[na:na]
	at java.base/java.util.stream.AbstractPipeline.copyInto(Unknown Source) ~[na:na]
	at java.base/java.util.stream.AbstractPipeline.wrapAndCopyInto(Unknown Source) ~[na:na]
	at java.base/java.util.stream.FindOps$FindOp.evaluateSequential(Unknown Source) ~[na:na]
	at java.base/java.util.stream.AbstractPipeline.evaluate(Unknown Source) ~[na:na]
	at java.base/java.util.stream.ReferencePipeline.findFirst(Unknown Source) ~[na:na]
	at org.springframework.data.jdbc.repository.config.DialectResolver.getDialect(DialectResolver.java:80) ~[spring-data-jdbc-2.4.0.jar:2.4.0]
	at org.springframework.data.jdbc.repository.config.AbstractJdbcConfiguration.jdbcDialect(AbstractJdbcConfiguration.java:186) ~[spring-data-jdbc-2.4.0.jar:2.4.0]
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[na:na]
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(Unknown Source) ~[na:na]
	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(Unknown Source) ~[na:na]
	at java.base/java.lang.reflect.Method.invoke(Unknown Source) ~[na:na]
	at org.springframework.beans.factory.support.SimpleInstantiationStrategy.instantiate(SimpleInstantiationStrategy.java:154) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.ConstructorResolver.instantiate(ConstructorResolver.java:653) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.ConstructorResolver.instantiateUsingFactoryMethod(ConstructorResolver.java:638) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.instantiateUsingFactoryMethod(AbstractAutowireCapableBeanFactory.java:1352) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1195) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:582) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:542) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:335) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:234) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:333) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:233) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveNamedBean(DefaultListableBeanFactory.java:1282) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveNamedBean(DefaultListableBeanFactory.java:1243) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveBean(DefaultListableBeanFactory.java:494) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.getBean(DefaultListableBeanFactory.java:349) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.getBean(DefaultListableBeanFactory.java:342) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.context.support.AbstractApplicationContext.getBean(AbstractApplicationContext.java:1172) ~[spring-context-5.3.20.jar:5.3.20]
	at org.springframework.data.jdbc.repository.config.AbstractJdbcConfiguration.jdbcCustomConversions(AbstractJdbcConfiguration.java:117) ~[spring-data-jdbc-2.4.0.jar:2.4.0]
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[na:na]
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(Unknown Source) ~[na:na]
	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(Unknown Source) ~[na:na]
	at java.base/java.lang.reflect.Method.invoke(Unknown Source) ~[na:na]
	at org.springframework.beans.factory.support.SimpleInstantiationStrategy.instantiate(SimpleInstantiationStrategy.java:154) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.ConstructorResolver.instantiate(ConstructorResolver.java:653) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.ConstructorResolver.instantiateUsingFactoryMethod(ConstructorResolver.java:486) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.instantiateUsingFactoryMethod(AbstractAutowireCapableBeanFactory.java:1352) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1195) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:582) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:542) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:335) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:234) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:333) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:208) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.config.DependencyDescriptor.resolveCandidate(DependencyDescriptor.java:276) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1389) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1309) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.ConstructorResolver.resolveAutowiredArgument(ConstructorResolver.java:887) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.ConstructorResolver.createArgumentArray(ConstructorResolver.java:791) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.ConstructorResolver.instantiateUsingFactoryMethod(ConstructorResolver.java:541) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.instantiateUsingFactoryMethod(AbstractAutowireCapableBeanFactory.java:1352) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1195) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:582) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:542) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:335) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:234) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:333) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:208) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:953) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:918) ~[spring-context-5.3.20.jar:5.3.20]
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:583) ~[spring-context-5.3.20.jar:5.3.20]
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.refresh(ServletWebServerApplicationContext.java:147) ~[spring-boot-2.7.0.jar:2.7.0]
	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:734) ~[spring-boot-2.7.0.jar:2.7.0]
	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:408) ~[spring-boot-2.7.0.jar:2.7.0]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:308) ~[spring-boot-2.7.0.jar:2.7.0]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1306) ~[spring-boot-2.7.0.jar:2.7.0]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1295) ~[spring-boot-2.7.0.jar:2.7.0]
	at com.example.demo.DemoApplication.main(DemoApplication.java:10) ~[classes/:na]
Caused by: java.sql.BatchUpdateException: java.sql.SQLNonTransientConnectionException: (conn=3) Socket error
	at org.mariadb.jdbc.export.ExceptionFactory.createBatchUpdate(ExceptionFactory.java:213) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.executePipeline(StandardClient.java:599) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.postConnectionQueries(StandardClient.java:343) ~[mariadb-java-client-3.0.4.jar:na]
	... 89 common frames omitted
Caused by: java.sql.SQLNonTransientConnectionException: (conn=3) Socket error
	at org.mariadb.jdbc.export.ExceptionFactory.createException(ExceptionFactory.java:281) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.export.ExceptionFactory.create(ExceptionFactory.java:347) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.readPacket(StandardClient.java:837) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.readResults(StandardClient.java:754) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.readResponse(StandardClient.java:673) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.executePipeline(StandardClient.java:550) ~[mariadb-java-client-3.0.4.jar:na]
	... 90 common frames omitted
Caused by: java.io.EOFException: unexpected end of stream, read 0 bytes from 4 (socket was closed by server)
	at org.mariadb.jdbc.client.socket.impl.PacketReader.readPacket(PacketReader.java:78) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.message.ClientMessage.readPacket(ClientMessage.java:114) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.readPacket(StandardClient.java:815) ~[mariadb-java-client-3.0.4.jar:na]
	... 93 common frames omitted

2022-05-31 12:19:46.373  WARN 1 --- [           main] ConfigServletWebServerApplicationContext : Exception encountered during context initialization - cancelling refresh attempt: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'jdbcMappingContext' defined in class path resource [org/springframework/boot/autoconfigure/data/jdbc/JdbcRepositoriesAutoConfiguration$SpringBootJdbcConfiguration.class]: Unsatisfied dependency expressed through method 'jdbcMappingContext' parameter 1; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'jdbcCustomConversions' defined in class path resource [org/springframework/boot/autoconfigure/data/jdbc/JdbcRepositoriesAutoConfiguration$SpringBootJdbcConfiguration.class]: Bean instantiation via factory method failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [org.springframework.data.jdbc.core.convert.JdbcCustomConversions]: Factory method 'jdbcCustomConversions' threw exception; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'jdbcDialect' defined in class path resource [org/springframework/boot/autoconfigure/data/jdbc/JdbcRepositoriesAutoConfiguration$SpringBootJdbcConfiguration.class]: Bean instantiation via factory method failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [org.springframework.data.relational.core.dialect.Dialect]: Factory method 'jdbcDialect' threw exception; nested exception is org.springframework.jdbc.CannotGetJdbcConnectionException: Failed to obtain JDBC Connection; nested exception is java.sql.SQLNonTransientConnectionException: (conn=3) Initialization command fail
2022-05-31 12:19:46.376  INFO 1 --- [           main] o.apache.catalina.core.StandardService   : Stopping service [Tomcat]
2022-05-31 12:19:46.387  INFO 1 --- [           main] ConditionEvaluationReportLoggingListener : 

Error starting ApplicationContext. To display the conditions report re-run your application with 'debug' enabled.
2022-05-31 12:19:46.403 ERROR 1 --- [           main] o.s.boot.SpringApplication               : Application run failed

org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'jdbcMappingContext' defined in class path resource [org/springframework/boot/autoconfigure/data/jdbc/JdbcRepositoriesAutoConfiguration$SpringBootJdbcConfiguration.class]: Unsatisfied dependency expressed through method 'jdbcMappingContext' parameter 1; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'jdbcCustomConversions' defined in class path resource [org/springframework/boot/autoconfigure/data/jdbc/JdbcRepositoriesAutoConfiguration$SpringBootJdbcConfiguration.class]: Bean instantiation via factory method failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [org.springframework.data.jdbc.core.convert.JdbcCustomConversions]: Factory method 'jdbcCustomConversions' threw exception; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'jdbcDialect' defined in class path resource [org/springframework/boot/autoconfigure/data/jdbc/JdbcRepositoriesAutoConfiguration$SpringBootJdbcConfiguration.class]: Bean instantiation via factory method failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [org.springframework.data.relational.core.dialect.Dialect]: Factory method 'jdbcDialect' threw exception; nested exception is org.springframework.jdbc.CannotGetJdbcConnectionException: Failed to obtain JDBC Connection; nested exception is java.sql.SQLNonTransientConnectionException: (conn=3) Initialization command fail
	at org.springframework.beans.factory.support.ConstructorResolver.createArgumentArray(ConstructorResolver.java:800) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.ConstructorResolver.instantiateUsingFactoryMethod(ConstructorResolver.java:541) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.instantiateUsingFactoryMethod(AbstractAutowireCapableBeanFactory.java:1352) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1195) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:582) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:542) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:335) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:234) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:333) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:208) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:953) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:918) ~[spring-context-5.3.20.jar:5.3.20]
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:583) ~[spring-context-5.3.20.jar:5.3.20]
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.refresh(ServletWebServerApplicationContext.java:147) ~[spring-boot-2.7.0.jar:2.7.0]
	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:734) ~[spring-boot-2.7.0.jar:2.7.0]
	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:408) ~[spring-boot-2.7.0.jar:2.7.0]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:308) ~[spring-boot-2.7.0.jar:2.7.0]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1306) ~[spring-boot-2.7.0.jar:2.7.0]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1295) ~[spring-boot-2.7.0.jar:2.7.0]
	at com.example.demo.DemoApplication.main(DemoApplication.java:10) ~[classes/:na]
Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'jdbcCustomConversions' defined in class path resource [org/springframework/boot/autoconfigure/data/jdbc/JdbcRepositoriesAutoConfiguration$SpringBootJdbcConfiguration.class]: Bean instantiation via factory method failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [org.springframework.data.jdbc.core.convert.JdbcCustomConversions]: Factory method 'jdbcCustomConversions' threw exception; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'jdbcDialect' defined in class path resource [org/springframework/boot/autoconfigure/data/jdbc/JdbcRepositoriesAutoConfiguration$SpringBootJdbcConfiguration.class]: Bean instantiation via factory method failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [org.springframework.data.relational.core.dialect.Dialect]: Factory method 'jdbcDialect' threw exception; nested exception is org.springframework.jdbc.CannotGetJdbcConnectionException: Failed to obtain JDBC Connection; nested exception is java.sql.SQLNonTransientConnectionException: (conn=3) Initialization command fail
	at org.springframework.beans.factory.support.ConstructorResolver.instantiate(ConstructorResolver.java:658) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.ConstructorResolver.instantiateUsingFactoryMethod(ConstructorResolver.java:486) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.instantiateUsingFactoryMethod(AbstractAutowireCapableBeanFactory.java:1352) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1195) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:582) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:542) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:335) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:234) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:333) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:208) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.config.DependencyDescriptor.resolveCandidate(DependencyDescriptor.java:276) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1389) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1309) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.ConstructorResolver.resolveAutowiredArgument(ConstructorResolver.java:887) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.ConstructorResolver.createArgumentArray(ConstructorResolver.java:791) ~[spring-beans-5.3.20.jar:5.3.20]
	... 19 common frames omitted
Caused by: org.springframework.beans.BeanInstantiationException: Failed to instantiate [org.springframework.data.jdbc.core.convert.JdbcCustomConversions]: Factory method 'jdbcCustomConversions' threw exception; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'jdbcDialect' defined in class path resource [org/springframework/boot/autoconfigure/data/jdbc/JdbcRepositoriesAutoConfiguration$SpringBootJdbcConfiguration.class]: Bean instantiation via factory method failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [org.springframework.data.relational.core.dialect.Dialect]: Factory method 'jdbcDialect' threw exception; nested exception is org.springframework.jdbc.CannotGetJdbcConnectionException: Failed to obtain JDBC Connection; nested exception is java.sql.SQLNonTransientConnectionException: (conn=3) Initialization command fail
	at org.springframework.beans.factory.support.SimpleInstantiationStrategy.instantiate(SimpleInstantiationStrategy.java:185) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.ConstructorResolver.instantiate(ConstructorResolver.java:653) ~[spring-beans-5.3.20.jar:5.3.20]
	... 33 common frames omitted
Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'jdbcDialect' defined in class path resource [org/springframework/boot/autoconfigure/data/jdbc/JdbcRepositoriesAutoConfiguration$SpringBootJdbcConfiguration.class]: Bean instantiation via factory method failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [org.springframework.data.relational.core.dialect.Dialect]: Factory method 'jdbcDialect' threw exception; nested exception is org.springframework.jdbc.CannotGetJdbcConnectionException: Failed to obtain JDBC Connection; nested exception is java.sql.SQLNonTransientConnectionException: (conn=3) Initialization command fail
	at org.springframework.beans.factory.support.ConstructorResolver.instantiate(ConstructorResolver.java:658) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.ConstructorResolver.instantiateUsingFactoryMethod(ConstructorResolver.java:638) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.instantiateUsingFactoryMethod(AbstractAutowireCapableBeanFactory.java:1352) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1195) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:582) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:542) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:335) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:234) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:333) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:233) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveNamedBean(DefaultListableBeanFactory.java:1282) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveNamedBean(DefaultListableBeanFactory.java:1243) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveBean(DefaultListableBeanFactory.java:494) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.getBean(DefaultListableBeanFactory.java:349) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.getBean(DefaultListableBeanFactory.java:342) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.context.support.AbstractApplicationContext.getBean(AbstractApplicationContext.java:1172) ~[spring-context-5.3.20.jar:5.3.20]
	at org.springframework.data.jdbc.repository.config.AbstractJdbcConfiguration.jdbcCustomConversions(AbstractJdbcConfiguration.java:117) ~[spring-data-jdbc-2.4.0.jar:2.4.0]
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[na:na]
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(Unknown Source) ~[na:na]
	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(Unknown Source) ~[na:na]
	at java.base/java.lang.reflect.Method.invoke(Unknown Source) ~[na:na]
	at org.springframework.beans.factory.support.SimpleInstantiationStrategy.instantiate(SimpleInstantiationStrategy.java:154) ~[spring-beans-5.3.20.jar:5.3.20]
	... 34 common frames omitted
Caused by: org.springframework.beans.BeanInstantiationException: Failed to instantiate [org.springframework.data.relational.core.dialect.Dialect]: Factory method 'jdbcDialect' threw exception; nested exception is org.springframework.jdbc.CannotGetJdbcConnectionException: Failed to obtain JDBC Connection; nested exception is java.sql.SQLNonTransientConnectionException: (conn=3) Initialization command fail
	at org.springframework.beans.factory.support.SimpleInstantiationStrategy.instantiate(SimpleInstantiationStrategy.java:185) ~[spring-beans-5.3.20.jar:5.3.20]
	at org.springframework.beans.factory.support.ConstructorResolver.instantiate(ConstructorResolver.java:653) ~[spring-beans-5.3.20.jar:5.3.20]
	... 55 common frames omitted
Caused by: org.springframework.jdbc.CannotGetJdbcConnectionException: Failed to obtain JDBC Connection; nested exception is java.sql.SQLNonTransientConnectionException: (conn=3) Initialization command fail
	at org.springframework.jdbc.datasource.DataSourceUtils.getConnection(DataSourceUtils.java:83) ~[spring-jdbc-5.3.20.jar:5.3.20]
	at org.springframework.jdbc.core.JdbcTemplate.execute(JdbcTemplate.java:330) ~[spring-jdbc-5.3.20.jar:5.3.20]
	at org.springframework.data.jdbc.repository.config.DialectResolver$DefaultDialectProvider.getDialect(DialectResolver.java:108) ~[spring-data-jdbc-2.4.0.jar:2.4.0]
	at org.springframework.data.jdbc.repository.config.DialectResolver.lambda$getDialect$0(DialectResolver.java:78) ~[spring-data-jdbc-2.4.0.jar:2.4.0]
	at java.base/java.util.stream.ReferencePipeline$3$1.accept(Unknown Source) ~[na:na]
	at java.base/java.util.ArrayList$ArrayListSpliterator.tryAdvance(Unknown Source) ~[na:na]
	at java.base/java.util.stream.ReferencePipeline.forEachWithCancel(Unknown Source) ~[na:na]
	at java.base/java.util.stream.AbstractPipeline.copyIntoWithCancel(Unknown Source) ~[na:na]
	at java.base/java.util.stream.AbstractPipeline.copyInto(Unknown Source) ~[na:na]
	at java.base/java.util.stream.AbstractPipeline.wrapAndCopyInto(Unknown Source) ~[na:na]
	at java.base/java.util.stream.FindOps$FindOp.evaluateSequential(Unknown Source) ~[na:na]
	at java.base/java.util.stream.AbstractPipeline.evaluate(Unknown Source) ~[na:na]
	at java.base/java.util.stream.ReferencePipeline.findFirst(Unknown Source) ~[na:na]
	at org.springframework.data.jdbc.repository.config.DialectResolver.getDialect(DialectResolver.java:80) ~[spring-data-jdbc-2.4.0.jar:2.4.0]
	at org.springframework.data.jdbc.repository.config.AbstractJdbcConfiguration.jdbcDialect(AbstractJdbcConfiguration.java:186) ~[spring-data-jdbc-2.4.0.jar:2.4.0]
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[na:na]
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(Unknown Source) ~[na:na]
	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(Unknown Source) ~[na:na]
	at java.base/java.lang.reflect.Method.invoke(Unknown Source) ~[na:na]
	at org.springframework.beans.factory.support.SimpleInstantiationStrategy.instantiate(SimpleInstantiationStrategy.java:154) ~[spring-beans-5.3.20.jar:5.3.20]
	... 56 common frames omitted
Caused by: java.sql.SQLNonTransientConnectionException: (conn=3) Initialization command fail
	at org.mariadb.jdbc.export.ExceptionFactory.createException(ExceptionFactory.java:281) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.export.ExceptionFactory.create(ExceptionFactory.java:347) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.postConnectionQueries(StandardClient.java:374) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.<init>(StandardClient.java:205) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.Driver.connect(Driver.java:64) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.Driver.connect(Driver.java:83) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.Driver.connect(Driver.java:27) ~[mariadb-java-client-3.0.4.jar:na]
	at com.zaxxer.hikari.util.DriverDataSource.getConnection(DriverDataSource.java:138) ~[HikariCP-4.0.3.jar:na]
	at com.zaxxer.hikari.pool.PoolBase.newConnection(PoolBase.java:364) ~[HikariCP-4.0.3.jar:na]
	at com.zaxxer.hikari.pool.PoolBase.newPoolEntry(PoolBase.java:206) ~[HikariCP-4.0.3.jar:na]
	at com.zaxxer.hikari.pool.HikariPool.createPoolEntry(HikariPool.java:476) ~[HikariCP-4.0.3.jar:na]
	at com.zaxxer.hikari.pool.HikariPool.checkFailFast(HikariPool.java:561) ~[HikariCP-4.0.3.jar:na]
	at com.zaxxer.hikari.pool.HikariPool.<init>(HikariPool.java:115) ~[HikariCP-4.0.3.jar:na]
	at com.zaxxer.hikari.HikariDataSource.getConnection(HikariDataSource.java:112) ~[HikariCP-4.0.3.jar:na]
	at org.springframework.jdbc.datasource.DataSourceUtils.fetchConnection(DataSourceUtils.java:159) ~[spring-jdbc-5.3.20.jar:5.3.20]
	at org.springframework.jdbc.datasource.DataSourceUtils.doGetConnection(DataSourceUtils.java:117) ~[spring-jdbc-5.3.20.jar:5.3.20]
	at org.springframework.jdbc.datasource.DataSourceUtils.getConnection(DataSourceUtils.java:80) ~[spring-jdbc-5.3.20.jar:5.3.20]
	... 75 common frames omitted
Caused by: java.sql.BatchUpdateException: java.sql.SQLNonTransientConnectionException: (conn=3) Socket error
	at org.mariadb.jdbc.export.ExceptionFactory.createBatchUpdate(ExceptionFactory.java:213) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.executePipeline(StandardClient.java:599) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.postConnectionQueries(StandardClient.java:343) ~[mariadb-java-client-3.0.4.jar:na]
	... 89 common frames omitted
Caused by: java.sql.SQLNonTransientConnectionException: (conn=3) Socket error
	at org.mariadb.jdbc.export.ExceptionFactory.createException(ExceptionFactory.java:281) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.export.ExceptionFactory.create(ExceptionFactory.java:347) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.readPacket(StandardClient.java:837) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.readResults(StandardClient.java:754) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.readResponse(StandardClient.java:673) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.executePipeline(StandardClient.java:550) ~[mariadb-java-client-3.0.4.jar:na]
	... 90 common frames omitted
Caused by: java.io.EOFException: unexpected end of stream, read 0 bytes from 4 (socket was closed by server)
	at org.mariadb.jdbc.client.socket.impl.PacketReader.readPacket(PacketReader.java:78) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.message.ClientMessage.readPacket(ClientMessage.java:114) ~[mariadb-java-client-3.0.4.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.readPacket(StandardClient.java:815) ~[mariadb-java-client-3.0.4.jar:na]
	... 93 common frames omitted

ited with code 1

```


## Connection without proxysql suceeds

1. reconfigure  
`spring-boot-client\src\main\resources\application.properties`
from `spring.datasource.url=jdbc:mariadb://proxysql:3306/...`  
  to `spring.datasource.url=jdbc:mariadb://mariadb:3306...`

2. rebuild sample app
```bash
 docker-compose build spring-client-builder
 docker-compose up spring-client-builder
 docker-compose rm -fs spring-client-runner
 ```

3. start sample app  
  ```bash
 docker-compose up spring-client-runner
 ```
4. Works without issues
