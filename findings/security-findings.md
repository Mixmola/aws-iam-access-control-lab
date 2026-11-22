# AWS IAM Access Control Lab — Security Findings

## 1. Overview

This document records the security observations and findings from the AWS IAM Access Control Lab.

The objective was to assess whether IAM roles had appropriate permissions, whether access was restricted to the intended S3 bucket, and whether trust relationships followed least-privilege principles.

## 2. Findings

### Finding 1: Least-Privilege Access Control

**Observation:** Three IAM roles were created for different responsibilities:

- `S3-ReadOnly-Role`
- `S3-Upload-Role`
- `S3-Application-Role`

Each role was assigned permissions according to its intended purpose.

**Security significance:** Separating access levels reduces unnecessary permissions and limits the potential impact of compromised credentials or identities.

**Recommendation:** Continue reviewing IAM roles to ensure their permissions match their responsibilities.

### Finding 2: Unnecessary Delete Permissions

**Observation:** None of the three roles was granted `s3:DeleteObject`.

The upload role could upload objects without having permission to delete existing objects.

**Security significance:** This reduces the risk of accidental or unauthorised deletion of stored data.

**Recommendation:** Avoid granting deletion permissions unless they are explicitly required.

### Finding 3: Resource-Level Restrictions

**Observation:** Permissions were restricted to the intended S3 bucket using specific bucket and object ARNs.

Policy Simulator tests against a different bucket returned denied results for all three roles.

**Security significance:** Restricting resource access prevents roles from receiving unnecessary access to other S3 buckets.

**Recommendation:** Use specific resource ARNs instead of wildcard resources wherever possible.

### Finding 4: IAM Trust Relationships

**Observation:** The read-only and upload roles trusted the AWS account, while the application role trusted the EC2 service.

**Security significance:** Trust policies determine which principals or services can assume IAM roles. Broad trust relationships can increase the number of identities potentially authorised to assume a role.

**Recommendation:** Restrict trusted principals to the identities or services that genuinely require access.

### Finding 5: Implicit Deny and Explicit Deny

**Observation:** The Policy Simulator returned implicit deny for actions that were not granted by the attached policies, including `s3:DeleteObject` for the upload role.

**Security significance:** AWS denies actions by default when no applicable policy grants permission. An explicit Deny statement also blocks an action and overrides an applicable Allow.

**Recommendation:** Understand both implicit and explicit deny when reviewing IAM permissions.

## 3. Permissions Comparison

| Role | List | Read | Upload | Delete |
|---|---|---|---|---|
| S3-ReadOnly-Role | Allow | Allow | Denied | Denied |
| S3-Upload-Role | Allow | Allow | Allow | Denied |
| S3-Application-Role | Allow | Allow | Denied | Denied |

## 4. Excessive Permissions Review

An excessive-permissions review was not performed as part of this lab. However, regularly reviewing IAM permissions is an important security practice.

The recommended approach is to:

1. Investigate whether a permission has been used and when it was last used.
2. Determine whether the permission is necessary for the role's responsibilities.
3. Remove or modify unnecessary permissions after assessing their potential impact.

**Note:** These steps are recommended practices and were not carried out as part of this lab.

AWS CloudTrail can help investigate API activity, subject to the available event history and logging configuration.

## 5. Testing Limitations

- Permissions were evaluated using the AWS IAM Policy Simulator.
- Cross-bucket access restrictions were tested using alternative resource ARNs.
- No live S3 API access tests were performed.
- No EC2 instance was launched to verify actual role assumption or access to S3.
- Explicit Deny was discussed as a hypothetical policy change but was not added to any of the lab policies.

## 6. Conclusion

The lab demonstrated practical implementation of AWS IAM roles and least-privilege permissions.

It also reinforced the importance of resource-level restrictions, trust relationships, and understanding how AWS evaluates Allow and Deny permissions.

The results demonstrate how appropriately scoped IAM policies can reduce unnecessary access and improve the security of cloud resources.
