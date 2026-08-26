# Security Hub Compliance Scan Report
 
## Summary
- Total Checks: 50
- Passed: 830 ~(82%)
- Failed: 179 ~(17%)

 
## Top 5 Failed Checks
 
1. **[CIS 1.4] No root user access key exists** - CRITICAL
2. **[CIS 3.1] CloudTrail enabled in all regions** - HIGH
3. **[CIS 5.1] No security groups allow SSH from 0.0.0.0/0** - CRITICAL
4. **[CIS 4.1] CloudWatch alarm for unauthorized API calls** - MEDIUM
5. **[CIS 2.1.1] S3 buckets encrypted** - HIGH
 
## Remediation Plan
### 1. Fix CIS 1.4 (Root Access Keys - CRITICAL)
- Log in to AWS Management Console as Root.
- Go to **IAM Dashboard** → **My Security Credentials**.
- Expand **Access keys**.
- Click **Deactivate**, then **Delete** the root access keys.
- *Best Practice*: Use IAM Roles / Federated identities instead of root keys.

### 2. Fix CIS 5.1 (Restrict SSH Access - CRITICAL)
- Go to **VPC Console** → **Security Groups**.
- Identify security groups with inbound rules allowing port 22 from `0.0.0.0/0`.
- Change the source CIDR to a specific trusted IP (e.g., `YOUR_IP/32`) or remove the rule entirely.

### 3. Fix CIS 3.1 (Enable CloudTrail Multi-Region - HIGH)
- Open **CloudTrail Console** → **Trails**.
- Select the existing trail or click **Create trail**.
- Ensure **"Apply trail to all regions"** is enabled.

### 4. Fix CIS 2.1.1 (S3 Bucket Encryption - HIGH)
- Open **S3 Console**.
- Select the target bucket → **Properties** tab.
- Under **Default encryption**, edit and set Encryption type to **SSE-S3 (AES-256)** or **SSE-KMS**.

### 5. Fix CIS 4.1 (CloudWatch Metric Alarms - MEDIUM)
- Open **CloudWatch Console** → **Logs**.
- Create a Metric Filter on CloudTrail Log Group for pattern: `{ ($.errorCode = "*UnauthorizedOperation") || ($.errorCode = "AccessDenied*") }`.
- Assign a metric name and create an SNS Alarm triggered when count exceeds threshold.

---

## Remediation Verification

- **Target Item Remediated**: CIS 1.4 (Delete Root Access Key)
- **Status**: Executed via AWS Console. Access key deleted successfully.
- **Verification Command**:
  ```bash
  aws securityhub get-findings \
    --region us-east-1 \
    --filters '{"ComplianceStatus": [{"Value": "FAILED", "Comparison": "EQUALS"}]}' \
    --query 'Findings[].[Title,Resources[0].Id]' --output table