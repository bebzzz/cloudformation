Create ClaudWatch Alarms (CPU utilization, FreeMemory, FreeDiskSpace) for All RDS instances in the default region

# Get a list of DBInstances

$  aws rds describe-db-instances --query 'DBInstances[*]['DBInstanceIdentifier']' --output text  > ./list


# Create CloudFormation stacks in a loop

$  for name in $(cat ./list); \
    do \
	   aws cloudformation create-stack --stack-name CloudWatch-RDS-Alarm-${name} \
	   --template-url https://\<S3 URL\>/rds_alarms-template.yml \
	   --parameters ParameterKey=InstanceNameParameter,ParameterValue=$name ; \
	   sleep 2; \
	done; 
