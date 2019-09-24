# Azure-SQL-Database-Security-Demo

## Overview
Security is a top concern for managing databases, and it has always been a priority for Azure SQL Database. Your databases can be tightly secured to help satisfy most regulatory or security requirements. 



**Contents**
  - [Exercise 0: Introduction to Azure Portal](./README.md#exercise-0-introduction-to-azure-portal)
    - [Task 1: Sign Up for pre configured environment](./README.md#task-1-sign-up-for-pre-configured-environment)
    - [Task 2: Log into your Azure Portal and Verify access to the Subscription](./README.md#task-2-log-into-your-azure-portal-and-verify-access-to-the-subscription)
  
  - [Exercise 1: Getting Started](./README.md#exercise-1-perform-database-assessments)
    - [Task 1: Review Azure SQL Database](./README.md#task-1-review-azure-sql-database)
  - [Exercise 2: Control Access](./README.md#exercise-2-control-access)
    - [Task 1:Configure Azure AD Login for your Azure SQL DB](./README.md#task-1-configure-azure-ad-login-for-your-azure-sql-db)
    - [Task 2: Access the Database using SQL Server Management Studio](./README.md#task-2-access-the-database-using-sql-server-management-studio)
  - [Exercise 3: Protect Data](./README.md#exercise-3-protect-data)
    - [Task 1: Transparent Data Encryption](./README.md#task-1-transparent-data-encryption)
    - [Task 2: Always Encrypted](./README.md#task-2-always-encrypted)
    - [Task 3: Auditing](./README.md#task-3-auditing)
    - [Task 4: Threat Protection](./README.md#task-4-threat-protection)
    - [Task 5: Configure SQL Data Discovery and Classification](./README.md#task-5-configure-sql-data-discovery-and-classification)
    - [Task 6: Simulate Attack](./README.md#task-6-simulate-attack)
    - [Task 7: Vulnerability Scan](./README.md#task-7-vulnerability-scan)
    - [Task 8: Configure Dynamic Data Masking](./README.md#task-8-configure-dynamic-data-masking)
    - [Task 9: Verify data how does it look by using App](./README.md#task-9-verify-data-how-does-it-look-by-using-app)
 
## Exercise 0: Introduction to Azure Portal

This exercise will take you through Azure login and portal experience and the pre-requisite environment.

   * [Task 1: Sign Up for Pre-configured Environment](#exercise-01-sign-up-for-pre-configured-environment)
   * [Task 2: Log into your Azure Portal and Verify access to the Subscription](#exercise-02-log-into-your-azure-portal-and-verify-access-to-the-subscription)
 

### Task 1: Sign Up for pre configured environment

1.	Navigate to bitly link which was provided by instructor and register by providing all required information and clicking on **SUBMIT** button.<br/>

![](images/1registernow.png)

2. Once registration is accepted, you will be automatically redirected to the lab activation page. Click on the **Launch Lab** button.<br/>

![](images/2launchlab.png)

3. You will see the environment details soon below.<br/>

![](images/labdetailpg.png)

Please ensure to take the values assigned to your deployment.


### Task 2: Log into your Azure Portal and Verify access to the Subscription

In this exercise, you will log into the **Azure Portal** using your Azure credentials.

1.  Navigate to https://portal.azure.com.
2.  Enter the **Username** which was displayed in the previous window and click on **Next**.<br/>

![](images/1signin.png)

3. Enter the **Password** and click on **Sign in**.<br/>

![](images/2signin.png)

4.	In the Welcome to **Microsoft Azure** pop-up window, click **Maybe Later**. Now you have login successfully.

![](images/maybelater1.png)

5. You will see two Resource Groups on which you have access. 
6. Click on **ODL-sql-security-xxxxx-SQL** Resource Group which contains the pre-deployed Azure SQL Database as shown below:

![](images/overview1.png)

7. Click on **ODL-sql-security-xxxxx-JumpVM** Resource Group which contains the pre-deployed on-premises infrastructure.

![](images/overview2.png)


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

5. Then go back to your SQL Server **contososerv-suffix**.
6. Select **Firewall and virtual networks**. Then select **ON** for **Allow Access to Azure Services** to enable firwall, then add fiirwall IP that ranges between the **Client IP address** you see in you sql server. 

![](images/firewalladip.png)

7. **Save** the changes.

![](images/firewallsave.png)

8. Now go back to **JumpVM** and login again, you will get login succesfully. Also you can review you SQL Database **Clinic** by expanding **Database** 

![](images/successfullogin.png)

## Exercise 3: Protect Data 


### Task 1: Transparent Data Encryption 

Transparent data encryption (TDE) helps protect Azure SQL Database against the threat of malicious offline activity by encrypting data at rest. It performs real-time encryption and decryption of the database, associated backups, and transaction log files at rest without requiring changes to the application

1. In the Azure Portal, go to Resource Group with suffix **-SQL**, select the SQL Database **Clinic**.
2. Select **Transparent data encryption** under the **Security** blade.
3. Here you can review; transparent data encryption is already enabled. 

![](images/transdataenc.png)

### Task 2: Always Encrypted
1. Go to **JumpVM**, then select your database **Clinic** > **Tables** > **dbo.Patients** > **Encrypt Columns**.

![](images/alwaysenc.png)

2. You will get a pop-up window, where you will enable encryption. So select **Next**.

![](images/alwaysenc1.png)

3. Check the box for **Birth Date** and for **choose type** select **Randomized**.

![](images/alwaysenc2.png)

4. Select **Next**. On next step, select **Azure Key Vault** and then click on **sign in** button.

![](images/alwaysenc3.png)

5. An authentication window will appear on the screen. Enter you username and password there.

img

6. Next, select the Key Vault provided you in resource group **-SQL** from the dropdown.



### Task 3: Auditing
1. In SQL Database, select **Auditing** under **Security** where you can review that Auditing is enabled and data is being stored in your storage account.

![](images/auditing.png)

2. Click on **View Audit Logs**, this will show all the database activities happened recently.

![](images/viewaudit.png)


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

7. To check how this works in respect to the app service, will go to the app service **contosoapp-suffix**.
8. Select **Browse**, this will open **Contoso Clinic** webpage.

![](images/contosowebpage.png)

9. Now navigate to **Patients**. Then perform an operation by selecting **Details** of any patient where this will be reflected in the audit log in auditing. 

![](images/patients.png)

10. The actions performed on the webpage will directly get logged into audit logs which can be reviewed. 
11. In SQL Database **Clinic**, select **Auditing** under **Security** where you can review the audit logs. Select **View Audit Logs**.

![](images/viewauditlogs.png) 

12. You can review the recently happened activites.

![](images/auditafterthreat1.png)

13. Then expand first activity, which will further show you details about the operation you performed in previous step (Step 9). It will refer to what kind of information is accessed.

![](images/auditafterthreat2.png)

This is how one can review all the activites, that which application has access the confidential information.

### Task 6: Simulate Attack 

1.	In the Azure Portal, open Resource Group with suffix **-SQL**, navigate to the app service **contosoapp-suffix** and select **Browse**.

![](images/appservice.png)

2.	You will be directed to **Contoso Clinic** webpage, select **Patients**, 

![](images/contosowebpage.png)

3.	In the **Search Box**, put the following code and  click on **Search**.
**' UNION SELECT '0', '1', '2', STUFF((select name from sys.tables FOR XML PATH('')),1,1,''), '4', '5', '6', '7', '8', '2010-10-10' --**

![](images/searchbox.png)

4. This will create an SQL Injection in the database.

5. You will receive an alert email regarding the threat, to review that navigate to https://portal.office.com.
6. Select **Outlook**.

![](images/outlook.png)

7. Then review the email which shows details about the threat.

![](images/mailerror.png)

8.	Also, the threat can be reviewd from **Azure Portal**. To check through the portal, go to SQL Database **Clinic**.
9. Then go to **Advanced Data Security** under Security blade, select **Advanced Threat Protection**. Here you can review the threat.

![](images/advthreatpro.png)
 
10. By selecting **Advanced Threat Protection**, you can review the threat as shown below in the image. Select the **Potential SQL Injcetion**.

![](images/threat1.png)

11. Here is the database in which threat is found. Select the sql database **Clinic**.

![](images/threat2.png)

12. In this section you will get the information about threat.

![](images/threat3.png)


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
**Verify data how does it look by logging in as non-admin user in SQL MGMT Studio**
