---
title : "Configure Monitoring & Notification (CloudWatch & SNS)"
date : 2026-07-27 
weight : 8
chapter : false
pre : " <b> 5.3.8 </b> "
---
#### Create SNS Topic
- Go to Amazon SNS $\rightarrow$ Topics $\rightarrow$ Click Create topic named TelcoChurnAlerts.
- Click Create subscription $\rightarrow$ Protocol: Email $\rightarrow$ Enter your Gmail address $\rightarrow$ Confirm email in your inbox.
![sns](/images/5-Workshop/5.3-Implementation/sns.png)
#### Create Alarm
- In the **CloudWatch Console** interface, navigate to **Alarms** and select **Create alarm**.
- Click the orange **Select metric** button under the **Metric** section.
![metric](/images/5-Workshop/5.3-Implementation/metric.png)
- In the **Select metric** window that appears, on the **Browse** tab, select **Lambda** $\rightarrow$ **By Function Name** $\rightarrow$ **telco-churn-api-handler** $\rightarrow$ **Select metric**.
![metric-name](/images/5-Workshop/5.3-Implementation/metric-name.png)
- After selecting the metric, the interface will return to the configuration page. Scroll down to the **Conditions** section:
   + **Threshold type**: Select **Static**
   + **Whenever Errors is...**: Select **Greater/Equal** (>= threshold)
   + **than...**: Enter threshold value `1`
 ![cond](/images/5-Workshop/5.3-Implementation/cond.png)
- Under **Notification**:
   + **Alarm state trigger**: Select **In alarm** (Execute action when metric exceeds alarm threshold).
   + **Send a notification to the following SNS topic**: Select **Select an existing SNS topic**.
   + **Send a notification to...**: Select pre-created SNS Topic from dropdown menu (e.g. Telco-Churn-Alarm-Topic).
   + Check Email information receiving notifications displayed in **Email (endpoints)** below (e.g.trungnam2682005@gmail.com).
   + Scroll to bottom of page and click **Next**.
    ![notice](/images/5-Workshop/5.3-Implementation/notice.png)
- Under **Name and description**:
   + **Alarm name**: Enter alarm name, e.g.: `Telco-Churn-API-Error-Alarm`.
   + **Alarm description - optional**: Enter alarm description if necessary.
   + Click **Next** at bottom of page.
    ![alarm-name](/images/5-Workshop/5.3-Implementation/alarm-name.png)