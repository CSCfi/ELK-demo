ansible
=======

Has the ansible playbook that sets up the ELK stack on a single VM - this VM could then be used to demo the ELK stack by giving the attendants a web interface where they can click around themselves.

Requirements: 
   * A VM with CentOS6 with Internet connectivity 
      * Works with RFC1918 IP on interface, as long as it has a public IP (like from public pool in openstack)
   * 3.5GB RAM and 1 core is enough for small demo

Run ansible:
   * ansible-playbook -i ansible-inventory setup.yml

Configure the firewall as necessary.
   * Allow remote TCP ports: 80 and 443 - both are HTTP - not https

Demo! 

More details in the wiki: https://github.com/CSC-IT-Center-for-Science/ELK-demo/wiki/Install-the-ELK-demo-VM-with-ansible

logstash
========

Here the logstash configs and patterns for the DEMO are stored.

   * To ingest some data:
      * /opt/logstash/bin/logstash agent -f /home/cloud-user/06-logstash-bf.conf
