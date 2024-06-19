Models are the basis for making database table structures map to Spring Boot's Java instances (basically Spring's version of POJOs)

A basic User model for accessing users in the database. Declare that it is a database Entity by using the `@Entity` annotation. `@Table` sets the table name. For each of the table's columns, be sure to name the table's column names (`@Column`), as they may differ from code-side variable names.

### User model

```java
@Entity  
@Table(name = "users")  
public class User {  
    @Id  
    @Column(name = "id")  
    private String id;  
  
    @Column(name = "email")  
    private String email;  

	// for the purpose of mapping column names to Java variable names.
    @Column(name = "passwd")  
    private String password;  
  
    @Column(name = "firstname")  
    private String firstName;  
  
    @Column(name = "lastname")  
    private String lastName;  
  
    @Column(name = "url")  
    private String url;  
  
    public String getId() {  
        return id;  
    }  
  
    public void setId(String id) {  
        this.id = id;  
    }  
  
    public String getEmail() {  
        return email;  
    }  
  
    public void setEmail(String email) {  
        this.email = email;  
    }  
  
    public String getPassword() {  
        return password;  
    }  
  
    public void setPassword(String password) {  
        this.password = password;  
    }  
  
    public String getFirstName() {  
        return firstName;  
    }  
  
    public void setFirstName(String firstName) {  
        this.firstName = firstName;  
    }  
  
    public String getLastName() {  
        return lastName;  
    }  
  
    public void setLastName(String lastName) {  
        this.lastName = lastName;  
    }  
  
    public String getUrl() {  
        return url;  
    }  
  
    public void setUrl(String url) {  
        this.url = url;  
    }  
  
    @Override  
    public String toString() {  
        return getId().concat(": ").concat(getEmail()).concat("; ")  
                .concat(getFirstName()).concat(" ").concat(getLastName());  
    }  
}
```

