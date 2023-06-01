# Transaction-Management


when working with Spring's Transaction Management framework:

- Transaction Manager: The transaction manager is responsible for managing transactions. In Spring, you can use different transaction managers depending on your needs, such as the DataSourceTransactionManager for JDBC-based transactions, the HibernateTransactionManager for Hibernate-based transactions, or the JtaTransactionManager for distributed transactions.

- Transactional Annotation: The @Transactional annotation is used to mark a method as transactional. When a method is annotated with @Transactional, Spring will automatically start a new transaction before the method is executed, and commit or rollback the transaction based on the outcome of the method.

- Transaction Propagation: Transaction propagation defines how transactions are managed when multiple methods are called in a single transactional context. Spring supports different propagation behaviors, such as REQUIRED, which means that the method must run within a transaction context, or REQUIRES_NEW, which means that the method should run in its own transaction context.

- Isolation Level: Isolation level defines how transactional data is protected from other transactions. Spring supports different isolation levels, such as READ_COMMITTED, which means that the transaction can only see committed data, or SERIALIZABLE, which means that the transaction can only see data that has been committed by other transactions.

Here's an example of how to use Spring's Transaction Management framework with the @Transactional annotation:

Spring Boot, transaction management is enabled by default, so you don't need to do any additional configuration to use it. Spring Boot provides support for both programmatic and declarative transaction management.

To use programmatic transaction management, you can use the TransactionTemplate class to manage transactions in your code. Here's an example of how to use TransactionTemplate to perform a transactional database operation:

```java
@Service
public class UserService {
    
    private final JdbcTemplate jdbcTemplate;
    private final TransactionTemplate transactionTemplate;

    public UserService(JdbcTemplate jdbcTemplate, PlatformTransactionManager transactionManager) {
        this.jdbcTemplate = jdbcTemplate;
        this.transactionTemplate = new TransactionTemplate(transactionManager);
    }

    public void createUser(String name, int age) {
        transactionTemplate.execute(status -> {
            jdbcTemplate.update("INSERT INTO users (name, age) VALUES (?, ?)", name, age);
            return null;
        });
    }

    // Other methods
}
```

n this example, we're injecting a JdbcTemplate and a PlatformTransactionManager into the UserService class constructor using Spring's dependency injection mechanism. We're then using the TransactionTemplate class to execute an operation in a transaction. The execute method takes a lambda function that represents the transactional operation to be executed. The status parameter represents the transaction status, which can be used to control the transaction behavior (e.g., by rolling back the transaction if an error occurs).




To use declarative transaction management, you can annotate your service methods with the @Transactional annotation. Here's an example of how to use @Transactional to perform a transactional database operation:

```java
@Service
public class UserService {

    private final JdbcTemplate jdbcTemplate;

    public UserService(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    @Transactional
    public void createUser(String name, int age) {
        jdbcTemplate.update("INSERT INTO users (name, age) VALUES (?, ?)", name, age);
    }

    // Other methods
}
```
In this example, we're using the @Transactional annotation to indicate that the createUser method should be executed in a transaction. The annotation takes several optional parameters that can be used to customize the transaction behavior (e.g., the transaction propagation behavior, the isolation level, etc.).

# Local vs. Global Transactions
Local transactions are specific to a single transactional resource like a JDBC connection, whereas global transactions can span multiple transactional resources like transactions in a distributed system.


![rrrrr drawio](https://user-images.githubusercontent.com/73180409/228164411-f1bf3cd0-7b4a-46a5-8717-f152d919d89e.png)

RolleBack-> Any method Throw Exception 
Commite-> All method Run successefully

![image](https://user-images.githubusercontent.com/73180409/228165499-7765ec87-c5e7-43fc-842f-fa79cc10a80c.png)

# ACID
ACID transactions can be achieved through the use of the Spring Framework's transaction management capabilities. 

- Atomicity: In Spring, atomicity can be achieved through the use of the @Transactional annotation, which ensures that a transaction is executed as a single, atomic unit of work. If any part of the transaction fails, the entire transaction is rolled back, ensuring that the data remains in a consistent state.

- Consistency: Consistency is achieved by ensuring that transactions enforce all the rules and constraints defined in the database schema. In Spring, consistency can be achieved through the use of validation annotations, such as @NotNull or @Size, which ensure that data is consistent with the database schema.

- Isolation: Isolation refers to the degree to which one transaction is isolated from the other concurrent transactions. In Spring, isolation levels can be specified using the @Transactional annotation, which allows the developer to control the level of isolation required for a transaction.

- Durability: Durability ensures that once a transaction is committed, the changes made to the database are permanent and will not be lost due to system failure or other issues. In Spring, durability can be achieved through the use of database-specific transaction managers, such as the DataSourceTransactionManager, which ensures that changes made to the database are durable.
