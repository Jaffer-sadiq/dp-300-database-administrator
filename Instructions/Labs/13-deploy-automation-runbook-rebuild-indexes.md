# Lab 13: Deploy an automation runbook to automatically rebuild indexes

## Lab scenario
You have been hired as a Senior Database Administrator to help automate day to day operations of database administration. This automation is to help ensure that the databases for AdventureWorks continue to operate at peak performance as well as provide methods for alerting based on certain criteria. AdventureWorks utilizes SQL Server in both Infrastructure as a Service (IaaS) and Platform as a Service (PaaS) offerings.

> **Note:** These exercises may ask you to copy and paste T-SQL code and makes use of existing SQL resources. Please verify that the code has been copied correctly, before executing the code.

## Lab objectives

In this lab, you will complete the following tasks:

- Task 1: Create an Automation Account
- Task 2: Connect to an existing Azure SQL Database
- Task 3: Configure Automation Account assets
- Task 4: Create a PowerShell runbook
- Task 5: Create a schedule for a runbook

## Estimated timing: 30 minutes

## Architecture diagram

![](../images/preview(13).png)

### Task 1 - Create an Automation Account

1. In the Azure portal in the search bar type **automation (1)** and then select **Automation Accounts (2)** from the search results, and then select **+ Create**.

    ![Screenshot of selecting the Automation Accounts.](../images/dp-300-L1302.png)

1. On the **Create an Automation Account** page, enter the information below, and then select **Review + Create (4)**.

    - **Resource Group:** **contoso-rg-<inject key="DeploymentID" enableCopy="false" />  (1)**
    - **Name:** autoAccount **(2)**
    - **Location:** Use the default. **(3)**

    ![Screenshot of the Add Automation Account screen.](../images/dp300-lab13-img1.png)

1. On the review page, select **Create**.

    ![Screenshot of the Add Automation Account screen.](../images/dp300-lab13-img2.png)
    
### Task 2 - Connect to an existing Azure SQL Database

1. In the Azure portal, navigate to your database by searching for **sql databases**.

   ![Screenshot of searching for existing SQL databases.](../images/sql.png)

1. Select the SQL database **AdventureWorksLT**.

    ![Screenshot of selecting the AdventureWorks SQL database.](../images/dp-300(5).png)

1. On the main section for your SQL Database page, select **Query editor (preview)**.

    ![Screenshot of selecting the Query editor (preview).](../images/dp300-lab13-img3.png)

1. You will be prompted for credentials to sign in to your database. Use this credential, and click on **OK**:

    - **Login:** sqladmin
    - **Password:** P@ssw0rd01

1. If You receive the following error message, follow these steps otherwise, start performing the steps from step-8:

    ![Screenshot of the sign in error.](../images/errorupdated01.png)

1. Then Select the **Allowlist IP ...** link provided at the end of the error message shown above. This will automatically add your client IP as a firewall rule entry for your SQL Database.

    ![Screenshot of the firewall rule creation.](../images/dp-300-8.png)

1. Return to the Query editor, and select **OK** to sign in to your database.



1.  Click on **Open query**, On the  **Open sql query**  select the file location **C:\LabFiles\Monitorandoptimize** and select **usp_AdaptiveIndexDefrag.sql** and click on **Open**, then select **OK**.

    > **Note:** The purpose of this script is to perform an intelligent defragmentation on one or more indexes, as well as required statistics update, for one or more databases.

1. Delete **ï»¿ (1)**,**`USE msdb` and `GO` (2)** on lines 1, 5 and 6 of the query (that are highlighted in the screenshot) , and then select **Run (3)**.

      ![Screenshot of pasting the code in a new Query window.](../images/query-editor1.png)

1. Expand the **Stored Procedures** folder to see what was created.

    ![Screenshot of the new stored procedures.](../images/dp-300-11.png)

### Task 3 - Configure Automation Account assets

The next steps consist of configuring the assets required in preparation for the runbook creation. Then select **Automation Accounts**.

1. On the Azure portal, in the top search box, type **automation**.

    ![Screenshot of selecting the Automation Accounts.](../images/dp-300-L1302.png)

1. Select the automation account that you created.

    ![Screenshot of selecting the autoAccount automation account.](../images/dp-300-13.png)

1. Select **Modules** from the **Shared Resources** section of the Automation blade. Then select **Browse gallery**.

    ![Screenshot of selecting the Modules menu.](../images/dp-300-14-01.png)

1. Search for **sqlserver** within the Gallery.

    ![Screenshot of selecting the SqlServer module.](../images/dp-300-16.png)

1. Select **SqlServer** which will direct to the next screen, and then select **Select**.

    ![Screenshot of selecting Select.](../images/dp-300-17.png)

1. On the **Add a module** page, under **runtime version** select the latest runtime version available **(1)**, then select **Import (2)**. This will import the PowerShell module into your Automation account.

   ![Screenshot of selecting Select.](../images/dp-300-18.png)

1. You'll need to create a credential to securely sign in to your database. From the blade for the Automation Account navigate to the **Shared Resources** section and select **Credentials**.

    ![Screenshot of selecting Credentials option.](../images/dp-300-19.png)

1. Select **+ Add a Credential**, enter the information below, and then select **Create (5)**.

    - Name: **SQLUser (1)**
    - User name: **sqladmin (2)**
    - Password: **P@ssw0rd01 (3)**
    - Confirm password: **P@ssw0rd01 (4)**

    ![Screenshot of adding account credentials.](../images/dp-300-20.png)

### Task 4 - Create a PowerShell runbook

1. In the Azure portal, navigate to your database by searching for **sql databases**.

    ![Screenshot of searching for existing SQL databases.](../images/sql.png)

1. Select the SQL database **AdventureWorksLT**.

    ![Screenshot of selecting the AdventureWorks SQL database.](../images/dp-300-22.png)

1. On the **Overview** page check the **Server name** of your Azure SQL Database as your server name should start with **dp300-lab**.

    ![Screenshot of copying the server name.](../images/dp300-lab13-img5.png)

1. On the Azure portal, in the top search box, type **automation**.

    ![Screenshot of selecting the Automation Accounts.](../images/dp-300-L1302.png)

1. Select the automation account that you created.

    ![Screenshot of selecting the autoAccount automation account.](../images/dp-300-25.png)

1. Scroll to the **Process Automation** section of the Automation account blade, select **Runbooks**, and then **+ Create a runbook**.

    ![Screenshot of the Runbooks page, selecting Create a runbook.](../images/dp-300-26.png)

    >**Note:** As we've learned, note that there are two existing runbooks created. These were automatically created during the automation account deployment.

1. Enter the runbook name as **IndexMaintenance (1)** and a runbook type of **PowerShell (2)**. Select the latest runtime version available **(3)**, then select **Create (4)**.

    ![Screenshot of creating a runbook.](../images/dp-30027.png)

1. Once the runbook has been created, copy and paste the Powershell code snippet below into your runbook editor. On the first line of the script paste in the **server name (1)** <inject key="sqlServerFqdn"></inject> . Select **Save (2)**, and then select **Publish (3)**, and then select **Yes**..

    >**Note:** Please verify that the code has been copied correctly, before saving the runbook.

    ```powershell
    $AzureSQLServerName = ''
    $DatabaseName = 'AdventureWorksLT'
    
    $Cred = Get-AutomationPSCredential -Name "SQLUser"
    $SQLOutput = $(Invoke-Sqlcmd -ServerInstance $AzureSQLServerName -UserName $Cred.UserName -Password $Cred.GetNetworkCredential().Password -Database $DatabaseName -Query "EXEC dbo.usp_AdaptiveIndexDefrag" -Verbose) 4>&1

    Write-Output $SQLOutput
    ```

    ![Screenshot of pasting code snipped.](../images/dp300-lab13-img6.png)

1. If everything goes well, you should receive a successful message.

    ![Screenshot of a successful message for the runbook creation.](../images/dp-300-29.png)

### Task 5 - Create a schedule for a runbook

Next you will schedule the runbook to execute on a regular basis.

1. Under **Resources** in the left hand navigation of your **IndexMaintenance** runbook, select **Schedules**. Then select **+ Add a schedule**.

    ![Screenshot of the Schedules page, selecting Add a schedule.](../images/dp-300-30.png)

1. Select **Link a schedule to your runbook**.

    ![Screenshot of selecting Link as schedule to your runbook.](../images/dp-300-31.png)

1. Select **+ Add a schedule**.

    ![Screenshot of the create a schedule link.](../images/dp-300-32.png)

1. Give the name **IndexMaintenanceSchedule**. 

1. Specify the start time of **4:00AM** of the following day and in the **United States - Pacific Time** time zone. Configure the reoccurrence for every **1** days. Do not set an expiration, select **Create**.

    ![Screenshot of the New Schedule pop out completed with example information.](../images/L13T5S5old.png)

1. The schedule is now created **(1)** and linked to the runbook. Select **OK (2)**.

    ![Screenshot of the created schedule.](../images/dp-300-333.png)

    Azure Automation delivers a cloud-based automation, and configuration service that supports consistent management across your Azure and non-Azure environments.
   
> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
- Click the Lab Validation tab located at the upper right corner of the lab guide section and navigate to the Lab Validation Page.
- Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
- If not, carefully read the error message and retry the step, following the instructions in the lab guide.
- If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.
  
>**Results:** By completing this exercise you've automated the defragging of indexes on a SQL server database to run every day, at 4am.

### Review

In this lab, you have completed:

- Created an Automation Account
- Connected to an existing Azure SQL Database
- Configured Automation Account assets
- Created a PowerShell runbook
- Created a schedule for a runbook

### You have successfully completed the lab.
