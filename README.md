ansible
=======

Has the ansible playbook that sets up the ELK stack on a single VM - this VM could then be used to demo the ELK stack by giving the attendants a web interface where they can click around themselves.

Requirements: 
   * A VM with CentOS6/CentOS7 with Internet connectivity 
      * Works with RFC1918 IP on interface, as long as it has a public IP (like from public pool in openstack)
   * 3.5GB RAM and 1 core is enough for small demo

Run ansible:
   * ansible-playbook -i ansible-inventory setup.yml

Configure the firewall as necessary.
   * Allow remote TCP ports: 80 and 443 - both are HTTP - not https
   * /etc/ntp.conf is not managed default but the service is set to start on boot

Demo! 

More details in the wiki: https://github.com/CSC-IT-Center-for-Science/ELK-demo/wiki/Install-the-ELK-demo-VM-with-ansible

logstash
========

Here the logstash configs and patterns for the DEMO are stored.

   * To ingest some data:
      * /opt/logstash/bin/logstash agent -f /home/cloud-user/06-logstash-bf.conf

fluentd
=======

Fluentd is also installed by default.

prod\_example
=============

variable:
<pre>logstash_prod_example: "yes"</pre>

If this is set to yes then we'll install templates and logstash configs as found in ../logstash/configs/prod_example and ../logstash/templates/es-template-demo.json

https for kibana4
======================

configurable by setting
<pre>kibana4_https: "on"
</pre>

Requires x509 certificates installed into /etc/pki/tls/certs/hostcert.pem and /etc/pki/tls/private/hostkey.pem - this is not handled by this playbook

ldaps for kibana4
======================

configurable by setting 
<pre>kibana4_ldap: True
</pre>

Requires a secrets.yml in ansible/secrets.yml

Example of that is found in ansible/roles/ansible-elk-httpd/defaults/main.yml
