# Enforcing Least Privilege with CheckNoNewAccess

## Overview
This repository demonstrates how to enforce the principle of least privilege using AWS Access Analyzer's `CheckNoNewAccess`. It includes sample IAM policies, CLI scripts, and outputs to validate policy changes before deployment.

![CheckNoNewAccess Diagram](images/checknoaccess.png)

## Prerequisites
- AWS CLI configured with necessary permissions
- IAM Access Analyzer enabled
- A working AWS account

## IAM Policies
### **Existing Policy (`existing-policy.json`)**
This policy grants full access to resources except for a sensitive S3 bucket.

### **New Policy (`new-policy.json`)**
This policy grants limited access to another S3 bucket.

### **Updated-New Policy (`news-policy.json`)**
This updated policy tries to modify permissions for a sensitive bucket, adding new access.

## CLI Command to Check Policy Changes
```bash
#!/bin/bash
aws accessanalyzer check-no-new-access \
--existing-policy-document file://../policies/existing-policy.json \
--new-policy-document file://../policies/new-policy.json \
--policy-type IDENTITY_POLICY

## Expected Output
1. **No Additional Access Granted (PASS)**  
   When the policy does not grant new access, Access Analyzer will output "PASS."

2. **New Access Granted (FAIL)**  
   If the new policy grants additional permissions, Access Analyzer detects the changes and outputs "FAIL."  
   In this case, deployment would be halted to prevent any unwanted privileges in production accounts.


## Risk Considerations and Control Analysis
### **Risks:**
- Granting excessive privileges may expose sensitive AWS resources.
- Unauthorized data access could lead to data leaks.
- Overly permissive IAM policies increase the attack surface.

### **Controls to Mitigate Risks:**
- Implement `CheckNoNewAccess` to prevent unauthorized privilege escalation.
- Integrate the check into CI/CD pipelines for automatic validation.
- Perform regular IAM policy reviews and audits.
- Use IAM roles with least privilege access.

## Conclusion
By leveraging AWS Access Analyzer’s `CheckNoNewAccess`, organizations can proactively enforce least privilege and prevent unintended privilege escalations in IAM policies.
