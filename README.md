# AWS-Errors-Resolutions 
Collection of AWS cloud issues faced in real production environments, along with their root causes and practical solutions.

Amazon Web Services offers highly scalable and reliable cloud services. However, in complex enterprise setups, engineers often face issues related to IAM, networking, scaling, security, and service limits. Below are some commonly encountered AWS errors and how to resolve them.

# (1) "AccessDenied: User is not authorized to perform this action"

What it is:
- AWS denies access because the user or role does not have sufficient permissions.

Possible causes:
- Missing IAM policy permissions.
- Incorrect role attached to resource.
- Explicit deny in policy.
- SCP (Service Control Policy) restriction.

How to fix it:
- Review attached IAM policies.
- Check for explicit deny rules.
- Validate trust relationship.
- Verify SCP in AWS Organizations.
  
# (2) "RequestLimitExceeded: API rate limit exceeded"

What it is:
- AWS API requests have crossed the allowed limit.

Possible causes:
- High automation traffic.
- Misconfigured scripts.
- Infinite loops in API calls.
- Excessive monitoring tools.

How to fix it:
- Implement exponential backoff.
- Use AWS SDK retry mechanism.
- Reduce unnecessary API calls.
- Request service limit increase if needed.
  
# (3)"InsufficientInstanceCapacity"

What it is:
- AWS cannot launch instances due to lack of capacity.

Possible causes:
- High demand in selected AZ.
- Rare instance type.
- Spot capacity shortage.

How to fix it:
- Try different Availability Zones.
- Use multiple instance types.
- Enable Auto Scaling mixed instances.
- Use capacity reservations.

# (4) "InvalidAMIID.NotFound"

What it is:
- The specified AMI does not exist or is inaccessible.

Possible causes:
- AMI deleted.
- AMI in another region.
- Missing permission.
- Copy failed.

How to fix it:
- Verify AMI ID and region.
- Check AMI ownership.
- Recreate or copy AMI.
- Update launch templates.
  
# (5) "ThrottlingException"

What it is:
- AWS service is throttling requests due to overload.

Possible causes:
- High request frequency.
- Batch jobs running together.
- Monitoring overload.
- Lambda concurrency spikes.

How to fix it:
- Enable request batching.
- Implement retry logic.
- Increase service quotas.
- Optimize application design.
  
# (6) "DependencyViolation" (VPC / Networking)

What it is:
- AWS cannot delete a resource because it is in use.

Possible causes:
- ENI attached to instance.
- Route table association.
- NAT gateway dependency.
- Load balancer attachment.

How to fix it:
- Identify dependent resources.
- Detach ENIs.
- Remove route associations.
- Delete in correct order.

# (7) "VolumeInUse" (EBS Error)

What it is:
- EBS volume is already attached and cannot be modified.

Possible causes:
- Volume mounted on instance.
- Snapshot in progress.
- Improper unmount.

How to fix it:
- Unmount volume properly.
- Detach from instance.
- Wait for snapshot completion.
- Force detach only if necessary.
  
# (8) "SignatureDoesNotMatch"

What it is:
- AWS rejects request due to invalid request signature.

Possible causes:
- Wrong access keys.
- System time mismatch.
- Proxy altering requests.
- SDK misconfiguration.

How to fix it:
- Sync server time using NTP.
- Regenerate credentials.
- Reconfigure SDK.
- Check proxy settings.
  
# (9) "BucketAlreadyExists" (S3 Error)

What it is:
- S3 bucket name is already taken globally.

Possible causes:
- Name already used.
- Naming conflict.
- Automated script reuse.

How to fix it:
- Use unique naming pattern.
- Add company/project prefix.
- Include random suffix.
company-dev-logs-2026-xyz

# (10) "Health checks failed" (Load Balancer / Auto Scaling)

What it is:
- Instances are marked unhealthy.

Possible causes:
- Application not responding.
- Wrong health check path.
- Firewall blocking traffic.
- Port mismatch.

How to fix it:
- Verify application endpoint.
- Check security groups.
- Match health check port.
- Review instance logs.
  
# (11) "Target group has no registered targets"

What it is:
- Load balancer has no healthy instances.

Possible causes:
- Instances not registered.
- Auto Scaling failure.
- Wrong target group.
- Port mismatch.

How to fix it:
- Register instances properly.
- Check Auto Scaling health.
- Verify target group config.
- Validate ports.

# (12)"Lambda function timed out"

What it is:
- Lambda execution exceeded timeout.

Possible causes:
- Long-running logic.
- External API delay.
- Database latency.
- Insufficient memory.

How to fix it:
- Increase timeout.
- Optimize code.
- Increase memory.
- Use Step Functions for long jobs.

# (13) "TooManyRequestsException (Lambda / API Gateway)"

What it is:
- Requests exceed allowed concurrency.

Possible causes:
- Traffic spike.
- No throttling configured.
- Uncontrolled retries.
- DDoS-like behavior.

How to fix it:
- Set throttling limits.
- Configure reserved concurrency.
- Use SQS buffering.
- Implement rate limiting.
  
# (14) "CloudFormation stack stuck in UPDATE_ROLLBACK_FAILED"

What it is:
- Stack update failed and rollback also failed.

Possible causes:
- Resource deletion failure.
- Manual changes.
- Permission loss.
- Service bug.

How to fix it:
- Identify failed resource.
- Fix dependency issue.
- Continue rollback.
- Manually delete resource if needed.

# (15) "ServiceQuotaExceededException"

What it is:
- Service limit has been reached.

Possible causes:
- Too many instances.
- Excess IPs.
- High Lambda concurrency.
- Resource sprawl.

How to fix it:
- Check service quotas.
- Clean unused resources.
- Request quota increase.
- Implement resource governance.

# (16) "Security group rule limit exceeded"

What it is:
- Maximum inbound/outbound rules reached.

Possible causes:
- Too many IP whitelists.
- Poor network design.
- Repeated automation rules.

How to fix it:
- Use prefix lists.
- Consolidate rules.
- Use NACLs where suitable.
- Re-architect security design.
  
# Advanced Best Practices (Production Level)
To avoid most AWS production issues:
- Use least-privilege IAM
- Enable CloudTrail and Config
- Monitor with CloudWatch
- Automate using IaC (Terraform/CloudFormation)
- Enable budget alerts
- Implement tagging policy
- Maintain DR and backup strategy
