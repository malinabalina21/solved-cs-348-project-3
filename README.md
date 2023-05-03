Download Link: https://assignmentchef.com/product/solved-cs-348-project-3
<br>
In this project, you will implement a simple access control scheme (role-based) for the given database schema. The project has two goals:

<ul>

 <li>Using JDBC to query/update your database from within a Java program</li>

 <li>Be able to simulate an admin-user interaction where the admin creates roles and grant privileges to users.</li>

</ul>

<h1>Details      of      the   Project</h1>

<h2>Access          Control        Mechanism</h2>

In this project, you will simulate an admin-user interaction, where the operations a user can perform on any data are determined by the user’s access roles. This is popularly called Role Based Access Control Protocol. Here we see that user can perform a said operation on a said table only if he has the access right to perform that operation on that table. Users can have multiple roles and are entitled to all privileges of their assigned roles. Here, users of the database are assigned roles by a single administrator, who also assigns specific permissions regarding the use of the database tables to each role. The operations regular users of the database can perform on the data are restricted to INSERT and SELECT for this project. There is an OwnerRole column in all the user tables and when a SELECT command is issued and a user has the SELECT privilege for that table, he/she should still not be able to see the OwnerRole column.

<h2>Connecting to        your  Oracle          database</h2>

For this project, you will use your Oracle account to host the database of the application. Your program should connect to this database using a database connection library (JDBC). To get started using JDBC, you can go through the tutorial at <u>http://www.tutorialspoint.com/jdbc/jdbc-quick-guide.htm</u>. This tutorial should provide the sufficient knowledge about JDBC for this project’s purposes.




Note that when registering the database driver in your code, you should pass the argument <strong>“oracle.jdbc.OracleDriver” to the Class.forName() method</strong>, as you will be connecting to an Oracle database. Also, you will need to include the JDBC driver <strong>ojdbc8.jar</strong> (provided to you) in your classpath at runtime to be able to use the database connectivity function.




You should use the URL <strong>“jdbc:oracle:thin:@claros.cs.purdue.edu:1524:strep”</strong> with the getConnection method of the DriverManager class when connecting to the database and <strong>include your Oracle account username (without @csora) and password</strong> as the username and password parameter values.

<h1>Database  Schema</h1>

The database in this project consists of the tables listed below. The scripts to create/drop the database are already provided to you.

<h2>Admin          tables</h2>

These tables are for administrative tasks only and are not accessible by regular users: Users, Roles, UserRoles, Privileges, and UserPrivileges.

<h2>User  tables</h2>

These are accessible by regular users. The schema uses a hospital environment and the tables are named in that way. The users will need to have the appropriate permissions to insert data into or view data from these tables. Note again that <strong>the OwnerRole column should not be output when a SELECT command is issued by any user</strong>.




Note: You can create your own corner testcases to further test  your code.




<h2>Command Set</h2>

Your program should accept input from a text file (with each command terminated with a newline character), process the input and produce as output the responses in another text file. Ultimately, your submission should be executed as follows:

java -cp .:ojdbc8.jar Project3 input.txt output.txt

Sample input and output files are also provided for you. input.txt is the file containing the commands and output.txt is a file you are going to create. output.txt should include the output for each command separated by newlines, and ends with a newline after the “EXIT” command. For example, if we have three commands in input.txt before the “EXIT” command, output.txt should look like




Notice that “Command 1” is the actual first command text you read from input.txt. Following any other format will result in a problem and eventually delay in grading.




The program should implement the command set listed below. Note that the commands will always be input in the correct format, i.e. you do not need to check for syntax errors.




<h3>● LOGIN username password</h3>




When this command is issued, you need to check that the provided username and password pair matches the one in the Users table of the Admin tables in the database. If not, you should print the error message “<strong>Invalid login</strong>” (use this format exactly). If a match is found, the current user should be made to the one specified by this username and also print the exact message “<strong>Login successful</strong>”.

Note that you should be having a record in your Users table for the admin user (UserId=1, Username=’admin’, Password=’password’). You should also be having  a record for the ADMIN role in the Roles table (RoleId=1, RoleName=’ADMIN’). You should have a record in your UserRoles table, for which the UserId is 1 and the RoleId is 1 (this means that the user admin has role ADMIN). A script with this data initialization is also provided with this handout. While checking whether the current user is the admin, you can either check that the username is admin or that the user has the ADMIN role in the UserRoles table (either approach will fetch points). Note that if the login command is issued before a user quits the program, you should just switch the current user to the user specified in the login command. You can assume that if the login is unsuccessful, the user will keep trying to login until it succeeds. <strong>The very first command issued to your program will always be the login command. </strong>




<h3>● CREATE ROLE roleName</h3>




If the current user is the admin when this command is issued, you should insert a new record in the Roles table with the values (roleName) and print “<strong>Role created successfully</strong>” (generate role Id dynamically). If the current user is not the admin, then print the error message “<strong>Authorization failure</strong>”.




<h4>● <strong>CREATE USER</strong> username password</h4>




If the current user is the admin when this command is issued, you need to insert a new record in the Users table with the values (username, password) and print “<strong>User created successfully</strong>” (generate user id dynamically). If the current user is not the admin, then print the error message “<strong>Authorization failure</strong>”.




<h4>● <strong>ASSIGN ROLE</strong> username roleName</h4>




If the current user is the admin when this command is issued, you should insert a new record in the UserRoles table with ids corresponding to (username, roleName) and print “<strong>Role assigned successfully</strong>”. If the current user is not the admin, print the error message “<strong>Authorization failure</strong>“.




<h4>● <strong>GRANT PRIVILEGE</strong> privName <strong>TO</strong> roleName <strong>ON</strong> tableName</h4>




In the already populated Privileges table (with two rows)

○Insert Privilege: Id = 1, Name = INSERT

○Select Privilege: Id = 2, Name = SELECT

If the current user is the admin when this command is issued, you should insert a new record in the UserPrivileges table with values corresponding to the parameters (roleName, privName, tableName) and print “<strong>Privilege granted successfully</strong>“. If the current user is not the admin, print the error message “<strong>Authorization failure</strong>”.




<h4>● <strong>REVOKE PRIVILEGE</strong> privName <strong>FROM</strong> roleName <strong>ON</strong> tableName</h4>




If the current user is the admin when this command is issued, you need to delete the record in the UserPrivileges table with values corresponding to the parameters (roleName, tableName, privName) and print “<strong>Privilege revoked successfully</strong>“. If the current user is not the admin, print the error message “<strong>Authorization failure</strong>”.




<h4>● <strong>INSERT INTO</strong> tableName <strong>VALUES </strong>(valueList) <strong>GET </strong>ownerRole</h4>




Upon issuance of this command, you should first check if any of the roles of the current user has the “INSERT” privilege on the table tableName. If not, you should print the error message “<strong>Authorization failure</strong>”. Otherwise, you should insert the values in valueList (which is a list of comma-separated strings enclosed in single quotes) into the table tableName. While inserting the tuple, you should set the OwnerRole attribute value to the id corresponding to ownerRole. Your output should be “<strong>Row inserted successfully</strong>” if the user is authorized.




<h3>● SELECT * FROM tableName</h3>




Upon issuance of this command, you should first check if any of the roles of the current user has the “SELECT” privilege on the table tableName. If not, you should print the error message “<strong>Authorization failure</strong>”. Otherwise, you should print the names of the attributes in this table (in upper case) on a line by itself, each separated by a comma and all records in the table tableName, one record per line, with each attribute value separated by a comma (attribute values in the same order as the attribute names listed on the first line). Note that OwnerRole should NOT be printed to output. You can assume that there will be at least one record in the table tableName when this command is issued.




<h3>●EXIT</h3>




Your program should terminate upon issuance of this command (you should ignore any command that appears after the EXIT command in the input file).




<strong> </strong>

<h1>Submission      Instructions</h1>

<strong>Please follow these instructions strictly. </strong>

<strong>Grading is automated, and your grade would be significantly affected otherwise.</strong>




<ul>

 <li>The project should be implemented in Java and your main file should be named <strong>java</strong>.</li>

</ul>




<ul>

 <li>Before you submit your project, make sure to delete all data from your database tables, and drop all the tables, except for the data in the initialization script (To guarantee a completely clean database before testing your submission).</li>

</ul>




<ul>

 <li>If needed, create a README file containing identifying information, and include anything you might want us to know when grading your project.</li>

</ul>




<ul>

 <li>A sample run.sh script is given. This is used to clean and initialize the database, and compile and run your code. Make sure you edit the file to add your oracle username and password. As shown in the script file:</li>

</ul>

Your submission should be compiled using the following command javac -cp .:ojdbc8.jar Project3.java

and should be run using the following command

java -cp .:ojdbc8.jar Project3 input.txt output.txt




<ul>

 <li>If you add other java files, you might need to edit the run.sh file to compile these too.</li>

</ul>




<ul>

 <li>Please submit on Blackboard a ZIP file named “username_project3.zip” where username is your Purdue career account username (i.e. <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="40353325322e212d25003035322435256e252435">[email protected]</a>). The ZIP file should contain <strong>ONLY</strong> the following files with no subfolders:</li>

</ul>

○ <strong>Project3.java</strong> (Your java main file) and any other .java files you might be using

○ The modified <strong>run.sh</strong> file with your username and password

○ [Optional]<strong> README</strong>: your readme file, if needed