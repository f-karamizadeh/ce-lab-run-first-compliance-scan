# ce-lab-run-first-compliance-scan
Author: Faramarz Karamizadeh

# AWS Security Hub & CIS Benchmark Compliance Scan

![AWS Security Hub](https://img.shields.io/badge/AWS-Security%20Hub-orange?style=flat-square&logo=amazonaws)
![CIS Benchmark](https://img.shields.io/badge/Compliance-CIS%20v1.4.0-blue?style=flat-square)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=flat-square)

## 📌 Project Overview
This repository contains documentation, scan findings, and remediation steps for **Lab M8.03: Run First Compliance Scan with Security Hub**.

The primary objective of this lab is to enable AWS Security Hub, execute an automated compliance scan using the **CIS AWS Foundations Benchmark v1.4.0**, analyze the findings, and remediate critical security vulnerabilities within the AWS environment.

---

## 🎯 Learning Objectives
- Enable **AWS Security Hub** and subscribe to security standards.
- Run continuous compliance scanning using **CIS AWS Foundations Benchmark**.
- Extract, analyze, and filter failed compliance checks via AWS CLI.
- Formulate a prioritized remediation strategy based on severity levels.
- Perform hands-on remediation for critical findings.

---

## 🛠️ Prerequisites & Tools
- **AWS CLI** configured with administrator credentials.
- **AWS Security Hub** enabled in region `us-east-1`.
- **Bash / Command Line Environment**.

---

## 🚀 Execution Steps

### 1. Enable Security Hub & CIS Benchmark
Security Hub was enabled in the target region along with the CIS AWS Foundations Benchmark standard:

```bash
# Enable Security Hub
aws securityhub enable-security-hub --region us-east-1

# Subscribe to CIS AWS Foundations Benchmark v1.4.0
aws securityhub batch-enable-standards \
  --region us-east-1 \
  --standards-subscription-requests StandardsArn="arn:aws:securityhub:us-east-1::standards/cis-aws-foundations-benchmark/v/1.4.0"
  
## Summary
- Total Checks: 50
- Passed: 830 ~(82%)
- Failed: 179 ~(17%)