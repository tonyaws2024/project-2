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

![Image Alt]()
