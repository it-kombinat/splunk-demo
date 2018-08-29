# ansible-splunk-demo

So far I've only used and tested this playbook on AWS instances.
Presentation about this Demo can find here: https://it-kombinat.github.io/slides-splunk-demo/

# Content of this Repository
This Repo contains two Ansible plays

- `ec2-splunk-basic.yml`
- `ec2-splunk-docker.yml`

# Splunk playbook and role Objectives

## `ec2-splunk-basic.yml`
This playbook deploys configuration changes to setup a Splunk Server.
* Starting EC2 instance
* Configure all-in-one Splunk instance on EC2
* Setup admin user
* adding rsyslog as Data input port 514(tcp)
* Provisioning EC2 instance(s) for Data input
* Starting RSYSLOG-Generator to provide logdata

## `ec2-splunk-docker.yml`
This playbook will do the following
* Starting EC2 instance
* Installing Docker on EC2
* Deploy Splunk as Docker container -  Enabling rsyslog and HEC within container
* Deploy [docker-collector](https://www.outcoldsolutions.com/docs/monitoring-docker/) on EC2
* Starting RSYSLOG-Generator as container to provide logdata
* Configure Callback-Plugin on Ansible - Forwarding Logs to [Splunk-Plugin](https://splunkbase.splunk.com/app/4023/)
* Starting additional EC2 instance for Badguy
* Deploy [Badguy as Container](https://github.com/it-kombinat/badguy) - this image contains an login cracker to simulate an attack [THC-Hydra](https://tools.kali.org/password-attacks/hydra)

# Expectations

This ansible package expectes your servers to be EL base OS (RHEL7/CENTOS7). The splunk binaries currently set are *Splunk 7.1*

##Uploading Splunk RPM to S3 Bucket
For the basic play, the rpm file is needed. e.g. in an S3 Bucket - See [Ansible Role](https://github.com/it-kombinat/ansible-splunk-base)
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

basic play
```
ansible-playbook ec2-splunk-basic.yml
```

docker play
```
ansible-playbook ec2-splunk-docker.yml
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
   * more secure - Ansible vault for the variable -  `splunk_admin_passwd`
   * number of other minor modifications
