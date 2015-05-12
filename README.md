ansible
=======

Has the ansible playbook that sets up the ELK stack on a single VM - this VM could then be used to demo the ELK stack.

   * modify ansible-inventory file - it should have the DNS to the VM - not the IP
   * modify setup.yml to your liking
   * elasticsearch settings are done in ansible/roles/ansible-elasticsearch/vars/my-vars.yml
      * Not all settings are actually used, see ansible/roles/ansible-elasticsearch/templates/ 

Run ansible:
   * ansible-playbook -i ansible-inventory setup.yml

Check that elasticsearch is running:
   * see /var/log/elasticsearch/demo.log
   * mkdir /data # or symlink if root partition has too little space
   * chown elasticsearch /data
   * service elasticsearch start

Configure the firewall as necessary.
   * Allow remote TCP ports: 80 and 443 - both are HTTP - not https

Check access:
   * http://DNStomachine/ should re-direct to kibana3
   * http://DNStomachine:443/ should re-direct to kibana4
   * http://DNStomachine/_cluster/health  -_ should show elasticsearch cluster health

logstash
========

   * ingest some data:
      * /opt/logstash/bin/logstash agent -f /home/cloud-user/06-nginx-logstash-bf.conf
