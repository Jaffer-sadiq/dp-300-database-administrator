# Lab 02: Provision an Azure SQL Database

## Lab scenario
Students will configure basic resources needed to deploy an Azure SQL Database with a Virtual Network Endpoint. Connectivity to the SQL Database will be validated using Azure Data Studio from the lab VM.

As a database administrator for AdventureWorks, you will set up a new SQL Database, including a Virtual Network Endpoint to increase and simplify the security of the deployment. Azure Data Studio will be used to evaluate the use of a SQL Notebook for data querying and results retention.

## Lab objectives

In this lab, you will complete the following tasks:

- Task 1: Create a Virtual Network
- Task 2: Provision an Azure SQL Database
- Task 3: Enable access to an Azure SQL Database
- Task 4: Connect to an Azure SQL Database in Azure Data Studio
- Task 5: Query an Azure SQL Database with a SQL Notebook

## Estimated timing: 45 minutes

## Architecture diagram

![](../images/preview(02).png)

### Task 1 - Create a Virtual Network

1. In the Azure portal home page, select the **left hand menu.**

     ![Picture 2](../images/upd-dp-300-module-02-lab-01_1.png)

2. In the left navigation pane, click **Virtual Networks**

3. Click **+ Create** to open the **Create Virtual Network** page. On the **Basics** tab, complete the following information:
   
   **Note** : **Please make sure not to include additional spaces at the end when copying the parameters!!**

- **Subscription:** Use existing subscription
- **Resource group:** **contoso-rg-<inject key="DeploymentID" enableCopy="false"/>** **(1)**
- **Name:** **lab02-vnet** **(2)**
- **Region:** <inject key="location" enableCopy="false"/> **(3)**

    ![Picture 2](../images/upd-dp-300-module-02-lab-05.png)

4.  Click **Review + Create**, review the settings for the new virtual network, and then click **Create**.

5. Configure the virtual network’s IP range for the Azure SQL database endpoint by navigating to the virtual network created, and on the **Settings** pane, click **Subnets**.

6. Click on the **default** subnet link. Note that the **Subnet address range** you see might be different.

7. Under **Service Endpoints** pane on the right, expand the **Services** drop-down, and select **Microsoft.Sql**. Select **Save**.

   ![Picture 16](../images/12.png)

### Task 2 - Provision an Azure SQL Database

1. From the Azure Portal, search for “SQL databases” in the search box at the top, then click **SQL databases** from the list of options.

    ![Picture 5](../images/upd-dp-300-module-02-lab-10.png)

2. On the **SQL databases** blade, select **+ Create**.

    ![Picture 6](../images/upd-dp-300-module-02-lab-10_1.png)

3. On the **Create SQL Database** page, select the following options on the **Basics** tab and then click **Next: Networking**.

     - **Subscription:** Use existing subscription
     - **Resource group:** **contoso-rg-<inject key="DeploymentID" enableCopy="false"/>**
     - **Database Name:** AdventureWorksLT
     - **Server:** click on **Create new** link. The **Create SQL Database Server** page will open. Provide the server details as follow:
          - **Server name:** dp300-lab-<inject key="DeploymentID" enableCopy="false"/> **(1)**
          - **Location:**  <inject key="location" enableCopy="false"/> **(2)**
          - **Authentication method:** Use SQL authentication **(3)**
          - **Server admin login:** dp300admin **(4)**
          - **Password:** dp300P@ssword! **(5)**
          - **Confirm password:** dp300P@ssword! **(6)**

          Your **Create SQL Database Server** page should look similar to the one below. Then click **OK (7)**.

     ![Picture 7](../images/database-01.png)

 -  Back to the **Create SQL Database** page, make sure **Want to use Elastic Pool?** is set to **No**.
 -  On the **Compute + Storage** option, click on **Configure database** link. On the **Configure** page, for **Service tier** dropdown, select **Basic**, and then **Apply**.

     ![Picture 16](../images/basic.png)

      >**Note:**  Make note of this server name, and your login information. You will use it in subsequent labs.

4. For the **Backup storage redundancy** option, keep the default value: **Geo-redundant backup storage**.

5. Then click **Next: Networking**.

6. On the **Networking** tab, for **Network Connectivity** option, click the **Private endpoint** radio button.

    ![Picture 8](../images/upd-dp-300-module-02-lab-14.png)

7. Then click the **+ Add private endpoint** link under the **Private endpoints** option.

    ![Picture 9](../images/upd-dp-300-module-02-lab-15.png)

8. Complete the **Create private endpoint** right pane as follows:

- **Subscription:**  Use existing subscription
- **Resource group:**  **contoso-rg-<inject key="DeploymentID" enableCopy="false"/>**
- **Location:** <inject key="location" enableCopy="false"/>
- **Name:** DP-300-SQL-Endpoint **(1)**
- **Target sub-resource:** SqlServer **(2)**
- **Virtual network:** lab02-vnet(contoso-rg-<inject key="DeploymentID" enableCopy="false"/>) **(3)**
- **Subnet:** default (10.x.0.0/24) **(4)**
- **Integrate with private DNS zone:** Yes **(5)**
- **Private DNS zone:** keep the default value
- Review settings, and then click **OK**

9. The new endpoint will appear on the **Private endpoints** list.

    ![Picture 11](../images/upd-dp-300-module-02-lab-17.png)

10. Click **Next: Security**, and then **Next: Additional settings**.

11. On the **Additional settings** page, select **Sample** on the **Use existing data** option. Select **OK** if a pop-up message is displayed for the sample database.

    ![Picture 12](../images/upd-dp-300-module-02-lab-18.png)

12. Click **Review + Create**.

13. Review the settings before clicking **Create**.

    >**NOTE:** deployment will take time.

14. Once the deployment is complete, click **Go to resource**.

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
- Click the Lab Validation tab located at the upper right corner of the lab guide section and navigate to the Lab Validation Page.
- Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
- If not, carefully read the error message and retry the step, following the instructions in the lab guide.
- If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.
  
 
### Task 3 - Enable access to an Azure SQL Database

1. From the **SQL database** page, select the **Overview** section, and then select the link for the server name in the top section:

    ![Picture 13](../images/updt-dp-300-module-02-lab-19.png)

2. On the SQL servers navigation blade, select **Networking** under the **Security** section.

    ![Picture 14](../images/upd-dp-300-module-02-lab-20.png)

3. On the **Public access** tab, select **Selected networks (1)**, and then check the **Allow Azure services and resources to access this server (2)** property. Click **Save (3)**.

    ![Picture 15](../images/upd-dp-300-module-02-lab-21.png)
    
    >**Note:** If you receive the failed Notification as shown in the below image please perform the previous three steps again.
   
    ![Picture 15](../images/dp300-l2-failed.png)
    
    
### Task 4 - Connect to an Azure SQL Database in Azure Data Studio

1. Launch Azure Data Studio from the lab virtual machine.

   ![Picture 24](../images/123456.png)

- You may see this pop-up at initial launch of Azure Data Studio. If you receive it, click **Yes (recommended)**

    ![Picture 16](../images/upd-dp-300-module-02-lab-22.png)

2. When Azure Data Studio opens, click the **Connections (1)** button in top left corner, and then **New Connection (2)**.

    ![Picture 17](../images/lab2_1.png)

3. In the **Connection** sidebar, fill out the **Connection Details** section with connection information to connect to the SQL database created previously.

- Connection Type: **Microsoft SQL Server (1)**
- Server: Enter the name of the SQL Server created previously. For example: **dp300-lab-<inject key="DeploymentID" enableCopy="false"/>.database.windows.net (2)**
- Authentication Type: **SQL Login (3)**
- User name: **dp300admin (4)**
- Password: **dp300P@ssword! (5)**
- Expand the Database drop-down to select **AdventureWorksLT (6)**
- Server group will remain on **&lt;default&gt; (7)**
- Name (optional) can be populated with a friendly name of the database, if desired
- Review settings and click **Connect (8)**

   ![Picture 24](../images/12345.png)

  >**NOTE:** if in **Database** the option doesn't come, try to reopen **Azure Data Studio** again, and re-perform the steps from 2-3.

  >**NOTE:** You may be asked to add a firewall rule that allows your client IP access to this server. If you are asked to add a firewall rule, click on **Add account** and login to your Azure account. On **Create new firewall rule** screen, click **OK**.

    ![Picture 18](../images/upd-dp-300-module-02-lab-26.png)

 - Alternatively, you can manually create a firewall rule for your SQL server on Azure portal by navigating to your SQL server, selecting **Networking**, and then selecting **+ Add your client IPv4 address (your IP address)**

   ![Picture 18](../images/upd-dp-300-module-02-lab-47.png)

4. Azure Data Studio will connect to the database, and show some basic information about the database, plus a partial list of objects.

    ![Picture 20](../images/upd-dp-300-module-02-lab-28.png)

### Task 5 - Query an Azure SQL Database with a SQL Notebook

1. In Azure Data Studio, connected to this lab’s AdventureWorksLT database, click the **New Notebook** button.

    ![Picture 21](../images/upd-dp-300-module-02-lab-29.png)

2. Click the **+Text** link to add a new text box in the notebook

    ![Picture 22](../images/upd-dp-300-module-02-lab-30.png)

    >**Note:** Within the notebook you can embed plain text to explain queries or result sets.

3. Enter the text **Top Ten Customers by Order SubTotal**, making it Bold if desired.

    ![A screenshot of a cell phone Description automatically generated](../images/upd-dp-300-module-02-lab-31.png)

4. Click the **+ Cell** button, then **Code cell** to add a new code cell at the end of the notebook.

    ![Picture 23](../images/upd-dp-300-module-02-lab-32.png)

5. Paste the following SQL statement into the new cell:

   ```sql
   SELECT TOP 10 cust.[CustomerID],
   cust.[CompanyName],
   SUM(sohead.[SubTotal]) as OverallOrderSubTotal
   FROM [SalesLT].[Customer] cust
   INNER JOIN [SalesLT].[SalesOrderHeader] sohead
   ON sohead.[CustomerID] = cust.[CustomerID]
   GROUP BY cust.[CustomerID], cust.[CompanyName]
   ORDER BY [OverallOrderSubTotal] DESC
   ```

6. Click on the **blue circle with the arrow to execute** the query. Note how the results are included within the cell with the query.

   ![Picture 24](../images/1234.png)

7. Click the **+ Cell** button, then **Text cell** to add a new code cell at the end of the notebook.

    ![Picture 24](../images/upd-dp-300-module-02-lab-34.png)

8. Enter the text **Top Ten Ordered Product Categories**, making it Bold if desired.

9. Click the **+ Cell** button again, then **Code cell**, and paste the following SQL statement into the cell:

   ```sql
   SELECT TOP 10 cat.[Name] AS ProductCategory,
   SUM(detail.[OrderQty]) AS OrderedQuantity
   FROM salesLT.[ProductCategory] cat
   INNER JOIN [SalesLT].[Product] prod
   ON prod.[ProductCategoryID] = cat.[ProductCategoryID]
   INNER JOIN [SalesLT].[SalesOrderDetail] detail
   ON detail.[ProductID] = prod.[ProductID]
   GROUP BY cat.[name]
   ORDER BY [OrderedQuantity] DESC
   ```

10. Click on the **blue circle with the arrow to execute** the query.

11. To run all cells in the notebook and present results, click the **Run all** button in the toolbar.

    ![Picture 17](../images/upd-dp-300-module-02-lab-33.png)

12. Within Azure Data Studio save the notebook from File menu (either Save or Save As) to the **C:\LabFiles\Deploy Azure SQL Database** path (create the folder structure if it does not exist). Make sure the file extension is **.ipynb**(Notebook)

13. Close the tab for the Notebook from inside of Azure Data Studio. From the File Menu, select Open File, and open the notebook you just saved. Observe that query results were saved along with the queries in the notebook.

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
- Click the Lab Validation tab located at the upper right corner of the lab guide section and navigate to the Lab Validation Page.
- Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
- If not, carefully read the error message and retry the step, following the instructions in the lab guide.
- If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.

>**Results:** In this exercise, you've seen how you deploy a Azure SQL Database with a Virtual Network Endpoint. You were also able to connect to the SQL Database you've created using SQL Server Management Studio.

### Review

In this lab, you have completed:

- Created a Virtual Network.
- Provisioned an Azure SQL Database.
- Enabled access to an Azure SQL Database.
- Connected to an Azure SQL Database in Azure Data Studio.
- Queried an Azure SQL Database with a SQL Notebook.
  
### You have successfully completed the lab.
