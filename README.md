# AWS IAM Access Control Lab

## Project Overview

This project demonstrates how to implement and evaluate role-based access control in Amazon Web Services (AWS) using IAM policies and IAM roles.

The objective was to apply the principle of least privilege by creating separate roles for different responsibilities and restricting their access to a specific Amazon S3 bucket.

The AWS IAM Policy Simulator was used to evaluate the permissions assigned to each role.

## Project Objectives

- Understand the difference between IAM permissions policies and trust policies.
- Implement least-privilege access using IAM roles.
- Create different access levels for different responsibilities.
- Restrict permissions to a specific S3 bucket and its objects.
- Test allowed and denied actions using the IAM Policy Simulator.
- Understand implicit deny and explicit deny.
- Review trust relationships and recommended practices for identifying excessive permissions.

## AWS Resources Used

- AWS Identity and Access Management (IAM)
- Amazon Simple Storage Service (S3)
- AWS IAM Policy Simulator
- IAM customer-managed policies
- IAM roles

## Role Design

| IAM Role | Permissions |
|---|---|
| `S3-ReadOnly-Role` | List bucket contents and read objects |
| `S3-Upload-Role` | List bucket contents, read objects and upload objects |
| `S3-Application-Role` | List bucket contents and read objects; trusted by EC2 |

The application role was configured with EC2 as its trusted service. No EC2 instance was launched during this lab.

## Policy Files

The `policies/` directory contains the JSON policy documents:

- `readonly-s3-policy.json`
- `upload-s3-policy.json`

The application role reused the read-only policy.

## Security Testing

The IAM Policy Simulator was used to evaluate the following permissions:

| Action | Read-only role | Upload role | Application role |
|---|---|---|---|
| `s3:ListBucket` | Allowed | Allowed | Allowed |
| `s3:GetObject` | Allowed | Allowed | Allowed |
| `s3:PutObject` | Denied | Allowed | Denied |
| `s3:DeleteObject` | Denied | Denied | Denied |

Alternative resource ARNs were also used to evaluate whether access was restricted to the intended bucket.

## Security Concepts Demonstrated

- **Least privilege:** Each role received permissions aligned with its intended responsibility.
- **Resource-level restrictions:** S3 permissions were scoped to a specific bucket and its objects.
- **Trust policies:** The lab demonstrated how trust policies determine which principals or services can assume a role.
- **Implicit deny:** Actions without an applicable allow permission were denied.
- **Explicit deny:** The effect of an explicit Deny statement was discussed but not added to the policies.

## Testing Limitations

All permission results were evaluated using the IAM Policy Simulator. No live S3 API access test was performed, and no EC2 instance was launched to verify actual role assumption.

See `findings/security-findings.md` for further testing details and security recommendations.

## Repository Structure

```text
aws-iam-access-control-lab/
├── README.md
├── policies/
│   ├── readonly-s3-policy.json
│   └── upload-s3-policy.json
├── findings/
│   └── security-findings.md
└── screenshots/
    └── (project evidence)

```

## Documentation Note

The bucket name is represented by `bucket` as a placeholder. The actual bucket name was used during the AWS lab.

## Conclusion

This lab provided practical experience in designing IAM roles, writing least-privilege S3 policies, configuring trust relationships, and evaluating permissions using the AWS IAM Policy Simulator.
