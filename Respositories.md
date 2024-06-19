These are Java interfaces for setting up and running queries against the database. There are sets of default CRUD operations that exist without having to create a repository class (e.g. `findAll`, `findById`, `findAllById`, `save`, etc...). These are derived from the `Crudrespository<>` class. In the below example, we are creating a repo instance for the users table, which is utilizing the [[Models#User model|User model]].

```java
public interface UserRepo extends CrudRepository<User, String> {  
      
}
```

Make sure the first parameter for the return types are set to the [[Entities|Entity]] that gets returned for each record as well as the type for the corresponding table's primary key (in this case it is a string as in this table it is a hash value).