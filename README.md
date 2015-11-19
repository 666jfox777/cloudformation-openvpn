# CloudFormation OpenVPN Service

Included in this repository is a CloudFormation template that can be used to
bootstrap an auto scaling group with an OpenVPN service. Zipped configuration
files can be downloaded to the hosts and run.

On the CloudFormation side you'll need:

- Knowledge on how to create a CloudFormation stack.
- An S3 bucket to hold the VPN configuration (examples are provided).
- The VPC/Subnet/AZ information (or the network configuration).

For the VPN configuration, it's almost plug and play. The only values that you
need to fill out is the CIDR for the otherside (`push 10.0.0.0 255.0.0.0`) and
generate the certificate, keys, etc. To generate the certificates:

- `yum install -y easy-rsa`
- `cd /usr/share/easy-rsa/2.0/` (note that the version number may differ on your system)
- `source ./vars`
- `./clean-all`
- `./build-ca` (Follow the prompts)
- `./build-key-server server` (Follow the prompts)
- `./build-dh`
- `cd keys`
- `ls`

You've generated the files you'll need now. Copy the contents into the configs:

````
<ca></ca>      <--- /usr/share/easy-rsa/2.0/keys/ca.crt
<cert></cert>  <--- /usr/share/easy-rsa/2.0/keys/server.crt
<key></key>    <--- /usr/share/easy-rsa/2.0/keys/server.key
<dh></dh>      <--- /usr/share/easy-rsa/2.0/keys/dh2048.pem
````

Make sure to zip the config up and name it [key]-openvpn.zip as that's what the
template expects. Now when you launch the stack you'll have a working config.
