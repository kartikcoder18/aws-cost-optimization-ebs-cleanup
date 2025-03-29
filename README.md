# 📌 Optimizing AWS Costs: Automating Unused EBS Snapshot Cleanup with Lambda  

## 📖 Table of Contents  
- [💡 Introduction](#-introduction)  
- [✅ Pre-Requisites](#-pre-requisites)  
- [🚀 Project Implementation](#-project-implementation)  
- [🔐 Adding Required Permissions](#-adding-required-permissions)  
- [🛠️ Testing the Lambda Function](#-testing-the-lambda-function)  
  - [Use-Case 1: Cleaning Snapshots of Deleted Instances](#use-case-1-cleaning-snapshots-of-deleted-instances)  
  - [Use-Case 2: Cleaning Snapshots of Deleted Volumes](#use-case-2-cleaning-snapshots-of-deleted-volumes)  
- [🎯 Conclusion](#-conclusion)  

---

## 💡 Introduction  
While working with AWS, I noticed that **unnecessary EBS snapshots** were accumulating over time, leading to **increased storage costs**. To address this, I built an **AWS Lambda function** that automatically:  

- Identifies **snapshots unattached to any volume**.  
- Detects **snapshots of instances that are no longer running**.  
- Deletes the **redundant snapshots**, reducing storage costs.  

This project helped me apply my knowledge of **AWS Lambda, IAM roles, and Boto3** while improving AWS cost efficiency.  

---

## ✅ Pre-Requisites  
To build this solution, I ensured the following:  

- **Python 3.7+** was installed on my system.  
- I had a **basic understanding of Python** to write the Lambda function.  
- An **AWS account** with access to **EC2 and EBS** was available.  
- **AWS CLI** was installed and configured using `aws configure`.  
- Familiarity with **EC2 snapshots and volumes** for better resource management.  

---

## 🚀 Project Implementation  
I built a **Lambda function** that interacts with AWS **EC2 snapshots** to identify and delete **unused EBS snapshots**.  

### 1️⃣ **Creating the AWS Lambda Function**  
- I navigated to the **AWS Lambda Console** and clicked **Create Function**.  
- Selected **Author from scratch** and named the function `ebs-snapshot-cleanup`.  
- Chose **Python 3.12** as the runtime and created the function.  

📌 *Refer to the screenshot below for creating a Lambda function:*  
![Lambda Function Creation](images/lambda-function.png)  

### 2️⃣ **Writing the Lambda Function Code**  
I created a separate folder named **`lambda_script`** and placed my **Python script** inside it.  
You can find the Lambda function implementation in:  

📂 `lambda_script/script.py`  

---

## 🔐 Adding Required Permissions  
To allow my Lambda function to interact with **EC2 snapshots**, I assigned the necessary permissions:  

### **1️⃣ Attaching an IAM Role to Lambda**  
- Opened the **AWS Lambda function**.  
- Went to *Configuration → Permissions* and clicked on the **service role** linked to Lambda.  

📌 *Refer to the screenshot below for IAM role settings:*  
![IAM Role Settings](images/iam-role.png)  

### **2️⃣ Creating an Inline IAM Policy**  
- Opened the **IAM Console** → Clicked **Add Permissions** → **Create Inline Policy**.  
- Granted the following permissions:  
  - `DescribeSnapshots`  
  - `DescribeInstances`  
  - `DescribeVolumes`  
  - `DeleteSnapshot`  

- Attached this policy to the Lambda function’s role.  

---

## 🛠️ Testing the Lambda Function  
I tested the function using two scenarios to ensure proper snapshot cleanup.  

### 🏷 **Use-Case 1: Cleaning Snapshots of Deleted Instances**  
✔️ **Created an EC2 Instance** (`test-ebs`).  

📌 *EC2 instance creation screenshot:*  
![EC2 Instance Creation](images/ec2-instance.png)  

✔️ **Took a snapshot** of the instance.  

📌 *EBS Snapshot Creation screenshot:*  
![EBS Snapshot](images/ebs-snapshot.png)  

✔️ **Terminated the EC2 instance**, leaving an orphaned snapshot.  
✔️ **Ran the Lambda function** → It detected and **deleted the unused snapshot**.  

### 🏷 **Use-Case 2: Cleaning Snapshots of Deleted Volumes**  
✔️ **Created a new EBS volume** (`test-volume`).  
✔️ **Created a snapshot** from this volume.  
✔️ **Deleted the volume**, leaving a detached snapshot.  
✔️ **Ran the Lambda function** → It identified and **deleted the unassociated snapshot**.  

📌 *Refer to the screenshot below for testing Lambda execution:*  
![Lambda Execution](images/lambda-execution.png)  

---

## 🎯 Conclusion  
This project successfully:  
✔️ **Automates AWS EBS snapshot cleanup**, reducing storage costs.  
✔️ **Improves AWS resource efficiency** by removing unnecessary snapshots.  
✔️ Strengthens my skills in **AWS Lambda, Python (Boto3), IAM policies, and EC2 snapshot management**.  

🔗 **References:**  
- [AWS Lambda Documentation](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)  
- [Boto3 Documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)  

✅ **Project Successfully Implemented! 🚀**


