

# Installation
* Download the Desktop Engine (see [Links](ms_sql_server_2000_desktop_engine.md#links))
* Extract the files by running the downloaded executable
* Edit the **setup.ini** file and add a strong password for the *sa* account:

```text
 SAPWD=*password*
```
>  Note: 
the default password is empty, which can prevent the setup from continuing the installation

* Run the setup

# Testing
* [This](http://support.microsoft.com/default.aspx?scid=kb;en-us;313100) article lists Java code for testing the connection

# Troubleshooting
* **Error Establishing Socket with JDBC Driver**
> Add TCP/IP to the list of protocols as stated in [this](http://support.microsoft.com/default.aspx?scid=kb;en-us;313178) article
* **Login failed for user 'sa'. Reason: 
Not associated with a trusted SQL Server connection.**
> For changing the authentication to mixed mode see [this](http://support.microsoft.com/kb/319930/en-us) article

# Links
* [Microsoft SQL Server 2000 (Desktop Engine)](http://www.microsoft.com/downloads/details.aspx?familyid=8e2dfc8d-c20e-4446-99a9-b7f0213f8bc5)
* [Microsoft SQL Server 2000 JDBC Driver SP 3](http://www.microsoft.com/downloads/details.aspx?familyid=07287b11-0502-461a-b138-2aa54bfdc03a)
