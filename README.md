# aws-rds



aws rds create-db-instance-read-replica \
    --db-instance-identifier myreadreplica \
    --region us-west-2 \
    --source-db-instance-identifier arn:aws:rds:us-east-1:123456789012:db:mydbinstance
    

aws rds create-db-instance-read-replica \
    --db-instance-identifier myreadreplica \
    --region ap-south-2 \
    --kms-key-id "arn:aws:kms:ap-south-2:510148569630:key/834b3f3b-15cf-42af-8c2e-04b1d41fb61e" \
    --source-db-instance-identifier arn:aws:rds:ap-south-1:510148569630:db:database-1
