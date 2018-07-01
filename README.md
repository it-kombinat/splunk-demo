# ansible-splunk-basic

DEMO - Starting EC2 instance and installing Splunk

# How to use

## Installing roles
```
ansible-galaxy install -r roles.yml --force
```
## Adding Variables
```
export AWS_ACCESS_KEY_ID="NUHKOIJFOJF9GFJDO" 
export AWS_SECRET_ACCESS_KEY="LSDJKFODSJF9SDJF8UH3U3HFKW"
```
## Playbook run
```
ansible-playbook ec2-splunk-basic.yml
```
