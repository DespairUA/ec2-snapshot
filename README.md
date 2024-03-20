# ec2-snapshot
AWS Linux ec2 snapshot script

Bash script for Automatic EBS Snapshots and Cleanup on Amazon Web Services (AWS)
Based on Casey Labs Inc. (http://www.caseylabs.com) aws-ec2-ebs-automatic-snapshot-bash script (https://github.com/CaseyLabs/aws-ec2-ebs-automatic-snapshot-bash)

===================================

How it works: ebs-snapshot.sh will:

Determine the instance ID of the EC2 server on which the script runs
Gather a list of all volume IDs attached to that instance
Take a snapshot of each attached volume
The script will then delete all associated snapshots taken by the script that are older than 7 days

===================================

IAM User: This script requires that new IAM user credentials be created, with the following IAM security policy attached:

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1426256275000",
            "Effect": "Allow",
            "Action": [
                "ec2:CreateSnapshot",
                "ec2:CreateTags",
                "ec2:DeleteSnapshot",
                "ec2:DescribeSnapshots",
                "ec2:DescribeVolumes"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}

===================================

sudo yum install python-pip -y
sudo pip install awscli

sudo aws configure
AWS Access Key ID: (Enter in the IAM credentials generated above.)
AWS Secret Access Key: (Enter in the IAM credentials generated above.)
Default region name: (The region that this instance is in: i.e. us-east-1, eu-west-1, etc.)
Default output format: (Enter "text".)```

cd ~
wget https://raw.githubusercontent.com/CaseyLabs/aws-ec2-ebs-automatic-snapshot-bash/master/ebs-snapshot.sh
chmod +x ebs-snapshot.sh
mkdir -p /opt/aws
sudo mv ebs-snapshot.sh /opt/aws/


You should then setup a cron job in order to schedule a daily backup. Example crontab jobs:

@daily /opt/aws/ebs-snapshot.sh
