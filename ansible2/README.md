ansible2
--------

ELK 5

define these in your group_vars/ELK/secrets.yml (used to restrict ssh):

<pre>
trusted_public_networks: "ip1/32 ipsubet2/24"
trusted_public_ipv6_networks: ""
</pre>

You'll need a real SSL cert. One could use letsencrypt but then the httpd needs to allow connections from the Internet (while requesting the certifiate, having any part of the ELK server available in any form from the Internet is a bad idea) and you'll need to make symlinks from the certs to where the httpd config expects them to be:

Default locations of the SSL certs:
<pre>
        SSLCertificateFile /etc/pki/tls/certs/hostcert.pem
        SSLCertificateKeyFile /etc/pki/tls/private/hostkey.pem
</pre>
