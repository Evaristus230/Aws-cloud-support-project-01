# AWS Global Infrastructure: Regions and Availability Zones

## 1. Project Information

| Item | Details |
|---|---|
| Project | AWS Foundations & Account Operations |
| Concept | AWS Global Infrastructure |
| Cloud Provider | Amazon Web Services |
| AWS Region used | `ca-central-1` |
| Environment | Kali Linux |
| Interface | AWS CLI |
| Exercise type | Read-only infrastructure discovery |

## 2. Objective

The objective of this exercise was to understand AWS global infrastructure and use AWS CLI to retrieve information about AWS Regions and Availability Zones.

The exercise focused on identifying geographical Regions, inspecting Region opt-in statuses, and retrieving Availability Zone names, identifiers, and operational states.

## 3. AWS Regions

An AWS Region is a separate geographical area where AWS operates cloud infrastructure.

Regions allow organisations to deploy workloads in locations that meet their operational, performance, data residency, and availability requirements.

### Practical implementation

The following command was executed to retrieve the AWS Regions catalogue:

```bash
aws ec2 describe-regions \
  --all-regions \
  --query 'Regions[].{Region:RegionName,Status:OptInStatus}' \
  --output table
```

### Command explanation

| Parameter | Description |
|---|---|
| `describe-regions` | Retrieves AWS Region information. |
| `--all-regions` | Includes Regions that may require opt-in. |
| `--query` | Filters the returned information. |
| `--output table` | Displays the results in table format. |

### Verification

The command executed successfully and returned the AWS Region catalogue with Region identifiers and opt-in statuses.

**Evidence:** `evidence/02-global-infrastructure/aws-regions.avif`

## 4. AWS Availability Zones

An Availability Zone (AZ) consists of one or more discrete data centres with independent power, networking, and connectivity within an AWS Region.

Multiple Availability Zones within a Region allow organisations to design applications that can tolerate certain infrastructure failures.

### Practical implementation

The following command was executed to retrieve Availability Zone information for Canada (Central):

```bash
aws ec2 describe-availability-zones \
  --region ca-central-1 \
  --query 'AvailabilityZones[].{Zone:ZoneName,State:State,ZoneID:ZoneId}' \
  --output table
```

### Command explanation

| Parameter | Description |
|---|---|
| `describe-availability-zones` | Retrieves Availability Zone information. |
| `--region ca-central-1` | Specifies the AWS Region to inspect. |
| `ZoneName` | Returns the Availability Zone name. |
| `State` | Returns the Availability Zone operational state. |
| `ZoneId` | Returns the stable Availability Zone identifier. |

### Verification

The command executed successfully and returned Availability Zone information for `ca-central-1`.

The output included the Availability Zone names, operational states, and stable identifiers.

**Evidence:** `evidence/02-global-infrastructure/availability-zones.avif`

## 5. Technical Observations

The exercise established the following operational concepts:

1. AWS Regions represent separate geographical infrastructure areas.
2. Availability Zones exist within individual Regions.
3. A configured AWS CLI default Region does not restrict all AWS operations to that Region.
4. Region opt-in status helps determine whether an account can use a particular Region.
5. Availability Zone IDs provide stable identifiers across AWS accounts, unlike account-specific AZ name mappings.
6. Multi-AZ architecture can improve application resilience against certain Availability Zone failures.

## 6. Cloud Support Relevance

When investigating missing or inaccessible AWS resources, a cloud support engineer should verify the selected Region before concluding that a resource has been deleted or is unavailable.

Availability Zone information is also useful when investigating infrastructure placement, application availability, and potential AZ-specific disruptions.

## 7. Security and Cost Considerations

- The commands used in this exercise were read-only discovery operations.
- No AWS infrastructure resources were created.
- No additional infrastructure charges were intentionally incurred.
- AWS account identifiers and credentials should not be exposed in public evidence.

## 8. Evidence Directory

```text
evidence/
└── 02-global-infrastructure/
    ├── aws-regions.avif
    └── availability-zones.avif
```

## 9. Project Status

**Status:** Practical verification completed. Further AWS global infrastructure learning and documentation remain part of Project 01.

## 10. Availability Zone Filtering and Count

### Objective

To identify Availability Zones in `ca-central-1` whose operational state is reported as `available` and determine their total count.

### Command 1: Filter Available Availability Zones

```bash
aws ec2 describe-availability-zones \
  --region ca-central-1 \
  --filters Name=state,Values=available \
  --query 'AvailabilityZones[].{Zone:ZoneName,ZoneID:ZoneId,State:State}' \
  --output table
```

**Result:** The command executed successfully and returned Availability Zone information for zones in the available state.

### Command 2: Count Available Availability Zones

```bash
aws ec2 describe-availability-zones \
  --region ca-central-1 \
  --filters Name=state,Values=available \
  --query 'length(AvailabilityZones)' \
  --output text
```

**Observed Result:** 3 available Availability Zones.

### Technical Interpretation

The AWS CLI successfully filtered the Availability Zone information and returned a count of three available zones in `ca-central-1`.

This demonstrates how AWS CLI filters and JMESPath queries can be used for infrastructure discovery and operational reporting.

### Cloud Support Relevance

A Cloud Support Engineer can use these commands to:

- Identify Availability Zones reported as available.
- Verify infrastructure location options.
- Investigate regional and Availability Zone-related availability questions.
- Support infrastructure planning and troubleshooting.

### Important Operational Note

An Availability Zone being reported as `available` does not guarantee that every AWS service or resource is healthy within it. Service availability and resource health must be investigated separately.

### Evidence

The following screenshots document the practical exercise:

| Evidence file | Description |
|---|---|
| `available-availability-zones.png` | Filtered list of available Availability Zones |
| `availability-zones-count.png` | CLI output showing three available Availability Zones |

Evidence location:

`evidence/02-global-infrastructure/`

### Exercise Status

**Status:** Successfully executed and verified.

Both AWS CLI commands completed successfully, and the filtered Availability Zone count was three.
