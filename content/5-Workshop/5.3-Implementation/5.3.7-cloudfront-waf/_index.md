---
title : "Configure Amazon CloudFront Distribution & AWS WAF"
date : 2026-07-30
weight : 7
chapter : false
pre : " <b> 5.3.7 </b> "
---

Create a CDN distribution in front of API Gateway to optimize content delivery performance and enable AWS WAF firewall to protect the API from DDoS and malicious attacks.

#### General Information Setup

- Go to the **CloudFront** service $\rightarrow$ select **Create distribution**.
- In the **Distribution options** section:
  - **Distribution name**: Enter `telco-churn-cloudfront-waf`.
  - **Distribution type**: Select **Single website or app**.
- In the **Domain** section:
  - Leave the **Route 53 managed domain** field empty.

![cloudfront-get-started](/images/5-Workshop/5.3-Implementation/cdn-name.png)

#### Configure Origin & Cache Settings

- In the **Origin type** section: Select **API Gateway**.
- In the **API Gateway origin** section:
  - Select the API Gateway endpoint created in the previous step (e.g., c6kbjaktj9.execute-api.ap-southeast-1.amazonaws.com).
- In the **Origin path - optional** section: Leave it blank to point directly to the API root, or enter a specific path if needed.
- In the **Settings** section:
  - **Origin settings**: Select **Use recommended origin settings**.
  - **Cache settings**: Select **Use recommended cache settings tailored to serving API Gateway content**.

![cloudfront-origin-settings](/images/5-Workshop/5.3-Implementation/origin.png)

#### Configure AWS WAF Security (Enable security)

- Select **Enable security protections** to integrate AWS WAF for application protection.
- Under **Included security protections**:
  - Check **Rate limiting** (Recommended): Restrict the number of requests per IP address to mitigate HTTP flood / DoS attacks.
  - Set **When rate exceeds...**: `100` requests per IP address per 5-minute period.
- Click **Next**, review all configurations, and then click **Create distribution**.

![cloudfront-security-waf](/images/5-Workshop/5.3-Implementation/waf.png)