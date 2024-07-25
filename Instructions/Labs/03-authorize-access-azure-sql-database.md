# Lab 03: Configure database authentication and authorization

## Lab scenario
The students will take the information gained in the lessons to configure and subsequently implement security in the Azure Portal and within the *AdventureWorks* database.

You've been hired as a Senior Database Administrator to help ensure the security of the database environment.

>**Note:** These exercises ask you to copy and paste T-SQL code. Please verify that the code has been copied correctly, before executing the code.

## Lab objectives

In this lab, you will complete the following tasks:

- Task 1: Authorize access to Azure SQL Database with Microsoft Entra ID
- Task 2: Manage access to database objects

## Estimated timing: 30 minutes

## Architecture diagram

![](../images/preview(03).png)

### Task 1 - Authorize access to Azure SQL Database with Microsoft Entra ID
 
1. On the Azure portal home page select **All resources**.

   ![Screenshot of the Azure portal home page, selecting All resources](../images/dp300-lab3-img2_update.png)

1. Search for **dp300 (1)** and select the SQL server **dp300-lab-<inject key="DeploymentID" enableCopy="false" /> (2)**

   ![Screenshot selecting Not Configured](../images/dp300-lab3-img3_update.png)

1. On the Overview page, select the **Not Configured** next to **Microsoft Entra Admin**.

   ![Screenshot selecting Not Configured](../images/DP-300-MED-1.png)

1. On the next screen, select **Set admin**.

   ![Screenshot selecting Set admin](../images/DP-300-MED-2.png)

1. In the **Microsoft Entra ID** sidebar, search for the Azure username you logged into the Azure portal with, then click on **Select**.

1. Select **Save** to complete the process. This will make your username the Microsoft Entra ID admin for the server as shown below.

   ![Screenshot of the Active Directory admin page](../images/DP-300-MED-3.png)

1. On the left navigation select **Overview (1)**, then copy the **Server name (2)** from right hand side.

   ![Screenshot showing where to copy the server name from](../images/DP-300-MED-4.png)

1. Select the Windows Start button and type **SSMS(1)**. Select **SQL Server Management Studio Management Studio 19 (2)** from the list.  

   ![Picture 34](../images/ssms-updated-19-lab03.png)

1. In the **Connect to Server** dialog box select **Database Engine** for Server type. In the **Server name** paste the name of your server you copied earlier. Change the authentication type to **Azure Active Directory - Universal with MFA**.For the **User name** field, select the Azure **Username** from the **Environment Details (1)** tab and click on **Connect**

   ![Screenshot of the Connect to server dialog](../images/dp300-lab3-img9.png)

    >**NOTE:** When you first try to sign in to an Azure SQL database, You may be asked to add your client IP address to the firewall. SQL Server Management Studio can do this for you. Use the Azure Portal **password** from the **Environment Details** tab, then select **Sign in**, choose your Azure credentials, and then select **OK**.
   
   ![Screenshot of adding the client IP address](../images/lab3_sql_pass.png)
   
> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
- Click the Lab Validation tab located at the upper right corner of the lab guide section and navigate to the Lab Validation Page.
- Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
- If not, carefully read the error message and retry the step, following the instructions in the lab guide.
- If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.

### Task 2 - Manage access to database objects

In this task, you will manage access to the database and its objects. The first thing you will do is create two users in the *AdventureWorksLT* database.

1. Use the **Object Explorer** and expand **Databases**.

1. Right-click on **AdventureWorksLT**, and select **New Query**.

   ![Screenshot of the new query menu option](../images/dp300-lab3-img11.png)

1. In the new query window, copy and paste the below T-SQL into it. **Execute** the query to create the two users.

    ```sql
    CREATE USER [DP300User1] WITH PASSWORD = 'Azur3Pa$$';
    GO

    CREATE USER [DP300User2] WITH PASSWORD = 'Azur3Pa$$';
    GO
    ```
    
    ![](../images/dp300-lab3-img12.png)
    
    >**Note:** These users are created in the scope of the AdventureWorksLT database. Next, you will create a custom role and add the users to it.

1. Execute the following T-SQL in the same query window.

    ```sql
    CREATE ROLE [SalesReader];
    GO

    ALTER ROLE [SalesReader] ADD MEMBER [DP300User1];
    GO

    ALTER ROLE [SalesReader] ADD MEMBER [DP300User2];
    GO
    ```


1. Next create a new stored procedure in the **SalesLT** schema. Execute the below T-SQL in your query window.

    ```sql
    CREATE OR ALTER PROCEDURE SalesLT.DemoProc
    AS
    SELECT P.Name, Sum(SOD.LineTotal) as TotalSales ,SOH.OrderDate
    FROM SalesLT.Product P
    INNER JOIN SalesLT.SalesOrderDetail SOD on SOD.ProductID = P.ProductID
    INNER JOIN SalesLT.SalesOrderHeader SOH on SOH.SalesOrderID = SOD.SalesOrderID
    GROUP BY P.Name, SOH.OrderDate
    ORDER BY TotalSales DESC
    GO
    ```

1. Next use the `EXECUTE AS USER` syntax to test out the security. This allows the database engine to execute a query in the context of your user. Execute the following T-SQL.

    ```sql
    EXECUTE AS USER = 'DP300User1'
    EXECUTE SalesLT.DemoProc
    ```

    >**Note:** This will fail with the message:

    ![Screenshot of the error message, The EXECUTE permission was denied on the object DemoProc](../images/dp300-lab3-img13.png)

1. Next grant permissions to the role to allow it to execute the store procedure. Execute the below T-SQL.

    ```sql
    REVERT;
    GRANT EXECUTE ON SCHEMA::SalesLT TO [SalesReader];
    GO
    ```

    >**Note:** The first command reverts the execution context back to the database owner.

1. Rerun the previous T-SQL.

    ```sql
    EXECUTE AS USER = 'DP300User1'
    EXECUTE SalesLT.DemoProc
    ```

   ![Screenshot showing the returned rows of data from the stored procedure](../images/dp300-lab3-img14_new.png)

   >**Results:** In this exercise, you've seen how you can use Microsoft Entra ID to grant Azure credentials  access to a SQL Server hosted in Azure. You've also used T-SQL statement to create new database users and granted them permissions to run stored procedures.

### Review

In this lab, you have completed:

- Authorized access to Azure SQL Database with Microsoft Entra ID.
- Managed access to database objects.

### You have successfully completed the lab.

