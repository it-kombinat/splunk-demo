# ansible-splunk-demo
==============

DEMO - Starting EC2 instance and installing and configure Splunk (all-in-one).
Provisioning of addionial EC2 instance(s) as Syslog-Client (Splunk-Onboarding).

## Splunk playbook and role Objectives
This playbook deploys configuration changes to setup a Splunk Server.
* Configure all-in-one Splunk instance on EC2
* Setup admin user
* adding rsyslog as Data input port 514(tcp)
* Provisioning EC2 instance(s) for Data input
* Starting RSYSLOG-Generator to provide logdata


## Testing

curl -k  http://<IPAddress>:8088/services/collector/event -H "Authorization: Splunk <token>" -d '{"event": "hello World"}'

docker run -it --entrypoint=/bin/bash --log-driver=splunk --log-opt splunk-token=f23b1ad6-5965-4537-bf69-b1aa54B1aa88 --log-opt splunk-url=http://54.93.226.176:8088 stackware/sysgen:1.1


## Expectations

This ansible package expectes your servers to be EL base OS (RHEL7/CENTOS7). The splunk binaries currently set are *Splunk 7.1*

###Uploading Splunk RPM to S3 Bucket
```
s3cmd put Downloads/splunk/splunk-7.1.1-8f0ead9ec3db-linux-2.6-x86_64.rpm  s3://<yourbucket>
```

# How to use

## Adding Variables
```
export AWS_ACCESS_KEY_ID="NUHKOIJFOJF9GFJDO"
export AWS_SECRET_ACCESS_KEY="LSDJKFODSJF9SDJF8UH3U3HFKW"
```

## Installing roles
All necessary roles has to be downloaded with the galaxy command or `git clone`
```
ansible-galaxy install -r roles.yml --force
```
## Playbook run
```
ansible-playbook ec2-splunk-basic.yml
```

## Splunk Account Information
```
**username:** admin
**password:** admin123 [default] - see Ansbile Role [ansible-splunk-basic]
**user:** https://[public-ip-of-the-ec2-instance]:8000
```

##Future

There's a few things I'm looking to do to make this play more re-usable, namely:

   * Increase the idempotency
   * Using docker container to run a All-In-One-Solution
   * more secure - Ansible vault for the variable -  `splunk_admin_passwd`
   * number of other minor modifications
