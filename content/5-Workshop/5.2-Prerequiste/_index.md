---
title : "Prerequisites"
date : 2024-01-01 
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

Before beginning the deployment workshop, ensure you have completed the following prerequisites:

---

### 1. AWS Account & IAM Configuration

To deploy the **CenFra-MS** application stack, you need an IAM user with administrative access. In this lab, we configure an IAM User Group and an IAM User with the `AdministratorAccess` policy.

#### Step 1: Create an IAM User Group
1. Navigate to the **IAM Console** -> **User groups** and click **Create group**.
2. Create a user group named `c8n`.

![IAM User Group list](/images/5-Workshop/5.2-Prerequisite/01-iam-groups.png)
*Figure 1: Creating the c8n User Group.*

#### Step 2: Attach Permissions Policy to the Group
1. In the `c8n` group settings, select the **Permissions** tab and click **Add permissions** -> **Attach policies**.
2. Search for and select the AWS-managed policy `AdministratorAccess`.

![Attach AdministratorAccess policy](/images/5-Workshop/5.2-Prerequisite/02-group-permissions.png)
*Figure 2: Attaching the AdministratorAccess policy to the group.*

The `AdministratorAccess` policy allows full access to all AWS resources:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
```

![AdministratorAccess policy JSON](/images/5-Workshop/5.2-Prerequisite/04-policy-details.png)
*Figure 3: Policy details showing full access permissions.*

#### Step 3: Create IAM User and Add to Group
1. Navigate to **Users** and click **Create user**.
2. Create a user named `c8n-dat` and enable **Console access** and **Access keys**.
3. Add the user to the `c8n` group. This user will inherit the `AdministratorAccess` permissions.

![User permissions and group membership](/images/5-Workshop/5.2-Prerequisite/03-user-permissions.png)
*Figure 4: IAM user c8n-dat inheriting AdministratorAccess via the c8n group.*

---

### 2. Network Infrastructure
* A **Virtual Private Cloud (VPC)** configured with:
  * At least two **Public Subnets** (in different Availability Zones) for the Application Load Balancer and public access.
  * A configured **Internet Gateway** attached to the VPC.

---

### 3. Custom Domain
* A registered domain name managed via **Amazon Route 53** (e.g., `tuandat.space`).

---

### 4. Container Image
* The **CenFra-MS** application code should be packaged as a Docker image and pushed to a public registry (e.g. Docker Hub `tuandat/cenframs-backend:latest`).
* A prepared `docker-compose.yml` file to pull and start the application container.