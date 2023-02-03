# Expense-Tracker-API
Restful API for tracking expenses. Spring Boot Java - Tutorial - REST API using PostgreSQL and JWT

Java Notes:
<ul>
  <li>Like a class, an <b>interface</b> can have methods and variables, but the methods declared in an interface are by default abstract. Interfaces specify what a class must do and not how. It is the blueprint of the class.</li>
  <li>Class <b>Pattern</b> is a compiled representation of a regular expression.</li>
  <li>Regex expressions. Can use <a href="https://regex101.com/">regex101</a> to determine regex expressions.</li>
  <li>Interface <code>PreparedStatement</code>. An object that represents a precompiled SQL statement. A SQL statement is precompiled and stored in a PreparedStatement object. This object can then be used to efficiently execute this statement multiple times.</li>
  <li>Interface <code>Statement</code>. The object used for executing a static SQL statement and returning the results it produces.</li>
</ul>

Spring Boot Notes:
<ul>
  <li>Service Components are the class file which contains @Service annotation. These class files are used to write business logic in a different layer, separated from @RestController class file. Spring will automatically scan and identifies classes with these annotations and configures appropriately.</li>
  <li>@Transactional annotation, Spring will enable transactional support. Any failure causes the entire operation to roll back to its previous state and to re-throw the original exception.</li>
  <li>Spring @Autowired annotation is used for automatic dependency injection. Spring framework is built on dependency injection and we inject the class dependencies through spring bean configuration file.</li>
  <li>Spring @Repository annotation is used to indicate that the class provides the mechanism for storage, retrieval, search, update and delete operation on objects.</li>
  <li>Class JdbcTemplate simplifies the use of JDBC and helps to avoid common errors. It executes core JDBC workflow, leaving application code to provide SQL and extract results.</li>
  <li>Interface KeyHolder: Interface for retrieving keys, typically used for auto-generated keys as potentially returned by JDBC insert statements.</li>
  <li>Class GeneratedKeyHolder: The standard implementation of the KeyHolder interface, to be used for holding auto-generated keys (as potentially returned by JDBC insert statements).</li>
  <li>Interface <code>RowMapper</code>. RowMapper interface allows to map a row of the relations with the instance of user-defined class. It iterates the ResultSet internally and adds it into the collection. So we don't need to write a lot of code to fetch the records as ResultSetExtractor.</li>
  <li>Class <code>ResponseEntity</code> is an extension of HttpEntity that adds an HttpStatusCode status code. Used in RestTemplate as well as in @Controller methods.</li>
</ul>

Docker Commands:
<ul>
  <li>~<code>docker container exec -it postgresdb psql -U postgres</code></li>
  <li>~<code>\connect expensetrackerdb</code></li>
  <li>select * from et_users;</li>
  <li>~<code>delete from et_users;</code></li>
</ul>

<b>Process:</b> <br>
<a href="https://www.youtube.com/watch?v=fVq9aPNGLAg&list=UULFLCn3zEnB0h0Y2GVhTLtHkg&index=12">1 - Installation process</a>
<ul>
  <li>Download Postman</li>
  <li>Download IntelliJ</li>
  <li>Download Docker. </li>
  <li>Download PostgreSQL. 
    <ul>When trying to run ~<code>docker pull postgres</code> got below error.
      <li><code>error during connect: This error may indicate that the docker daemon is not running.: Post "http://%2F%2F.%2Fpipe%2Fdocker_engine/v1.24/images/create?fromImage=postgres&tag=latest": open //./pipe/docker_engine: The system cannot find the file specified.</code></li>
      <li>Needed to use ~<code>"C:\Program Files\Docker\Docker\DockerCli.exe" -SwitchDaemon</code> from <a href="https://stackoverflow.com/questions/40459280/docker-cannot-start-on-windows">Stackoverflow Link</a>.</li>
      <li>Went through DockerDesktop Tutorial. Created a new container named "docker101tutorial".</li>
      <li>Reran ~<code>docker pull postgres</code> and it was completed.</li>
      <li>Ran ~<code>docker container ls</code> to view containers.</li>
    </ul>
  </li>
<li>Create new container using ~<code>docker container run --name postgresdb -e POSTGRES_PASSWORD=admin -d -p 5432:5432 postgres</code></li>
<li>Check creation with ~<code>docker container ls</code></li>
</ul>

<a>2 - Project Setup & creating Database Objects</a>
<ul>
  <li>Go to <a href="https://start.spring.io/">Spring Initializr</a> and generate a new spring boot project.</li>
  <li>Add dependencies:
    <ul>
      <li>Spring Web - WEB: Build web, including RESTful, applications using Spring MVC. Uses Apache Tomcat as the default embedded container.</li>
      <li>JDBC API - SQL: Database Connectivity API that defines how a client may connect and query a database.</li>
      <li>PostgreSQL Driver - SQL: A JDBC and R2DBC driver that allows Java programs to connect to a PostgreSQL database using standard, database independent Java code.</li>
      <li>Spring Boot DevTools - DEVELOPER TOOLS: Provides fast application restarts, LiveReload, and configurations for enhanced development experience.</li>
    </ul>
  </li>
  <li>Import generated spring boot project file into IntelliJ. <a href="https://www.youtube.com/watch?v=397QPCAjm0o">Youtube</a></li>
  <li>Create new file "expensetracker_db.sql".
    <ul>
      <li>Create user <code>expensetracker</code> and database <code>expensetrackerdb</code>.</li>
      <li>Enable privileges all on tables and sequences for user <code>expensetracker</code>.</li>
      <li>Create table <code>et_users</code>.</li>
      <li>Create table <code>et_categories</code> with foreign key constraint to prevent actions that would destroy links between tables.</li>
      <li>Create table <code>et_transactions</code> with constraints for foreign key and user_id.</li>
      <li>Create sequences <code>et_users_seq</code>, <code>et_categories_seq</code>, and <code>et_transactions_seq</code>.</li>
    </ul>
  </li>
  <li>Run "expensetracker_db.sql" using docker
    <ul>
      <li>Copy contents of "expensetracker_db.sql" to the db container "postgresdb" using ~<code>docker cp expensetracker_db.sql postgresdb:/</code>.</li>
      <li>Open docker bash using ~<code>docker container exec -it postgresdb bash</code>.</li>
      <li>In the bash, view the sql file using ~<code>ls</code>. Then use PostgreSQL interactive terminal to run sql file by running ~<code>psql -U postgres --file expensetracker_db.sql</code>. ~<code>exit</code> to exit.</li>
    </ul>
  </li>
  <li>Modify application.properties with spring.datasource.url, spring.datasource.username, and spring.datasource.password.</li>
  <li>Run "ExpenseTrackerApiApplication" and navigate to http://localhost:8080/ to see that the app is running.</li>
  <li>Create a new package named "resources" in "com.pairlearning.expensetracker" where we will put our user resource classes.
  <li>Create class UserResource.
    <ul>
      <li>Use <code>@RestController</code> to import RestController.</li>
      <li>Use <code>@RequestMapping</code> to import RestMapping.</li>
      <li>Create method <code>registerUser()</code> to register the user.</li>
    </ul>
  </li>
  <li>Test api endpoint using Postman. Select POST with url "https://http://localhost:8080/api/users/register". Select Body->raw->text->JSON and input below</li>
  <li>
    {
      "firstName": "David",
      "lastName": "Smith",
      "email": "david@testmail.com",
      "password": "test123"
    }
  </li>
</ul>

3 - Persisting User Info on Register
<ul>
  <li>Create new package "domain" for holding all entity classes for our app. Inside "domain" package, create new class "UserService".
    <ul>
      <li>Create variables for User(userId, firstName, lastName, email and password).</li>
      <li>Generate constructors for each variable.</li>
      <li>Generate getters and setters for each variable.</li>
    </ul>
  </li>
  <li>Create new package "exceptions". Inside package "exceptions", create new class EtAuthException. Whenever we throw this exception, Spring will automatically respond back with the status we provided here <code>@ResponseStatus(HttpStatus.UNAUTHORIZED)</code></li>
  <li>Create new package "services". Inside package "services", create new interface "UserService" to register/validate a User and throw exception <code>EtAuthException</code> when unauthorized.</li>
  <li>Create new package "repositories". Inside package "repositories", create new interface "UserRepository" to reflect db operations. </li>
  <li>Add implementation classes UserServiceImpl and UserRepositoriesImpl for both services and repositories. Implement the methods associated with the implementation classes.</li>
  <li><code>UserServiceImpl</code>
    <ul>
      <li>Add <code>@Service</code> annotation to this class. Service Components are the class file which contains @Service annotation. These class files are used to write business logic in a different layer, separated from @RestController class file. Spring will automatically scan and identifies classes with these annotations and configures appropriately. </li>
      <li>Add <code>@Transactional</code> annotation to this class. Spring will enable transactional support. Any failure causes the entire operation to roll back to its previous state and to re-throw the original exception. Not user is added if something fails.</li>
      <li>Add <code>Autowired</code> UserRepository to this class. Spring @Autowired annotation is used for automatic dependency injection. Spring framework is built on dependency injection and we inject the class dependencies through spring bean configuration file.</li>
      <li>Modify method <code>registerUser</code> to check email and create new <code>User</code> with userId.</li>
    </ul>
  </li>
  <li><code>UserRepositoryImpl</code>
    <ul>
      <li>Add <code>@Repository</code> to this class. Spring @Repository annotation is used to indicate that the class provides the mechanism for storage, retrieval, search, update and delete operation on objects.</li>
      <li>Create variable <code>SQL_CREATE</code> for SQL command.</li>
      <li><code>JdbcTemplate</code> - Class JdbcTemplate simplifies the use of JDBC and helps to avoid common errors. It executes core JDBC workflow, leaving application code to provide SQL and extract results.</li>
      <li>Interface <code>KeyHolder</code> - Interface for retrieving keys, typically used for auto-generated keys as potentially returned by JDBC insert statements.</li>
      <li>Class <code>GeneratedKeyHolder()</code>. The standard implementation of the KeyHolder interface, to be used for holding auto-generated keys (as potentially returned by JDBC insert statements).</li>
      <li>Interface <code>PreparedStatement</code>. An object that represents a precompiled SQL statement. A SQL statement is precompiled and stored in a PreparedStatement object. This object can then be used to efficiently execute this statement multiple times.</li>
      <li>Interface <code>Statement</code>. The object used for executing a static SQL statement and returning the results it produces.</li>
      <li>Interface <code>RowMapper</code>. RowMapper interface allows to map a row of the relations with the instance of user-defined class. It iterates the ResultSet internally and adds it into the collection. So we don't need to write a lot of code to fetch the records as ResultSetExtractor.</li>
    </ul>
  </li>
  <li>Update <code>UserResource</code> 
    <ul>
      <li>Use <code>@Autowired UserService userService;</code>. </li>
      <li>Update registerUser to return <code>ResponseEntity &lt; Map &lt; String,String>></code> instead of String. Class <code>ResponseEntity</code> is an extension of HttpEntity that adds an HttpStatusCode status code. Used in RestTemplate as well as in @Controller methods.</li>
    </ul>
  </li>
  <li>Save, build, and test api endpoint using Postman. Select POST with url "https://http://localhost:8080/api/users/register". Select Body->raw->text->JSON and input below</li>
  <li>
    {
      "firstName": "David",
      "lastName": "Smith",
      "email": "david@testmail.com",
      "password": "test123"
    }
  </li>
  <li>Had an error <code>Caused by: org.postgresql.util.PSQLException: FATAL: password authentication failed for user \"expensetracker\"</code>. Solved by reinstalling POSTgreSQL</li>
  <li>Check expensetrackerdb for user registered.
    <ul>
      <li>~<code>docker container exec -it postgresdb psql -U postgres</code></li>
      <li>~<code>\connect expensetrackerdb</code></li>
      <li>select * from et_users;</li>
      <li>View that the user was created.</li>
    </ul>
  </li>
  <li>In Postman, if you POST again, you will receive error message: "Email already in use".</li>
  <li>In Postman, if you POST again with wrong email pattern, you will receive error message: "Invalid email format".</li>
</ul>

4-Login and Hashing Password
<ul>
  <li>In <code>UserResource</code> create new method loginUser to return <code>ResponseEntity &lt; Map &lt; String,String>></code>.</li>
  <li>In <code>UserServiceImpl</code>, update <code>validateUser</code> to return a user using <code>userRepository.findByEmailAndPassword(email, password).</code></li>
  <li>In <code>UserRepositoryImpl</code>, update <code>findByEmailAndPassword</code></li>
  <li>Test with Postman.
    <ul>
      <li>POST to "http://localhost:8080/api/users/login"</li>
      <li>
        {
          "email": "david@testmail.com",
          "password": "test123"
        }
      </li>
      <li>Should receive successful message. If using wrong email/password, should receive error message "Invalid email/password".</li>
    </ul>
  </li>
  <li>Modify <code>pom.xml</code> and add dependency groupID:org.mindrot, artifact:jbcrypt, version:0.4</li>
  <li>Modify <code>UserRepositoryImpl</code>. 
    <ul>
      <li>In <code>create</code> method, create new variable <code>hashedPassword</code> using <code>BCrypt.hashpw()</code>.</li>
      <li>In <code>findByEmailAndPassword</code> method, modify if statement to check using <code>Bcrypt.checkpw()</code></li>
    </ul>
  </li>
  <li>Delete user from db
    <ul>
      <li>~<code>docker container exec -it postgresdb psql -U postgres</code></li>
      <li>~<code>\connect expensetrackerdb</code></li>
      <li>~<code>delete from et_users;</code></li>
    </ul>
  </li>
  <li>Test register with Postman.
    <ul>
      <li>POST to "http://localhost:8080/api/users/register"</li>
      <li>
        {
          "firstName": "David",
          "lastName": "Smith",
          "email": "david@testmail.com",
          "password": "test123"
        }
      </li>
      <li>Should receive <code>{"message": "registered successfully"}</code></li>
    </ul>
  </li>
  <li>Check expensetrackerdb for user registered.
    <ul>
      <li>~<code>docker container exec -it postgresdb psql -U postgres</code></li>
      <li>~<code>\connect expensetrackerdb</code></li>
      <li>select * from et_users;</li>
      <li>View that the user was created with a hashed password.</li>
    </ul>
  </li>
  <li>Test login with Postman.
    <ul>
      <li>POST to "http://localhost:8080/api/users/login"</li>
      <li>
        {
          "email": "david@testmail.com",
          "password": "test123"
        }
      </li>
      <li>Should receive successful message. If using wrong email/password, should receive error message "Invalid email/password".</li>
    </ul>
  </li>
</ul>







