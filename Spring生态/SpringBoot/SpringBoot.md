# SpringBoot

Spring 脚手架：

config：@Configuration，@Bean

JPA：@Entity，@Repository

REST：@RestController，@GetMapping()，@PostMapping()，@PutMapping()，@DeleteMapping()。

定时任务：@EnableScheduling，@Scheduled()。

## JPA

**Entity 配置** 

`@Entity` - 标注实体

`@Table(name = "table_name")` - 标注数据表

**Spring JpaRepository** 

```java
@NoRepositoryBean
public interface JpaRepository<T, ID extends Serializable> extends PagingAndSortingRepository<T, ID>, QueryByExampleExecutor<T> {
    List<T> findAll();

    List<T> findAll(Sort var1);

    List<T> findAll(Iterable<ID> var1);

    <S extends T> List<S> save(Iterable<S> var1);

    void flush();

    <S extends T> S saveAndFlush(S var1);

    void deleteInBatch(Iterable<T> var1);

    void deleteAllInBatch();

    T getOne(ID var1);

    <S extends T> List<S> findAll(Example<S> var1);

    <S extends T> List<S> findAll(Example<S> var1, Sort var2);
}
```

spring-boot-starter-jdbc：DataSourceTransactionManager。

spring-boot-starter-data-jpa：JpaTransactionManager。

@EnableTransactionManagement：启用事务管理。

@Transactional：发生异常，直接回滚。

