# autoscaling-ansible

## Setup steps

1. Create a CodeCommit repo to hold your ansible config files e.g. `ansible-configs`
2. Copy the file `site.yml` (from the `ansible` directory) to the repo and commit.
3. Use the CloudFormation template `cfn/AS-Ansible-Pull.yaml` to create an AutoScaling group. This includes a userdata script to pull the ansible config from the repo.
