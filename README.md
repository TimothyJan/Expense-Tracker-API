# Expense-Tracker-API
Restful API for tracking expenses. Spring Boot Java - Tutorial - REST API using PostgreSQL and JWT


<a href="https://www.youtube.com/watch?v=fVq9aPNGLAg&list=UULFLCn3zEnB0h0Y2GVhTLtHkg&index=12">Installation process</a>
<ul>
  <li>Download Postman</li>
  <li>Download IntelliJ</li>
  <li>Download Docker. </li>
  <li>Download PostgreSQL. To check that postgresdb is working use~<code>docker container run --name postgresdb -e POSTGRES_PASSWORD=admin -d -p 5432:5432 postgres</code>
    <ul>When trying to run ~<code>docker pull postgres</code> got below error.
      <li><code>error during connect: This error may indicate that the docker daemon is not running.: Post "http://%2F%2F.%2Fpipe%2Fdocker_engine/v1.24/images/create?fromImage=postgres&tag=latest": open //./pipe/docker_engine: The system cannot find the file specified.</code></li>
      <li>Needed to use ~<code>"C:\Program Files\Docker\Docker\DockerCli.exe" -SwitchDaemon</code> from <a href="https://stackoverflow.com/questions/40459280/docker-cannot-start-on-windows">Stackoverflow Link</a>.</li>
      <li>Went through DockerDesktop Tutorial. Created a new container named "docker101tutorial".</li>
      <li>Reran ~<code>docker pull postgres</code> and it was completed.</li>
      <li>Ran ~<code>docker container ls</code> to view containers.</li>
    </ul>
  </li>
</ul>

<a>Project Setup & creating Database Objects</a>
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
  <li>Copy "expensetracker_db.sql" to the db container using ~<code>docker cp expensetracker_db.sql postgresdb:/</code></li>
  <li>Open docker bash using ~<code>docker container exec -it postgresdb bash</code></li>
  <li>In the bash, view the sql file using ~<code>ls</code></li>
</ul>

