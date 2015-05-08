ansible
=======

Has the ansible playbook that sets up the ELK stack on a single VM - this VM could then be used to demo the ELK stack.

   * modify ansible-inventory file - it should have the DNS to the VM - not the IP
   * modify setup.yml to your liking
   * after the VM is up - elasticsearch is configured to store everything in /data - change this/symlink as necessary.

Run ansible:
   * ansible-playbook -i ansible-inventory setup.yml

Check that elasticsearch is running:
   * see /var/log/elasticsearch/demo.log

Configure iptables as necessary.
   * Remote TCP ports: 80 and 443 - both are HTTP - not https

Check access:
   * http://DNStomachine/ should re-direct to kibana3
   * http://DNStomachine:443/ should re-direct to kibana3
   * http://DNStomachine/_cluster/health  -_ should show elasticsearch cluster health

logstash
========

   * ingest some data:
      * /opt/logstash/bin/logstash agent -f /home/cloud-user/06-nginx-logstash-bf.conf
