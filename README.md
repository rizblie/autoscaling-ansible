# autoscaling-ansible

This repo contains samples that demonstrate various options for using Ansible
in conjunction with EC2 AutoScaling groups.

## Run Ansible using AWS SSM State Manager

1. Create an S3 bucket to hold the Ansible configuration 
2. Create a folder `ansible/` in the bucket
3. Upload the file `ssm-state-manager/ssm-playbook.yml` to the `ansible/` folder in the bucket.
4. Run the CloudFormation template `ssm-state-manager/Cfn-SSM-Ansible.yaml`. You can accept the default values where provided.
   - Specify your bucket name for both the Ansible bucket and the Ansible Logs bucket.
   - Specify an `EnvironmentName` e.g. `ssm-poc`. This is used to tag
    resources and generate resource names that are specific to a
    particular environment.

The CloudFormation template spins up an ASG and configures an SSM State Manager Association for all the instances in the ASG. The association
uses Ansible to configure each instance at creation time, and also regularly checks/updates each instance on a 30 minute interval.

Note that there is no need to install Ansible as part of the EC2 LaunchConfiguration UserData, as SSM automatically installs Ansible on each instance upon creation.


## Set up EC2 instances to use `ansible-pull`

In this sample, EC2 instances in the ASG use ansible-pull to fetch the intial
Ansible configuration from a git repo. This configuration then includes
a cron job that polls the repo for changes using ansible-pull on a cron chedule.

1. Create a CodeCommit repo to hold your ansible config files e.g. `ansible-configs`
2. Copy the file `site.yml` (from the `ansible-pull` directory) to the repo and commit.
3. Use the CloudFormation template `Cfn-Ansible-Pull.yaml` to create an AutoScaling group. This includes a userdata script to pull the ansible config from the repo.
