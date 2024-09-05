# aws-rds-postgresql



# my curated AWS RDS PostgreSQL CLI Command Reference

This guide provides a comprehensive list of AWS CLI commands for managing RDS PostgreSQL instances. It covers various operations from creating and modifying instances to monitoring and maintenance tasks.

## Table of Contents

1. [Instance Management](#instance-management)
   - [Create Instance](#create-instance)
   - [Modify Instance](#modify-instance)
   - [Delete Instance](#delete-instance)
   - [Reboot Instance](#reboot-instance)
   - [Start Instance](#start-instance)
   - [Stop Instance](#stop-instance)
2. [Snapshots and Backups](#snapshots-and-backups)
   - [Create Snapshot](#create-snapshot)
   - [Copy Snapshot](#copy-snapshot)
   - [Restore from Snapshot](#restore-from-snapshot)
   - [Delete Snapshot](#delete-snapshot)
3. [Parameter Groups](#parameter-groups)
   - [Create Parameter Group](#create-parameter-group)
   - [Modify Parameter Group](#modify-parameter-group)
   - [Delete Parameter Group](#delete-parameter-group)
4. [Security Groups](#security-groups)
   - [Create Security Group](#create-security-group)
   - [Modify Security Group](#modify-security-group)
   - [Delete Security Group](#delete-security-group)
5. [Monitoring and Maintenance](#monitoring-and-maintenance)
   - [Describe Instances](#describe-instances)
   - [Describe Logs](#describe-logs)
   - [Apply Minor Version Upgrade](#apply-minor-version-upgrade)
6. [Read Replicas](#read-replicas)
   - [Create Read Replica](#create-read-replica)
   - [Promote Read Replica](#promote-read-replica)
7. [Tags](#tags)
   - [Add Tags](#add-tags)
   - [Remove Tags](#remove-tags)
8. [Option Groups](#option-groups)
   - [Create Option Group](#create-option-group)
   - [Modify Option Group](#modify-option-group)
   - [Delete Option Group](#delete-option-group)

## Instance Management

### Create Instance

```bash
aws rds create-db-instance \
    --db-instance-identifier mydbinstance \
    --db-instance-class db.t3.micro \
    --engine postgres \
    --engine-version 13.7 \
    --master-username myusername \
    --master-user-password mypassword \
    --allocated-storage 20 \
    --vpc-security-group-ids sg-0123456789abcdef0 \
    --availability-zone us-west-2a \
    --db-name mydbname \
    --port 5432
```

**Hint**: Adjust the `--db-instance-class`, `--engine-version`, and other parameters as needed for your specific use case.

### Modify Instance

```bash
aws rds modify-db-instance \
    --db-instance-identifier mydbinstance \
    --db-instance-class db.t3.small \
    --apply-immediately
```

**Hint**: Use `--apply-immediately` to apply changes immediately, or omit it to apply during the next maintenance window.

### Delete Instance

```bash
aws rds delete-db-instance \
    --db-instance-identifier mydbinstance \
    --skip-final-snapshot
```

**Hint**: Remove `--skip-final-snapshot` and add `--final-db-snapshot-identifier` if you want to create a final snapshot before deletion.

### Reboot Instance

```bash
aws rds reboot-db-instance \
    --db-instance-identifier mydbinstance
```

### Start Instance

```bash
aws rds start-db-instance \
    --db-instance-identifier mydbinstance
```

### Stop Instance

```bash
aws rds stop-db-instance \
    --db-instance-identifier mydbinstance
```

## Snapshots and Backups

### Create Snapshot

```bash
aws rds create-db-snapshot \
    --db-instance-identifier mydbinstance \
    --db-snapshot-identifier mydbsnapshot
```

### Copy Snapshot

```bash
aws rds copy-db-snapshot \
    --source-db-snapshot-identifier arn:aws:rds:us-west-2:123456789012:snapshot:mydbsnapshot \
    --target-db-snapshot-identifier mynewdbsnapshot \
    --kms-key-id arn:aws:kms:us-west-2:123456789012:key/abcd1234-a123-456a-a12b-a123b4cd56ef
```

**Hint**: Use `--kms-key-id` to encrypt the copied snapshot with a custom KMS key.

### Restore from Snapshot

```bash
aws rds restore-db-instance-from-db-snapshot \
    --db-instance-identifier mynewdbinstance \
    --db-snapshot-identifier mydbsnapshot \
    --db-instance-class db.t3.micro
```

### Delete Snapshot

```bash
aws rds delete-db-snapshot \
    --db-snapshot-identifier mydbsnapshot
```

## Parameter Groups

### Create Parameter Group

```bash
aws rds create-db-parameter-group \
    --db-parameter-group-name myparametergroup \
    --db-parameter-group-family postgres13 \
    --description "My custom parameter group"
```

### Modify Parameter Group

```bash
aws rds modify-db-parameter-group \
    --db-parameter-group-name myparametergroup \
    --parameters "ParameterName=max_connections,ParameterValue=250,ApplyMethod=immediate"
```

### Delete Parameter Group

```bash
aws rds delete-db-parameter-group \
    --db-parameter-group-name myparametergroup
```

## Security Groups

### Create Security Group

```bash
aws rds create-db-security-group \
    --db-security-group-name mysecuritygroup \
    --db-security-group-description "My security group"
```

### Modify Security Group

```bash
aws rds authorize-db-security-group-ingress \
    --db-security-group-name mysecuritygroup \
    --cidrip 203.0.113.0/24
```

### Delete Security Group

```bash
aws rds delete-db-security-group \
    --db-security-group-name mysecuritygroup
```

## Monitoring and Maintenance

### Describe Instances

```bash
aws rds describe-db-instances \
    --db-instance-identifier mydbinstance
```

### Describe Logs

```bash
aws rds describe-db-log-files \
    --db-instance-identifier mydbinstance
```

### Apply Minor Version Upgrade

```bash
aws rds modify-db-instance \
    --db-instance-identifier mydbinstance \
    --engine-version 13.8 \
    --apply-immediately
```

## Read Replicas

### Create Read Replica

```bash
aws rds create-db-instance-read-replica \
    --db-instance-identifier myreadreplica \
    --source-db-instance-identifier mydbinstance
```

### Promote Read Replica

```bash
aws rds promote-read-replica \
    --db-instance-identifier myreadreplica
```

## Tags

### Add Tags

```bash
aws rds add-tags-to-resource \
    --resource-name arn:aws:rds:us-west-2:123456789012:db:mydbinstance \
    --tags Key=Environment,Value=Production
```

### Remove Tags

```bash
aws rds remove-tags-from-resource \
    --resource-name arn:aws:rds:us-west-2:123456789012:db:mydbinstance \
    --tag-keys Environment
```

## Option Groups

### Create Option Group

```bash
aws rds create-option-group \
    --option-group-name myoptiongroup \
    --engine-name postgres \
    --major-engine-version 13 \
    --option-group-description "My option group"
```

### Modify Option Group

```bash
aws rds modify-option-group \
    --option-group-name myoptiongroup \
    --options-to-include OptionName=JSONB,OptionSettings=[{Name=JSONB,Value=true}]
```

### Delete Option Group

```bash
aws rds delete-option-group \
    --option-group-name myoptiongroup
```

This guide covers the most common AWS CLI commands for managing RDS PostgreSQL instances. Remember to replace placeholder values (like `mydbinstance`, `myusername`, `mypassword`, etc.) with your actual values when using these commands.

For more detailed information on each command and additional options, refer to the [official AWS CLI documentation for RDS](https://docs.aws.amazon.com/cli/latest/reference/rds/index.html).
