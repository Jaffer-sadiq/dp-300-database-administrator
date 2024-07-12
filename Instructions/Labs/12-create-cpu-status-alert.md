# Lab 12: Create a CPU status alert for a SQL Server on Azure

## Lab scenario
You have been hired as a Senior Data Engineer to help automate day to day operations of database administration. This automation is to help ensure that the databases for AdventureWorks continue to operate at peak performance as well as provide methods for alerting based on certain criteria.

## Lab objectives

In this lab, you will complete the following task:

- Task 1: Create an alert when a CPU exceeds an average of 80 percent

## Estimated timing: 30 minutes

## Architecture diagram

![](../images/preview(12).png)

### Task 1 - Create an alert when a CPU exceeds an average of 80 percent

1. In the search bar at the top of the Azure portal, type **SQL database (1)** and select **SQL databases (2)**. 

    ![Screenshot of selecting a SQL database](../images/sql.png)
 
3. Select the **AdventureWorksLT** database name listed.

   ![Screenshot of selecting a SQL database](../images/dp300-lab12-img1.png)

2. On the main blade for the **AdventureWorksLT** database, navigate down to the monitoring section. Select **Alerts (1)**.Select **Create alert rule (2)**.

   ![Screenshot of selecting Alerts on the SQL database Overview page](../images/dp300-lab12-img02.png)

3. In the **Select a signal**, select **CPU percentage** from the drop-down.

   ![Screenshot of selecting CPU percentage](../images/dp300-lab12-img4.png)

4. In the **Create an alert rule** slide out, On the **Alert logic** section, select **Static (1)** for the **Threshold** property. Then check that the **Aggregation** type is **Average (2)**, the **Operator** property is **Greater than (3)** Then in **Threshold value** enter a value of **80 (4)**. Select **Next:Actions (5)**.

    ![Screenshot of entering 80 and selecting Done](../images/lab12-dp-300-001.png)

5. In the **Actions (1)** tab, select **Create action group (2)**.

    ![Screenshot of selecting the Create action group](../images/dp300-lab12-img7.png)

6. On the **Create action group** screen, type **emailgroup (1)** in the **Action group name** field, type **emailgroup** in the **Display name** and then select **Next: Notifications (2)**.

    ![Screenshot of entering emailgroup and selecting Next: Notifications](../images/dp-300-lab12-02.png)

7. On the **Notifications (1)** tab, enter the following information:

    - **Notification type:** Email/SMS message/Push/Voice **(2)**
       > **Note:** When you select this option, a Email/SMS message/Push/Voice flyout will appear. Check the Email **(3)** property and type the proper email address **(4)**, you can also use your personal email so that you can receive the notifications as shown in the last steps, Check the SMS **(5)** enter your country code **(6)** and your phone number **(7)**.
     - Click on **OK (8)**
    - **Name:** DemoLab (9)
    - Select **Review + create**, then select **Create**.
        >**Note:** Wait till action group created successfully then move to next step.

      ![Screenshot of the Create action group page with information added](../images/dp300-lab12-img9.png)
    
8. On the **Create an alert rule** screen, select **Details (1)** tab, enter Alert rule name as **emailrule (2)**, then select **Review + create**.
    
    ![Screenshot of entering emailgroup and selecting Next: Notifications](../images/lab12-dp-300-0015-step.png)

9. On the **Review + create (1)** tab, Select **Create (2)**.

    ![Screenshot of the Create alert rule page selecting the Create alert rule](../images/lab12-dp-300-02.png)
    
10. An email like this is sent to the email address that you entered, once the rule is created.

    ![Screenshot of the confirmation email](../images/dp300-lab12-img11.png)

    With the alert in place, if the CPU usage on average exceeds 80%, an email like this is sent.

    ![Screenshot of the warning email](../images/dp300-lab12-img13.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
- Click the Lab Validation tab located at the upper right corner of the lab guide section and navigate to the Lab Validation Page.
- Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
- If not, carefully read the error message and retry the step, following the instructions in the lab guide.
- If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.

>**Results:** In this exercise you've learned how Alerts can send you an email or call a web hook when some metric (for example database size or CPU usage) reaches a threshold you define. You've just seen how you can easily configure alerts for Azure SQL Databases.

### Review

In this lab, you have completed:

- Created an alert when a CPU exceeds an average of 80 percent

### You have successfully completed the lab.
