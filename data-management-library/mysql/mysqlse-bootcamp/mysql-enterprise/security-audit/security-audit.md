# SECURITY - MYSQL ENTERPRISE AUDIT

## Introduction
3d) MySQL Enterprise Audit
Objective: Auditing in action…
Server: serverB


Estimated Lab Time: -- minutes

### Objectives

In this lab, you will:
* Setup Audit Log
* Use Audit


### Prerequisites


This lab assumes you have:
* An Oracle account
* All previous labs successfully completed

**Notes:**
- Audit can be activated and configured without stop the instance. In the lab we edit my.cnf to see how to do it in this way

**Server:** serverB


## Task 1: Setup Audit Log

1. Enable Audit Log on mysql-advanced (remember: you can’t install on mysql-gpl).  Audit is COMMERCIAL plugin.

    a. Edit the my.cnf setting in /mysql/etc/my.cnf

    **shell>** 
    ```
    <copy>sudo nano /mysql/etc/my.cnf</copy>
    ```
    b. Change the line “plugin-load=thread_pool.so” to load the plugin

    ```
    <copy>plugin-load=thread_pool.so;audit_log.so</copy>
    ```
    c. below the previous add these lines to make sure that the audit plugin can't be unloaded and that the file is automatically rotated at 20 MB

    ```
    <copy>audit_log=FORCE_PLUS_PERMANENT</copy>
    ```
    ```
    <copy>audit_log_rotate_on_size=20971520</copy>
    ```
    d. Restart MySQL (you can configure audit without restart the server, but here we show how to set the configuration file)

    **shell>** 
    ```
    <copy>sudo systemctl restart mysqld-advanced</copy>
    ```
## Task 2: Use Audit 

1. Login to a mysql-advanced with the user “appuser”, then submit some commands

    a. **shell>** 
    ```
    <copy>mysql -u appuser2 -p -h 127.0.0.1 -P 3307</copy>
    ```
    b. **mysql>** 
    ```
    <copy>USE world;</copy>
    ```
    c. **mysql>** 
    ```
    <copy>SELECT * FROM city limit 25;</copy>
    ```
    d. **mysql>** 
    ```
    <copy>SELECT Code, Name, Region FROM country WHERE population > 200000;</copy>
    ```
2. Open the audit.log file the datadir and verify the content

    **shell>** 
    ```
    <copy>nano /mysql/data/audit.log</copy>
    ```
3. In addition, if you'd like to reduce the amount of data generated by the Audit Plugin you can use filters. As an example, to just trace 'Login' add this line to my.cnf file

    ```
    <copy>audit_log_policy=Login</copy>
    ```
4. Now restart the server and test again

    **shell>** 
    ```
    <copy>sudo systemctl restart mysqld-advanced</copy>
    ```
5. You can check the documentation about other Log policies

## Learn More

*(optional - include links to docs, white papers, blogs, etc)*

* [URL text 1](http://docs.oracle.com)
* [URL text 2](http://docs.oracle.com)

## Acknowledgements
* **Author** - Perside Foster, MySQL Engineering
* **Contributors** -  Marco Carlessi, MySQL Engineering
* **Last Updated By/Date** - <Perside Foster, October 2021
