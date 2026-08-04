---
title: "Week 3 Worklog: Amazon EBS Volumes & Custom AMIs"
date: 2024-01-15
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* Understand Amazon EBS (Elastic Block Store) volume types and storage features.
* Practice attaching, formatting, and mounting additional EBS volumes onto an EC2 Linux instance.
* Practice creating EBS Snapshots to back up persistent data volumes.
* Create a Custom AMI (Amazon Machine Image) for rapid server replication.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Study Amazon EBS volume types: `gp3`, `io2`, `st1`, `sc1` <br> - Understand the distinction between Root Volumes and Additional EBS Volumes | 08/25/2025 | 08/25/2025 | [EBS Volume Types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html) |
| Tue | - Create a 10GB `gp3` EBS volume in the same Availability Zone as the EC2 instance <br> - Attach volume to EC2 and use Linux commands (`lsblk`, `mkfs.ext4`, `mount`) to extend disk space | 08/26/2025 | 08/26/2025 | [Attach & Format EBS Volume](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html) |
| Wed | - Populate test data onto the mounted EBS volume <br> - Create an EBS Snapshot for data backup <br> - Restore a new EBS volume from the created Snapshot | 08/27/2025 | 08/27/2025 | [EBS Snapshots](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html) |
| Thu | - Configure a fully functional web server on EC2 (JDK, Node.js, Web App) <br> - Bake a Custom AMI `My-Custom-WebServer-AMI` from this instance | 08/28/2025 | 08/28/2025 | [Create an EBS-backed AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/creating-an-ami-ebs.html) |
| Fri | - Launch a new EC2 instance from the Custom AMI <br> - Verify the new instance comes pre-configured with all packages without requiring manual setup | 08/29/2025 | 08/29/2025 | [Launch Instance from AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance-from-ami.html) |

### Week 3 Achievements:

* Understood Amazon EBS block storage provisioning and scaling operations.
* Mastered mounting and managing storage volumes in cloud Linux servers.
* Learned snapshot-based backup and volume restoration procedures.
* Successfully baked Custom AMIs to enable rapid instance cloning.
