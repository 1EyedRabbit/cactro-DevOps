# cactro-DevOps
Repo for cactro assignment

Steps followed:

Setting up the infra:
1. Created a EC2 Instance having Amazon Linux 2 and configured it to allow SSH only from my IP address.
2. Created a private S3 bucket and enabled both object versioning and server side encryption.
3. Created a custom IAM policy for the S3 bucket and added the following permissions which ensured least privilege access to the bucket: s3:PutObject, s3:GetObject, s3:ListBucket
4. Assigned the policy created in step#3 to a custom role and attached the role to the EC2 instance created in step#1.

Scripting part:
1. To simulate log generation at /var/log/app.log, created a cron job to run for every 1 minute using the following script:
  #!/bin/bash
  # Append a timestamped log entry
  echo "$(date +'%Y-%m-%d %H:%M:%S') - Sample log entry" >> /var/log/app.log
2. Then, created the upload-to-S3 script.
3. Modified the ownership of both the above scripts by adding +x (making the scripts executable)
4. Scheduled both the scripts to run at the proper time intervals in crontab.

For testing the upload and locating the uploaded logs:
1. Execute the upload-to-S3 script and check the audit log /var/log/s3_upload_audit.log for success/failure messages
2. The logs can be located at the path <S3-bucket-name>/logs/<date-of-upload>/<file-name>
