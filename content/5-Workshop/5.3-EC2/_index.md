---
title : "EC2"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

#### Overview

In this section, an Amazon EC2 instance is prepared as the backend server for the application. The instance hosts the containerized **CenFra-MS** service and later becomes the backend target for the Application Load Balancer.

The main goals of this section are:

+ Review the EC2 security group.
+ Check the EC2 instance configuration.
+ Start the EC2 instance.
+ Connect to the instance.
+ Install Docker and prepare the application runtime.

#### EC2 security group

The EC2 security group controls which traffic is allowed to reach the instance. In this setup, the inbound rules include **HTTP**, **SSH**, **RDP**, and **Custom TCP 8080**. Port 8080 is used by the application container.

![EC2 security group](/images/5-Workshop/5.3-EC2/ec2-security-group.png)

#### EC2 instance summary

The backend instance is named **CenFra-MS**. It uses the **t3.micro** instance type and is deployed in the selected VPC and subnet. The instance summary also shows the public IPv4 address, private IPv4 address, public DNS name, and private DNS name.

![EC2 instance summary](/images/5-Workshop/5.3-EC2/ec2-summary.png)

#### Start EC2 instance

After starting the instance, the EC2 console shows **CenFra-MS** in the **Running** state. This confirms that the server is ready for application deployment.

![EC2 running state](/images/5-Workshop/5.3-EC2/ec2-running.png)

#### Connect and install Docker

The instance is accessed through SSH. The terminal shows that the server is running **Amazon Linux 2023**. Docker is installed to run the application in a containerized environment.

![Install Docker](/images/5-Workshop/5.3-EC2/docker-install.png)

#### Prepare Docker Compose

A working directory is created for the application and a `docker-compose.yml` file is prepared. Docker Compose defines the application service, image, environment file, restart policy, and port mapping for the application.

![Prepare Docker Compose](/images/5-Workshop/5.3-EC2/docker-compose-file.png)


#### IAM role support

An IAM role can be created for EC2 by selecting **AWS service** as the trusted entity type and **EC2** as the service use case. This allows EC2 instances to call AWS services without storing long-term access keys on the server.

![IAM EC2 role](/images/5-Workshop/5.3-EC2/iam-ec2-role.png)

#### Test & Validation

After starting the container via Docker Compose, verify that the application is healthy by running a curl command or opening the browser:

```bash
curl -i http://localhost:8080/actuator/health
```

Expected response (`HTTP/1.1 200 OK`):
```json
{
  "status": "UP"
}
```

#### EC2 summary

At the end of this section, the EC2 instance is running and prepared for the application workload. The application runs on port **8080**, which is the port used later when registering the instance into the load balancer target group.
