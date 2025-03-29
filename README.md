# aws-cost-optimization-ebs-cleanup
# 🚀 AWS Cost Optimization: Automated EBS Snapshot Cleanup with Lambda

## 📌 Introduction
Managing Amazon EBS snapshots efficiently is crucial for optimizing AWS costs. This project automates the cleanup of **unused EBS snapshots** using **AWS Lambda** and **Python (boto3)**. It ensures that snapshots of deleted EC2 instances or volumes are removed, preventing unnecessary storage costs.

## 🎯 Project Objectives
✅ Identify and delete **snapshots of terminated EC2 instances**  
✅ Remove **snapshots of deleted EBS volumes**  
✅ Implement **AWS IAM best practices** for secure execution  
✅ **Automate the cleanup** using AWS Lambda  

## 🛠 Prerequisites
Before proceeding, ensure you have:
- **Python 3.7+** installed
- **AWS CLI** configured (`aws configure`)
- **IAM Role for Lambda** with permissions:
  - `DescribeSnapshots`
  - `DescribeInstances`
  - `DescribeVolumes`
  - `DeleteSnapshot`

## 🔧 Setting Up the Lambda Function
### 1️⃣ Create a Lambda Function
1. Navigate to **AWS Lambda Console**
2. Click **Create Function** → **Author from scratch**
3. Name it **ebs-snapshot-cleanup** and select **Python 3.12**
4. Assign an **IAM role** with required permissions

### 2️⃣ Add the Python Code
```python
import boto3

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')
    response = ec2.describe_snapshots(OwnerIds=['self'])
    
    instances_response = ec2.describe_instances(Filters=[{'Name': 'instance-state-name', 'Values': ['running']}])
    active_instance_ids = {instance['InstanceId'] for reservation in instances_response['Reservations'] for instance in reservation['Instances']}
    
    for snapshot in response['Snapshots']:
        snapshot_id = snapshot['SnapshotId']
        volume_id = snapshot.get('VolumeId')
        
        if not volume_id:
            ec2.delete_snapshot(SnapshotId=snapshot_id)
            print(f"Deleted EBS snapshot {snapshot_id} (No Volume Attached)")
        else:
            try:
                volume_response = ec2.describe_volumes(VolumeIds=[volume_id])
                if not volume_response['Volumes'][0]['Attachments']:
                    ec2.delete_snapshot(SnapshotId=snapshot_id)
                    print(f"Deleted snapshot {snapshot_id} (Orphaned Volume)")
            except ec2.exceptions.ClientError as e:
                if e.response['Error']['Code'] == 'InvalidVolume.NotFound':
                    ec2.delete_snapshot(SnapshotId=snapshot_id)
                    print(f"Deleted snapshot {snapshot_id} (Volume Not Found)")
```

### 3️⃣ Deploy and Test
- **Increase timeout**: Go to **Configuration** → **General Configuration** → Set timeout to **10s**.
- **Run a test event** to check if snapshots are being deleted.
- **Use CloudWatch Logs** to verify function execution.

## 📊 Architecture Diagram
![AWS Architecture](images/architecture-diagram.png)

## 🎯 Real-World Scenarios
**✅ Scenario 1: Cleaning Snapshots of Deleted Instances**
1. Create an EC2 instance and take a snapshot.
2. Terminate the instance.
3. Run the Lambda function → Snapshot gets deleted.

**✅ Scenario 2: Cleaning Snapshots of Deleted Volumes**
1. Create an EBS volume and take a snapshot.
2. Delete the volume.
3. Run the Lambda function → Snapshot gets deleted.

## 📄 Full Documentation
📜 **Detailed Guide**: [docs/ebs-snapshot-cleanup.md](docs/ebs-snapshot-cleanup.md)  
📸 **Screenshots & Diagrams**: Available in `/images` folder.  
