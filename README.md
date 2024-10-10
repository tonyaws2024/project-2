# ASSIGNMENT 2

## Description

## Challenge

**Problem Statement**:

An organization operating in a cloud-based infrastructure, relying on Amazon Web Services
(AWS), needs an efficient solution to take daily snapshots of its Amazon Elastic Block Store
(EBS) volumes. Currently, the process of creating snapshots is manual, time-consuming, and
error-prone. The organization seeks to implement an automated system for taking daily EBS
volume snapshots to improve data backup and recovery processes and ensure business
continuity.

**Project Objectives**:

1. Automated Snapshot Creation: Develop an automated system that can create daily
snapshots of designated EBS volumes at a scheduled time.
2. Retention Policy: Implement a retention policy to manage the lifecycle of snapshots,
ensuring that older snapshots are pruned to conserve storage costs.
3. Notification and Reporting: Configure notifications to inform stakeholders of snapshot
creation and completion. Generate reports for tracking snapshot history and compliance.
4. Error Handling: Implement error handling mechanisms to address any issues that may arise
during the snapshot creation process.
5. Cost Optimization: Optimize costs by scheduling snapshots during off-peak hours,
efficiently managing snapshots' retention, and avoiding unnecessary snapshots.
6. Security: Ensure that the automated system complies with security best practices, including
access control and encryption for sensitive data.

**Expected Outcomes**:

By implementing an automated daily snapshot system for EBS volumes, the organization
expects the following outcomes:
1. Improved Data Resilience: Daily snapshots ensure that data can be quickly restored in case
of data loss or system failures.
2. Reduced Manual Effort: Automation eliminates the need for manual snapshot creation,
reducing the risk of human error and freeing up staff for other tasks.
3. Enhanced Data Retention: A well-defined retention policy ensures that older snapshots are
managed efficiently, reducing storage costs.
4. Improved Compliance: The automated system aids in adhering to data backup and
retention compliance requirements.
5. Cost Savings: Efficient management of snapshots and optimal scheduling help minimize
snapshot-related costs.

## Approach Taken

1. I launched an EC2 instance and created and attached an EBS volume.

2. I added tags to the EBS volume with the key "backup" and the value "yes". The tags were necessary because the Data Lifecycle Manager (DLM) requires the tags for the target volume.

3. I created an IAM role with SNS access, CloudTrail access, basic lambda execution role, DLM access.

4. The Data Lifecycle Manager (DLM) runs a daily schedule starting from 11:45pm. The retention policy grants for storage of 7 snapshots a week. After the seventh one the next one will automatically replace the oldest snapshot in that order.

5. When the scheduled DLM snapshot auto creation is complete, CloudTrail records the AWS API Call from DLM which initiates the EventBridge rule (EBSVolumeBackup). The EventBridge rule (EBSVolumeBackup) then  triggers a lambda function (DLMSnapshotNotifier) to send an email to an 
   already subscribed email address through a Simple Notification Service (SNS) Topic (MySnapshotNotifier)

### EC2

Amazon Elastic Compute Cloud (Amazon EC2) provides on-demand, scalable computing capacity in the Amazon Web Services (AWS) Cloud. Using Amazon EC2 reduces hardware costs so you can develop and deploy applications faster. You can use Amazon EC2 to launch as many or as few virtual servers as you need, configure security and networking, and manage storage. You can add capacity (scale up) to handle compute-heavy tasks, such as monthly or yearly processes, or spikes in website traffic. When usage decreases, you can reduce capacity (scale down) again.

![Image Alt](https://github.com/tonyaws2024/project-2/blob/7a3fab35945cbe9f8d6e6d44fc912f640abe1881/EC2%20Instance.jpg)

### Amazon EBS volumes

An Amazon EBS volume is a durable, block-level storage device that you can attach to your instances. After you attach a volume to an instance, you can use it as you would use a physical hard drive. EBS volumes are flexible. For current-generation volumes attached to current-generation instance types, you can dynamically increase size, modify the provisioned IOPS capacity, and change volume type on live production volumes.

You can use EBS volumes as primary storage for data that requires frequent updates, such as the system drive for an instance or storage for a database application. You can also use them for throughput-intensive applications that perform continuous disk scans. EBS volumes persist independently from the running life of an EC2 instance.

You can attach multiple EBS volumes to a single instance. The volume and instance must be in the same Availability Zone. Depending on the volume and instance types, you can use Multi-Attach to mount a volume to multiple instances at the same time.

![Image Alt](https://github.com/tonyaws2024/project-2/blob/1571d4f241062ae235f717cd77b8057d708addea/EBS%20Volume%20Attached%20to%20EC2.jpg)

### IAM

An IAM role is an IAM identity that you can create in your account that has specific permissions. An IAM role is similar to an IAM user, in that it is an AWS identity with permission policies that determine what the identity can and cannot do in AWS. However, instead of being uniquely associated with one person, a role is intended to be assumable by anyone who needs it. Also, a role does not have standard long-term credentials such as a password or access keys associated with it. Instead, when you assume a role, it provides you with temporary security credentials for your role session. You can use roles to delegate access to users, applications, or services that don't normally have access to your AWS resources

![Image Alt](https://github.com/tonyaws2024/project-2/blob/76fa4958750f354094fef79b85dcc72409d78085/IAM%20Role.jpg)


### Amazon Data Lifecycle Manager

You can use Amazon Data Lifecycle Manager to automate the creation, retention, and deletion of EBS snapshots and EBS-backed AMIs. When you automate snapshot and AMI management, it helps you to:

* Protect valuable data by enforcing a regular backup schedule.

* Create standardized AMIs that can be refreshed at regular intervals.

* Retain backups as required by auditors or internal compliance.

* Reduce storage costs by deleting outdated backups.

* Create disaster recovery backup policies that back up data to isolated Regions or accounts.

When combined with the monitoring features of Amazon EventBridge and AWS CloudTrail, Amazon Data Lifecycle Manager provides a complete backup solution for Amazon EC2 instances and individual EBS volumes at no additional cost.

![Image Alt](https://github.com/tonyaws2024/project-2/blob/76fa4958750f354094fef79b85dcc72409d78085/DLMPolicy.jpg)


### Amazon EBS snapshots

You can back up the data on your Amazon EBS volumes by making point-in-time copies, known as Amazon EBS snapshots. A snapshot is an incremental backup, which means that we save only the blocks on the volume that have changed since the most recent snapshot. This minimizes the time required to create the snapshot and saves on storage costs by not duplicating data.



### AWS Lambda

You can use AWS Lambda to run code without provisioning or managing servers.

Lambda runs your code on a high-availability compute infrastructure and performs all of the administration of the compute resources, including server and operating system maintenance, capacity provisioning and automatic scaling, and logging. With Lambda, all you need to do is supply your code in one of the language runtimes that Lambda supports.

You organize your code into Lambda functions. The Lambda service runs your function only when needed and scales automatically. You only pay for the compute time that you consume—there is no charge when your code is not running.

***Below is the screen shot containing the lambda function used to send notification through SNS*

![Image Alt](https://github.com/tonyaws2024/project-2/blob/76fa4958750f354094fef79b85dcc72409d78085/DLMSnapshotNotifier.jpg)









