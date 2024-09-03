# spring 事物管理
在 Spring 框架中，事务管理是一个非常重要的功能，它确保了数据库操作的一致性和完整性。Spring 提供了多种事务管理方式，包括编程式事务管理和声明式事务管理。以下是关于 Spring 事务管理的详细讲解：
## 1. 事务管理的基本概念
* 事务（Transaction）：事务是数据库操作的基本单位，它包含一组数据库操作，这些操作要么全部成功，要么全部失败。
* ACID 属性：事务具有以下四个基本属性：
  *  原子性（Atomicity）：事务是一个不可分割的工作单位，事务中的操作要么全部成功，要么全部失败。
  * 一致性（Consistency）：事务必须使数据库从一个一致性状态变换到另一个一致性状态。
  * 隔离性（Isolation）：多个事务并发执行时，一个事务的执行不应影响其他事务的执行。
  * 持久性（Durability）：事务成功完成后，其对数据库的更改是永久的，即使系统故障也不应该有影响。
## 2. Spring 事务管理的方式
### 2.1 编程式事务管理

编程式事务管理通过编写代码来管理事务的开始、提交和回滚。Spring 提供了 TransactionTemplate 和 PlatformTransactionManager 接口来实现编程式事务管理。

    import org.springframework.transaction.PlatformTransactionManager;
    import org.springframework.transaction.TransactionDefinition;
    import org.springframework.transaction.TransactionStatus;
    import org.springframework.transaction.support.DefaultTransactionDefinition;
    public class ProgrammaticTransactionExample {
        private PlatformTransactionManager transactionManager;
        public void setTransactionManager(PlatformTransactionManager transactionManager) {
            this.transactionManager = transactionManager;
        }
        public void performTransaction() {
            TransactionDefinition def = new DefaultTransactionDefinition();
            TransactionStatus status = transactionManager.getTransaction(def);
            try {
                // 执行数据库操作
                // ...
                transactionManager.commit(status);
            } catch (Exception e) {
                transactionManager.rollback(status);
                throw e;
            }
        }
    }
### 2.2 声明式事务管理
声明式事务管理通过配置来管理事务，无需在业务代码中编写事务管理代码。Spring 提供了 @Transactional 注解和 AOP（面向切面编程）来实现声明式事务管理。

#### 2.2.1 使用 @Transactional 注解
在 Spring 中，你可以使用 @Transactional 注解来标记需要事务管理的方法或类。

    import org.springframework.transaction.annotation.Transactional;

    @Transactional
    public class UserService {

        public void addUser(User user) {
            // 执行数据库操作
            // ...
        }
    }
#### 2.2.2 配置事务管理器
你需要在 Spring 配置文件中配置事务管理器，并启用事务注解支持。

    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
    </bean>

    <tx:annotation-driven transaction-manager="transactionManager"/>
或者在 Java 配置类中：

    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.jdbc.datasource.DataSourceTransactionManager;
    import org.springframework.transaction.PlatformTransactionManager;
    import org.springframework.transaction.annotation.EnableTransactionManagement;
    import javax.sql.DataSource;

    @Configuration
    @EnableTransactionManagement
    public class TransactionConfig {

        @Bean
        public PlatformTransactionManager transactionManager(DataSource dataSource) {
            return new DataSourceTransactionManager(dataSource);
        }
    }
## 3. 事务传播行为
Spring 提供了多种事务传播行为，用于定义事务在不同方法调用之间如何传播。常见的传播行为包括：
* REQUIRED：如果当前存在事务，则加入该事务；如果没有事务，则创建一个新事务。
* REQUIRES_NEW：创建一个新事务，如果当前存在事务，则挂起当前事务。
* SUPPORTS：如果当前存在事务，则加入该事务；如果没有事务，则以非事务方式执行。
* NOT_SUPPORTED：以非事务方式执行，如果当前存在事务，则挂起当前事务。
* MANDATORY：如果当前存在事务，则加入该事务；如果没有事务，则抛出异常。
* NEVER：以非事务方式执行，如果当前存在事务，则抛出异常。
* NESTED：如果当前存在事务，则在嵌套事务内执行；如果没有事务，则创建一个新事务。

## 4. 事务隔离级别
Spring 支持多种事务隔离级别，用于定义事务之间的隔离程度。常见的隔离级别包括：
* DEFAULT：使用数据库默认的隔离级别。
* READ_UNCOMMITTED：允许读取未提交的数据（可能导致脏读、不可重复读和幻读）。
* READ_COMMITTED：允许读取已提交的数据（可能导致不可重复读和幻读）。
* REPEATABLE_READ：确保同一事务中的多次读取结果一致（可能导致幻读）。
* SERIALIZABLE：最高隔离级别，确保事务串行执行，避免脏读、不可重复读和幻读。

## 5. 示例：使用 @Transactional 注解
以下是一个完整的示例，展示如何在 Spring Boot 应用中使用 @Transactional 注解进行事务管理：

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.stereotype.Service;
    import org.springframework.transaction.annotation.Transactional;

    import javax.sql.DataSource;
    import java.sql.Connection;
    import java.sql.PreparedStatement;
    import java.sql.SQLException;

    @Service
    public class UserService {

        @Autowired
        private DataSource dataSource;

        @Transactional
        public void addUser(User user) {
          try (Connection connection = dataSource.getConnection();
             PreparedStatement statement = connection.prepareStatement("INSERT INTO users (name, email) VALUES (?, ?)")) {
            statement.setString(1, user.getName());
            statement.setString(2, user.getEmail());
            statement.executeUpdate();
          } catch (SQLException e) {
            throw new RuntimeException("Failed to add user", e);
          }
        }
    }

以上代码展示了如何在 Spring Boot 应用中使用 @Transactional 注解进行事务管理。你可以根据实际需求选择适合的事务管理方式和配置。










