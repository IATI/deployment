# IATI Website

## Project repository

https://github.com/IATI/preview-website


## Deployment process

The site is deployed to a VPS using Ansible:
`ansible-playbook wagtail.yml -i servers.ini --ask-vault-pass`

Ask @allthatilk or @dalepotter for the Ansible vault password.


## Deployment service

Single-purpose VPS server, provided by Digital Ocean.

@allthatilk @dalepotter and @k8hughes are owners of the IATI Team account on Digital Ocean, with add/remove user privileges.


## Expected deployments

- Production: `http://iatistandard.org/`
This is deployed from the `git_clone_branch` defined in the [project vars file]( https://github.com/IATI/deployment/blob/dev/roles/wagtail/wagtail_setup/vars/main.yml).
- Staging: This is set up ad hoc with access using the server IP address only (i.e. we do not use a domain)

The DNS A record for this domain is made in VPS.net DNS manager.


## Associated services

### S3 Storage Bucket

Amazon S3 provides storage for Database backups, which are stored with Amazon Web Services under the bucket name defined in the [project vars file]( https://github.com/IATI/deployment/blob/dev/roles/wagtail/wagtail_setup/vars/main.yml).

Login details for the AWS master account can be found in the IATI Asset Register.


## Administration

`/cms` can be added to the deployed URL to access the admin interface.

Amongst some others, @dalepotter @akmiller01 and @allthatilk are each set up with admin accounts with add/remove user privileges.


## Backups

### Backup policy

A cron command to backup the database is made in https://github.com/IATI/deployment/blob/dev/roles/wagtail/wagtail_setup/tasks/main.yml The backup is stored in an S3 storage bucket (see above).


### Restoring a backup

The script created by https://github.com/IATI/deployment/blob/dev/roles/wagtail/wagtail_setup/templates/postgres_recover_backup.sh.j2 can be run (locally on the server) to reinstate the most recent backup.

Depending on the circumstances whereby a restore is needed, it may be more worthwhile to rebuild the instance on a brand new VPS (see `Deployment` section, above).

*Note:* This will drop the existing database and make a copy of the last backup.

#### Process

```
# After SSHing into the server (as root)
$ service apache2 stop
$ /usr/local/bin/postgres_recover_backup.sh
$ service apache2 start
```
