---
title : "Route 53 & CloudFront CDN Integration"
date : 2024-01-01 
weight : 6 
chapter : false
pre : " <b> 5.6. </b> "
---

#### Step 1: Route 53 Hosted Zone Configuration

1. In the **Route 53 Console**, select **Hosted zones** and click **Create hosted zone**.
2. Enter your custom domain name (e.g., `tuandat.space`), set Type to **Public hosted zone**, and click **Create**.
3. Update your domain registrar's Name Server (NS) records with the AWS Name Servers listed in Route 53.

![Route 53 Hosted Zone](/images/5-Workshop/route53/Screenshot%202026-07-28%20113639.png)
*Creating Route 53 Hosted Zone.*

![Enter Hosted Zone name](/images/5-Workshop/route53/Screenshot%202026-07-28%20114628.png)
*Enter `tuandat.space`, choose **Public hosted zone**, and create the Hosted Zone.*

![Configure Nameservers on Namecheap](/images/5-Workshop/route53/namecheap-route53-nameservers.png)
*In Namecheap, select **Custom DNS** and replace the nameservers with the `awsdns-*` nameservers supplied by Route 53. This delegates DNS to Route 53; it is not the CloudFront CNAME record.*

![Check NS and SOA records](/images/5-Workshop/route53/Screenshot%202026-07-28%20114519.png)
*After creating the Hosted Zone, verify the default **NS** and **SOA** records. The NS values are the nameservers used for the domain.*

![Route 53 DNS record editor](/images/5-Workshop/route53/Screenshot%202026-07-28%20114945.png)
*Use the Route 53 record editor to choose a record type and enter DNS values when adding a record.*

---

#### Step 2: Requesting ACM Certificate for HTTPS

1. Navigate to **AWS Certificate Manager (ACM)** and click **Request certificate**.
2. Choose **Request a public certificate** and click **Next**.
3. Enter your domain names (e.g., `cenframs.tuandat.space` and `*.tuandat.space`).
4. Select **DNS validation** as validation method and click **Request**.
5. Once requested, click **Create records in Route 53** under the domain details to validate ownership automatically.

---

#### Step 3: CloudFront Distribution Setup

1. Open the **CloudFront Console** and click **Create distribution**.
2. Under **Origin domain**, select your Application Load Balancer DNS name.
3. Under **Default cache behavior**:
   * Set **Viewer protocol policy** to **Redirect HTTP to HTTPS**.
   * Set **Allowed HTTP methods** to `GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE` (for API support).
   * Under **Cache key and query requests**, select **Cache policy: CachingDisabled** to ensure API requests are dynamically routed to the backend rather than cached.
4. Under **Settings**:
   * Add your custom domain to **Alternate domain name (CNAME)** (e.g., `cenframs.tuandat.space`).
   * In **Custom SSL certificate**, choose the ACM SSL certificate requested in the previous step.
5. Click **Create distribution** and wait for the status to turn to **Enabled**.

#### Step 3.1: Start the distribution

Choose **Single website or app** and give the distribution a clear name such as `c8n-aws-clf`.

![Create CloudFront distribution](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20115634.png)
*Create the distribution and set its name.*

#### Step 3.2: Select the Application Load Balancer origin

Choose **Elastic Load Balancer** and select the ALB DNS name that forwards traffic to the backend Target Group. Do not select S3 because CenFra-MS is a dynamic API behind an ALB.

![Choose Elastic Load Balancer origin type](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20115958.png)
*Select ELB as the origin type.*

![Enter the ALB origin](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20120815.png)
*Select the ALB and enter its `...elb.amazonaws.com` DNS name.*

#### Step 3.3: Configure the CloudFront-to-ALB connection

For an ALB listening on HTTP port `80`, choose **Customize origin settings**, **HTTP only**, and port `80`. Viewer HTTPS is terminated at CloudFront; the workshop's CloudFront-to-ALB connection follows the existing HTTP ALB configuration.

![Configure origin settings](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20120220.png)
*Use HTTP only and port 80 for the origin connection.*

![Review origin connection settings](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20120820.png)
*Keep Origin Shield disabled and review connection and timeout settings.*

#### Step 3.4: Configure HTTPS and API methods

Choose **Customize cache settings**, then set:

* **Viewer protocol policy**: `Redirect HTTP to HTTPS`.
* **Allowed HTTP methods**: `GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE`.
* **Cache policy**: `CachingDisabled` because the API uses JWT, dynamic data, and user-specific responses.
* **Origin request policy**: `AllViewerExceptHostHeader` to forward the required viewer parameters while excluding the incompatible Host header.

![Viewer protocol and HTTP methods](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20120433.png)
*Redirect HTTP to HTTPS and allow all REST API methods.*

![Cache and origin request policies](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20120832.png)
*Disable API caching and select `AllViewerExceptHostHeader`.*

![Confirm cache behavior](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20120837.png)
*Verify the cache policy, origin request policy, and API methods.*

![Completed cache settings](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20120826.png)
*The custom cache settings and HTTPS redirect are selected.*

#### Step 3.5: Review and create

Confirm the ALB origin, `CachingDisabled`, HTTPS redirect, and all required API methods, then choose **Create distribution**.

![Review distribution configuration](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20121314.png)
*Review the origin, HTTPS, API methods, and cache settings before creation.*

#### Step 3.6: Add the custom domain and ACM certificate

If the distribution was created before the domain was added, open it and choose **Add domain**. Enter `cenframs.tuandat.space`.

![Enter custom domain](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20124937.png)
*Enter the backend hostname served by CloudFront.*

![CloudFront requests a TLS certificate](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20124945.png)
*CloudFront requires an ACM certificate in `us-east-1` for a custom domain.*

![Select the ACM certificate](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20125101.png)
*Select the certificate covering `*.tuandat.space` after DNS validation completes.*

![Review custom domain and certificate](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20125109.png)
*Review the domain and certificate before choosing **Add domains**.*

![CloudFront update succeeds](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20125414.png)
*The distribution update succeeds and CloudFront displays the DNS records to point to it.*

---

#### Step 4: Point Custom Domain to CloudFront via Route 53

The CDN is used through Route 53: Namecheap remains the domain registrar and nameserver delegation point, Route 53 manages DNS records, and CloudFront receives the CDN traffic.

1. Go back to your **Route 53 Hosted Zone**.
2. Click **Create record**. Set record name to `cenframs`, choose **A record**, and add an **AAAA record** if IPv6 support is required.
3. Toggle the **Alias** switch on.
4. Under **Route traffic to**, choose **Alias to CloudFront distribution**, select your CloudFront distribution, and click **Create records**. Do not point this record directly to the ALB.
5. You can now access your Spring Boot application securely at `https://cenframs.tuandat.space/actuator/health`.

---

#### Step 5: Verify the Application & Verify S3 Media Storage

To verify that the entire architecture is working correctly (Route 53 -> CloudFront CDN -> ALB -> EC2 Backend -> RDS PostgreSQL & Amazon S3), access the frontend application and perform typical operations.

##### 5.1. Access the Application Login Page
Go to `https://cenfra-ms.tuandat.space/login`. You will see the centralized login screen for **Pizza Five Guys Central Kitchen Management**.

![Centralized Login Page](/images/5-Workshop/5.5-Route53-CloudFront/01-login-page.png)
*Figure 5: Pizza Five Guys Central Kitchen Management login portal.*

##### 5.2. Management Dashboard
Enter your credentials and log in. The application will redirect you to the management dashboard where you can see product status, order summaries, and real-time inventory counts retrieved from the RDS PostgreSQL database.

![Management Dashboard](/images/5-Workshop/5.5-Route53-CloudFront/02-dashboard.png)
*Figure 6: Central Kitchen System overview dashboard.*

##### 5.3. Add a New Product and Upload Media to S3
1. Navigate to the **Product Management** screen (`/manager/products`).
2. Click **Add new product**. Set name to `Sprite lon`, category to `Prepared Food`, unit to `pack`, and price to `20000`.
3. Choose an image to upload and click **Add product**.

![Add Product Dialog](/images/5-Workshop/5.5-Route53-CloudFront/03-add-product.png)
*Figure 7: Product creation form with image upload.*

##### 5.4. Verify Product Created Successfully
The product is listed in the dashboard and product catalog, confirming successful writes to the database.

![Product List](/images/5-Workshop/5.5-Route53-CloudFront/04-product-list.png)
*Figure 8: Product management catalog showing the newly added product.*

##### 5.5. Verify Media Files Hosted on S3
Right-click on the product image and open it in a new tab. You will see that the product image is directly hosted in your Amazon S3 bucket (`https://aws-c8n-s3.s3.us-west-2.amazonaws.com/products/...`). This confirms that the backend application successfully uploads and serves product images using AWS S3.

![S3 Image URL](/images/5-Workshop/5.5-Route53-CloudFront/05-s3-image.png)
*Figure 9: Product image successfully uploaded and served from Amazon S3.*
