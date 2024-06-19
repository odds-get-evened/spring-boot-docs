
## Customized Login

Setup a mapping in a controller to point to "/login" (this overrides the default login page)

```java
@Controller
class TheController {
	...
	@GetMapping("/login")  
	public String login(Model model, String error, String logout) {  
	    if(error != null) model.addAttribute("errorMsg", "username or password is invalid");  
	  
	    return "login";  
	}
	...
}
```

### Configure Security

Get the security configuration set up:

```java
@Configuration  
@EnableWebSecurity  
public class PracSecurityConfig {  
    /**  
     * set up a service to map custom user data to Spring's     
     * UserDetailsService     
     **/    
    @Bean  
    public PracUserDetailsService userDetailsService() {  
        return new PracUserDetailsService();  
    }  
```

[[Authentication#Implementing `UserDetailsService`|see the code]] for `PracUserDetailService` for more details

```java
    /**  
     * this will assign the custom UserDetailsService to Spring's authentication        * provider.     
     **/    
    @Bean  
    public AuthenticationProvider authenticationProvider() {  
        DaoAuthenticationProvider prov = new DaoAuthenticationProvider();  
        prov.setUserDetailsService(userDetailsService());  
        prov.setPasswordEncoder(passwordEncoder());  
  
        return prov;  
    }  
```

Handle custom password:

```java
    /**  
     * sets up a custom password encoder     
     * in this case we are using the encoding     
     * used in MariaDB or MySQL     
     **/    
    private PasswordEncoder passwordEncoder() {  
        return new MariaDBPasswordEncoder();  
    }  
```

The `MariaDBPasswordEncoder` instance implements native `PasswordEncoder`. This will provide the proper coding and password matches for the native authentication process.

```java
public class MariaDBPasswordEncoder implements PasswordEncoder {  
    @Override  
    public String encode(CharSequence rawPassword) {  
        return PracUtils.mariaDBPassword(rawPassword.toString());  
    }  
  
    @Override  
    public boolean matches(CharSequence rawPassword, String encodedPassword) {  
        return PracUtils.mariaDBPassword(rawPassword.toString()).equals(encodedPassword);  
    }  
}
```

This part of the security configuration allows us to customize not just the form location, but also its custom login parameters from the form, and the parts of the application that require authorization or not (form login [example](https://howtodoinjava.com/spring-security/login-form-example/)).

```java
    @Bean  
    protected SecurityFilterChain configure(HttpSecurity http) throws Exception {
        /*         
         * set up the app's authentication zones         
         * - permitAll() is any user or not authorized users         
         * - authenticated() requires a authorized user         
         * - apply permitAll to login and logout instances         
         * here we set up the login page permissions as well         
         * as the POST parameter names from the form that will         
         * be passed to the native authentication process.         
         */        
        http.authorizeHttpRequests((auth) -> auth.requestMatchers(  
                "/", "/assets/**", "/register/**"  
        ).permitAll()  
                .requestMatchers("/test").authenticated());  
  
        http.formLogin((loginForm) -> loginForm.loginPage("/login")  
                .usernameParameter("loginEmail")  
                .passwordParameter("loginPasswd").permitAll());  
  
        http.logout(LogoutConfigurer::permitAll);  
  
        return http.build();  
    }  
}
```

### Implementing `UserDetailsService`

Below is the `UserDetailsService` established in the [[Authentication#Configure Security|above security configuration]]. At this point we're bundling up the `UserRepo` a repository used to query the database for a user account, the assignment of the user data to the `UserDetails` instance. This is the decision of whether user is authorized or not.

```java
public class PracUserDetailsService implements UserDetailsService {  
    @Autowired  
    private UserRepo userRepo;  
  
    @Override  
    public UserDetails loadUserByUsername(String uname) throws UsernameNotFoundException {  
        User u = userRepo.findByEmail(uname);  
  
        if(u == null) throw new UsernameNotFoundException("no user found with the email ".concat(uname));  
  
        return new PracUserDetails(u);  
    }  
}
```

This will return an instance that implements `UserDetails` the class used to handle the customized login, mapping the `User` Entity to the native authorization parts.

```java
public class PracUserDetails implements UserDetails {  
    // what Entity we use to authenticate  
    private User user;  
  
    public PracUserDetails(User u) {  
        this.user = u;  
    }  
```

Next, tell Spring what user roles to use for authentication.

```java
  
    @Override  
    public Collection<? extends GrantedAuthority> getAuthorities() {  
        SimpleGrantedAuthority auth = new SimpleGrantedAuthority("USER");  
  
        return List.of(auth);  
    }  
```

Below we are using the implemented methods from `UserDetails` to map our `User::password` to native password, and `User::email` to native username (since this sample site uses emails instead of usernames).

```java
  
    @Override  
    public String getPassword() {  
        return user.getPassword();  
    }  
  
    @Override  
    public String getUsername() {  
        return user.getEmail();  
    }  
  
    @Override  
    public boolean isAccountNonExpired() {  
        return true;  
    }  
  
    @Override  
    public boolean isAccountNonLocked() {  
        return true;  
    }  
  
    @Override  
    public boolean isCredentialsNonExpired() {  
        return true;  
    }  
  
    @Override  
    public boolean isEnabled() {  
        return true;  
    }  
}
```