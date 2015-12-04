# ansible

This is a minimalistic style ELK demo playbook.

Some roles depend on the files in ../logstash/

<pre>
$EDITOR ansible-inventory
ansible-galaxy install -r requirements.yml
ansible-playbook -i ansible-inventory setup.yml
</per>

All the variables are set as defaults in their roles, if you'd like to change any I'd suggest putting them in group_vars/ELK/ELK.yml:

<pre>
mkdir -p group_vars/ELK/
$EDITOR group_vars/ELK/ELK.yml
</pre>
