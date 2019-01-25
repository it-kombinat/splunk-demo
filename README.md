# ansible-splunk-demo

So far I've only used and tested this playbook on AWS instances.
Presentation about this Demo can find [here](https://it-kombinat.org)

# Content of this Repository
This Repo contains two Ansible plays, Ansible-Roles for these plays and an Ansible configuration file.

- `ec2-splunk-basic.yml`
- `ec2-splunk-docker.yml`
- `ec2-splunk-snort.yml`
- `roles.yml`
- `ansible.cfg`

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

## `ec-splunk-snort.yml`
Splunk as a Service with on-boarding of the following services
* [Docker-Collector](https://www.outcoldsolutions.com/docs/monitoring-docker/)
* [SNORT](https://github.com/it-kombinat/ansible-snort)
* [Cowrie - Honeypot](https://github.com/it-kombinat/ansible-cowried)
* [Badguy](https://github.com/it-kombinat/badguy)

# Expectations

This ansible package expectes your servers to be EL base OS (RHEL7/CENTOS7). The splunk binaries currently set are *Splunk 7.1*

## Uploading Splunk RPM to S3 Bucket
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

## Customizing Variables for your Environment
Configure variables in [ec2-splunk-snort.yml](https://github.com/it-kombinat/splunk-demo/blob/master/ec2-splunk-snort.yml) and [group_vars/all](hittps://github.com/it-kombinat/splunk-demo/blob/master/group_vars/all.yml)

```
keypair: <Name of your SSH-KEY> # Name of your SSH-Key Name 
dyn_dns: true|false # Enable or disable DNS management - Default is false
dyn_zone: example.com # DNS Zonename of your route53 Zone
dyn_hostname: spaas.example.com # # Hostname 
```

## Installing roles
All necessary roles has to be downloaded with the galaxy command or `git clone`
```
ansible-galaxy install -r roles.yml --force
```
## Playbook run

Basic play
```
ansible-playbook ec2-splunk-basic.yml
```

Docker play
```
ansible-playbook ec2-splunk-docker.yml
```

Snort play
```
ansible-playbook ec2-splunk-snort.yml
```

## Splunk Account Information
```
**username:** admin
**password:** admin123 [default] - see Ansbile Role [ansible-splunk-basic]
**user:** https://[public-ip-of-the-ec2-instance]:8000
```

## Future

There's a few things I'm looking to do to make this play more re-usable, namely:

   * Increase the idempotency
   * more secure - Ansible vault for the variable -  `splunk_admin_passwd`
   * number of other minor modifications
