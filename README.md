ansible
=======

Has the ansible playbook that sets up the ELK stack on a single VM - this VM could then be used to demo the ELK stack.

It: 
   * Installs kibana3-4 (these are from tar balls in this git repo..), elasticsearch and logstash. 
   * Adds users, disables selinux and proxies everything via an httpd to make things a bit safer.

Requirements: 
   * A VM with CentOS6 with Internet connectivity 
      * Works with RFC1918 IP on interface, as long as it has a public IP (like from public pool in openstack)
   * Works with 3.5GB and 1 core

Configuration: 
   * Modify ansible-inventory file - it should have the DNS to the VM - not the IP
   * Config is done in setup.yml - but default should be OK
   * Elasticsearch settings are done in ansible/roles/ansible-elasticsearch/vars/my-vars.yml
      * For example if you want to increase memory given to elasticsearch - increase elasticsearch_heap_size in my-vars.yml
      * Not all settings are actually used, see ansible/roles/ansible-elasticsearch/templates/ 

Run ansible:
   * ansible-playbook -i ansible-inventory setup.yml

Check that elasticsearch is running (it probably isn't):
   * See /var/log/elasticsearch/demo.log
   * mkdir /data # or symlink if root partition has too little space
   * chown elasticsearch /data
   * service elasticsearch start

Configure the firewall as necessary.
   * Allow remote TCP ports: 80 and 443 - both are HTTP - not https

Then start kibana4 - it depends on elasticsearch during startup:
   * service logstash-web-kibana4 start

At this stage - for testing kibanas - send some docs to the elasticsearch. See the logstash configs in this repo.
   * After 03-logstash.conf you should have data in elasticsearch.

Check access:
   * For example http://DNStomachine/demo-2015.05.18/_search?pretty should display something
   * http://DNStomachine/ should re-direct to kibana3
   * http://DNStomachine:443/ should re-direct to kibana4
   * http://DNStomachine/_cluster/health  -_ should show elasticsearch cluster health

Kibana4:
   * When accessing for the first time it asks for a index, enter: demo-2015* and then select @timestamp. This is to not make it also match the demo-kibana-int index that houses the kibana dashboards.

For a VM in CSC's cPouta you can also just run these after running the ansible-playbook - this assumes many things which is why it's not in playbook: 

_-

<pre>

sudo -i
mkdir /mnt/data
chown elasticsearch /mnt/data
ln -s /mnt/data /data
service elasticsearch start
# sleep so that elasticsearch is really running before kibana4
sleep 20
service logstash-web-kibana4 start
echo "iptables -I INPUT 1 -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT" >> /etc/rc.local
echo "iptables -I INPUT 1 -m state --state NEW -m tcp -p tcp --dport 443 -j ACCEPT" >> /etc/rc.local
iptables -I INPUT 1 -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
iptables -I INPUT 1 -m state --state NEW -m tcp -p tcp --dport 443 -j ACCEPT
exit

</pre>

logstash
========

Here the logstash configs and patterns for the DEMO are stored.

   * ingest some data:
      * /opt/logstash/bin/logstash agent -f /home/cloud-user/06-logstash-bf.conf
