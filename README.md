ansible
=======

Has the ansible playbook that sets up the ELK stack on a single VM - this VM could then be used to demo the ELK stack by giving the attendants a web interface where they can click around themselves.

It: 
   * Installs kibana3+4 (these are from tar balls in this git repo..) as well as elasticsearch and logstash from yum repos. 
   * Adds users, disables selinux and proxies everything via an httpd to make things a bit safer. 
   * elasticsearch configuration like: data dir, cluster name
   * kibana3 config like: non-default index prefix
   * write a test event into elasticsearch

Requirements: 
   * A VM with CentOS6 with Internet connectivity 
      * Works with RFC1918 IP on interface, as long as it has a public IP (like from public pool in openstack)
   * 3.5GB RAM and 1 core is enough for small demo

Configuration: 
   * Modify ansible-inventory file - it should have the DNS to the VM - not the IP
   * Config is done in setup.yml - but default should be OK
   * Elasticsearch settings are done in ansible/roles/ansible-elasticsearch/vars/my-vars.yml
      * For example: 
	* if you want to increase memory given to elasticsearch - increase elasticsearch_heap_size in my-vars.yml
        * to put elasticsearch data edit the same file
      * Not all settings are actually used, see ansible/roles/ansible-elasticsearch/templates/ 

Run ansible:
   * ansible-playbook -i ansible-inventory setup.yml

Configure the firewall as necessary.
   * Allow remote TCP ports: 80 and 443 - both are HTTP - not https

Check access:
   * For example http://DNStomachine/demo-2015.05.18/_search?pretty should display something
   * http://DNStomachine/ should re-direct to kibana3
   * http://DNStomachine:443/ should re-direct to kibana4
   * http://DNStomachine/_cluster/health  -_ should show elasticsearch cluster health

Kibana4:
   * When accessing for the first time it asks for an index, enter: demo-2015* and then select @timestamp. This is to prevent it from matching the demo-kibana-int index.

Firewall for CentOS6 (assumes network firewalls/openstack security groups):

_-

<pre>

# This assumes you are doing network firewall / openstack security groups.
sudo -i
echo "iptables -I INPUT 1 -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT" >> /etc/rc.local
echo "iptables -I INPUT 1 -m state --state NEW -m tcp -p tcp --dport 443 -j ACCEPT" >> /etc/rc.local
iptables -I INPUT 1 -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
iptables -I INPUT 1 -m state --state NEW -m tcp -p tcp --dport 443 -j ACCEPT
exit

</pre>

logstash
========

Here the logstash configs and patterns for the DEMO are stored.

   * To ingest some data:
      * /opt/logstash/bin/logstash agent -f /home/cloud-user/06-logstash-bf.conf
