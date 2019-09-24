# Azure-SQL-Database-Security-Demo

## Overview
Security is a top concern for managing databases, and it has always been a priority for Azure SQL Database. Your databases can be tightly secured to help satisfy most regulatory or security requirements. 



**Contents**
  - [Exercise 0: Introduction to Azure Portal](./Exercise-0-Intorduction-To-Azure-Portal-readme.md#exercise-0-introduction-to-azure-portal)
    - [Task 1: Sign Up for pre configured environment](./README.md#task-1-review-azure-s)
    - [Task 2: Log into your Azure Portal and Verify access to the Subscription](./README.md#task-1-review-azure-s)
  
  - [Exercise 1: Getting Started](./README.md#exercise-1-perform-database-assessments)
    - [Task 1: Review Azure SQL Database](./README.md#task-1-review-azure-sql-database)
  - [Exercise 2: Control Access](./README.md#exercise-2-control-access)
    - [Task 1:Configure Azure AD Login for your Azure SQL DB](./README.md#task-1-configure-azure-ad-login-for-your-azure-sql-db)
    - [Task 2: Access the Database using SQL Server Management Studio](./README.md#task-2-access-the-database-using-sql-server-management-studio)
  - [Exercise 3: Protect Data](./README.md#exercise-3-protect-data)
    - [Task 1: Transparent Data Encryption](./README.md#task-1-transparent-data-encryption)
    - [Task 2: Auditing](./README.md#task-2-auditing)
    - [Task 3: Threat Protection](./README.md#task-3-threat-protection)
    - [Task 4: Configure SQL Data Discovery and Classification](./README.md#task-4-configure-sql-data-discovery-and-classification)
    - [Task 5: Simulate Attack](./README.md#task-5-simulate-attack)
    - [Task 6: Vulnerability Scan](./README.md#task-6-vulnerability-scan)
    - [Task 7: Configure Dynamic Data Masking](./README.md#task-7-configure-dynamic-data-masking)
    - [Task 8: Verify data how does it look by using App](./README.md#task-8-verify-data-how-does-it-look-by-using-app)
 
## Exercise 1: Perform database assessments

### Task 1: Review Azure SQL Database

1. In the Azure Portal, navigate to the Resource Groups. There are two resource groups, select the resource group with suffix **-SQL**.

![](images/rgs.png)

2. Locate a resource named **Clinic**. This is your **Azure SQL Database**.

![](images/clinicsqldatabase.png)

## Exercise 2: Control Access

### Task 1: Configure Azure AD Login for your Azure SQL DB

1. Login into the Azure Portal, open **Resource Group** with suffix **-SQL**, navigate to the SQL Server **contososerv-suffix**.
1. Under the **Settings** blade, select **Active Directory Admin**. Here you can review your username registered as Active Directory Admin.

![](images/activediradmin.png)


### Task 2: Access the Database using SQL Server Management Studio

1. Now, login to **JumpVM** by clicking on **GO TO JumpVM** button on lab details page. 

![](images/gotojumpvm.png)

2. On start bar, search SQL and select **Microsoft SQL Server Management Studio 18**.
3. Use the following configurations then click Connect:
* Server name: enter the server name which you can copy from the overview page of SQL Server at the top right corner.

![](images/servername.png)


* authentication method: from dropdown select **Active Directory-Password**
* username: **odl_user_xxxxx@xxxxxxxxxxx.xxxxxxxxxxx.com**
* password: **arno32QCO*R1**

![](images/sqlauthentiction.png)


4.	You will get a login failure which will state: ***Your client IP address does not have access to the server. Sign in to an Azure Account and create a new firewall rule to enable access.***

![](images/firewallerror.png)

5. Then go back to your SQL Server **contososerv-suffix**, select **Firewall and virtul networks**.

img

6. **Allow Access to Azure Services** - Select **ON** to enable firwall, then add fiirwall IP 

img

***iv.	Go back to SQL server, under security blade select Firewalls and virtual networks.?????
v.	Should we show a login failure first and then add firewall IP? 
vi.	Show the login failure due to firewall then add the rule.***

## Exercise 3: Protect Data 


### Task 1: Transparent Data Encryption 

Transparent data encryption (TDE) helps protect Azure SQL Database against the threat of malicious offline activity by encrypting data at rest. It performs real-time encryption and decryption of the database, associated backups, and transaction log files at rest without requiring changes to the application

1. In the Azure Portal, go to Resource Group with suffix **-SQL**, select the SQL Database **Clinic**.
2. Select **Transparent data encryption** under the **Security** blade.
3. Here you can review; transparent data encryption is already enabled. 

![](images/transdataenc.png)

### Task 2: Always Encrypted
1. Go to **JumpVM**, then select your database **Clinic** > **Tables** > **dbo.Patients** > **Encrypt Columns**.

img

2. You will get a pop-up window, where you will enable encryption. So select **Next**.

img

3. Check the box for **Birth Date** and for choose type select **Randomized*.

img

4. Then on next step, select **Azure Key Vault** and then select **sign in**.

img

5. An authentication window will appear on the screen. Enter you username and password there.

img

6. Next, select the Key Vault provided you in resource group **-SQL** from the dropdown.



### Task 3: Auditing
1. In SQL Database, select **Auditing** under **Security** where you can review that Auditing is enabled and data is being stored in your storage account.
2. Click on **View Audit Logs**, this will show all the database activities happened recently.

![](images/auditing.png)


### Task 4: Threat Protection 
1. Open Resource Group with suffix **-SQL**, navigate to the SQL Server. Select **Advanced Data Security** under Security.
2. Use the following configurations:
* Advanced Data Security: **On**
* Subscription: **Choose your subscription**
* Storage Account: **Choose your storage account**
* Periodic recurring scans: **On**
* Send scan reports to: **username**
* Send Alerts to: **username**

![](images/advancedsecurity.png)

3. Select **Save**.


### Task 5: Configure SQL Data Discovery and Classification

In this task, you will look at the SQL Data Discovery and Classification feature that introduces a new tool for discovering, classifying, labeling & reporting the sensitive data in your databases. It introduces a set of advanced services, forming a new SQL Information Protection paradigm aimed at protecting the data in your database.

1. Go to SQL Database, on the **Advanced Data Security** blade, select the **Data Discovery & Classification** tile.

![](images/dataclassification.png)


2. In the Data Discovery & Classification blade, select the info link with the message **We have found 16 columns with classification recommendations**.

![](images/16columns.png)


3. Look over the list of recommendations to get a better understanding of the types of data and classifications that can be assigned, based on the built-in classification settings.
4. Check the **Select all** check box at the top of the list to select all the remaining recommended classifications, and then select **Accept selected recommendations**.

![](images/acceptrecom.png)


5. Select **Save**.
6. When the save completes, select the **Overview** tab on the Data Discovery & Classification blade to view a report with a full summary of the database classification state.


**Verify by running SQL Query**


### Task 6: Simulate Attack 

1.	In the Azure Portal, open Resource Group with suffix **-SQL**, navigate to the app service **contosoapp-suffix** and select **Browse**.

![](images/appservice.png)

2.	You will be directed to **Contoso Clinic** webpage, select **Patients**, 

![](images/contosowebpage.png)
3.	Select **SQLi Hints**.

![](images/sqlihints.png)

4. Few text formats will appear which will let you create SQL ingections that will result into an attack.

![](images/sqlihints(2).png)

5.	Pick one of them and paste in the search box and select **Search**.

![](images/searchbox.png)

6.	Now go back to SQL Database **Clinic**. .
7.	Go to **Advanced Data Security** under Security blade and select **Advanced Threat Protection**.

![](images/advthreatprot.png)

8. Here you can review the alert , also this will be shown as a threat. It will take few minutes to get fetched.


### Task 7: Vulnerability Scan

The SQL Vulnerability Assessment service is a service that provides visibility into your security state, and includes actionable steps to resolve security issues, and enhance your database security.

1.	Return to the **Advanced Data Security** blade and then select the **Vulnerability Assessment** tile.
1.	On the Vulnerability Assessment blade, select **Scan** on the toolbar.
1.	When the scan completes, you will see a dashboard, displaying the number of failing checks, passing checks, and a breakdown of the risk summary by severity level.
1.	__________

1.	Run a SQL Injection Alert 
1.	You'll receive an email with security alert (lab user has mailbox) 
1.	Review the security alerts in ATP 
1.	Review Audit Logs
1.	Run a Vulnerability Scan and review results. 


### Task 8: Configure Dynamic Data Masking: 
 
In this exercise, you will enable Dynamic Data Masking (DDM). DDM limits sensitive data exposure by masking it to non-privileged users. This feature helps prevent unauthorized access to sensitive data by enabling customers to designate how much of the sensitive data to reveal with minimal impact on the application layer. It’s a policy-based security feature that hides the sensitive data in the result set of a query over designated database fields, while the data in the database is not changed.

1. Now navigating to SQL Database **Clinic** to add a mask.
2. Go to **Dynamic Data Masking** under **Security** blade, then select **+Add Mask**.

![](images/ddm.png)


2. Use following configurations to create a mask rule:
* Schema: **dbo**
* Table: select **Patients** from dropdown
* Column: **SSN (char)** from dropdown

![](images/mask1.png)

3. Select **Add**.
4. Add a mask again by selecting **+Add Mask**.
5.  Use following configurations:
* Table: select **Visits** from dropdown
* Column: **PatientID (int)** from dropdown
* Masking field format: **Number (random number range)**
* Enter random value for **To** and **For**.

![](images/mask2.png)

6. Then select **Add**. 
Your mask rules are ready.

### Task 9: Verify data how does it look by using App
1. Go back to your app service, select **Visits**.
