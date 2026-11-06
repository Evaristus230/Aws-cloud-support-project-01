# AWS Account Foundations and Environment Verification

## 1. Project Overview

This document records the initial AWS environment verification performed as part of Project 01: AWS Foundations & Account Operations.

The purpose of this exercise was to establish that the Kali Linux working environment could interact with AWS through the AWS Command Line Interface (AWS CLI).

The exercise verified CLI installation, default Region configuration and AWS authentication.

## 2. Objectives

The objectives were to:

- Verify that the AWS CLI is installed and operational.
- Identify the configured default AWS Region.
- Verify that the CLI can successfully authenticate with AWS.
- Establish a reliable baseline before performing further cloud operations.

## 3. Working Environment

| Component | Configuration |
|---|---|
| Operating system | Kali Linux |
| Virtualisation platform | Oracle VirtualBox |
| AWS interface | AWS CLI |
| Default AWS Region | ca-central-1 |
| Project repository | AWS Cloud Support Project 01 |

The exact AWS CLI version is recorded in the terminal evidence.

## 4. Practical Implementation

### 4.1 Verify AWS CLI installation

**Command:**

```bash
aws --version
```

**Purpose:**

Displays the installed AWS CLI version and associated runtime information.

**Verification:**

The command executed successfully and displayed the installed AWS CLI version.

**Operational relevance:**

Checking the CLI version is an initial troubleshooting step when investigating command compatibility or AWS CLI behaviour.

### 4.2 Verify the default AWS Region

**Command:**

```bash
aws configure get region
```

**Purpose:**

Retrieves the default Region configured for the AWS CLI profile.

**Observed result:**

```text
ca-central-1
```

**Interpretation:**

The AWS CLI default Region is configured as Canada Central.

This setting determines the default Region used by regional AWS service commands when no explicit Region override is provided.

### 4.3 Verify AWS authentication

**Command:**

```bash
aws sts get-caller-identity --query 'Account != null' --output text
```

**Purpose:**

Uses AWS Security Token Service (STS) to verify the identity making the authenticated request.

The JMESPath query checks whether an account identifier was returned without displaying the actual identifier.

**Observed result:**

```text
True
```

**Interpretation:**

The CLI successfully authenticated with AWS and received an identity response containing an account identifier.

This confirms that the CLI session can communicate with the AWS STS endpoint.

## 5. Evidence

The following evidence is maintained in the project's evidence directory:

| Evidence | Description |
|---|---|
| 01-aws-cli-environment-verification.png | AWS CLI version, configured Region and authentication verification |

Evidence directory:

`evidence/01-account-foundations/`

## 6. Security Considerations

- AWS credentials must never be committed to a public GitHub repository.
- Account identifiers, ARNs and other sensitive operational details should be protected when collecting screenshots.
- Authentication should use an appropriately secured AWS identity.
- The principle of least privilege should guide permissions for future AWS operations.

## 7. Cost Considerations

This exercise used AWS CLI verification commands and an STS identity lookup.

No EC2 instances, databases, storage resources or other billable infrastructure were provisioned as part of this exercise.

## 8. Troubleshooting Knowledge

Common issues that may affect this verification process include:

| Problem | Possible explanation |
|---|---|
| `aws: command not found` | AWS CLI is missing or not available in the shell PATH. |
| `Unable to locate credentials` | The CLI cannot locate usable AWS credentials. |
| Expired token | The current temporary credentials have expired. |
| Unexpected Region | The CLI is using a different profile or Region configuration. |
| Authentication failure | The configured credential source may be invalid or unavailable. |


