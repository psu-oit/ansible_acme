# POC for issuing Let's Encrypt certs and deploying to an apache webserver

This is a proof-of-concept quality example for issuing and deploying Let's Encrypt certificates.
Only minimal features are implemented. Support for things such as SANs and deploying to alternative servers
like IIS on Windows has not been explored.

This assumes a single control node will be used to request, store, and deploy certificates.

## Requirements

* [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
* boto3 python package if using route53 DNS

## Files

### inventory.yml

This file contains neccessary information about the managed nodes. The acme_certs variable for localhost contains a list of all certificates to issue. To deploy an issued cert to an apache webserver, add the certificate name to the acme_certs variable for it. By default, a host is assumed to be accessed by SSH. Additional configuration is required for WinRM.

### account.yml

This code creates the 'account' for use with the ACME directory. The account is used for setting the contacts for issued certificate and authenticating requests/responses through use of it's private key.

`ansible-playbook -i inventory.yml account.yml`

### issue_cert.yml

This playbook issues or renews all desired certificates listed in the inventory.

`ansible-playbook -i inventory.yml issue_cert.yml`

### deploy_cert.yml

Example playbook for deploying a certificate to a RHEL compatible system running httpd.

`ansible-playbook -i inventory.yml deploy_cert.yml`
