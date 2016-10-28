==========================================================================
aws-ubuntu-eni
==========================================================================

This role configures and brings up a secondary network interface eth1 in an ubuntu server for
attachment of an aws elastic network interface.
AWS does not automatically configure secondary and bring up secondary network interfaces
for ubuntu based servers. They only do so for AWS linux Amis and some windows boxes.

Requirements
------------

This role was tested with aws ubuntu server 14.04 LTS
At least 2 subnets in the same VPC in the same availability zone.
The ENI should be in a different subnet from the instance's default subnet i.e eth0 subnet
The ENI should ideally be configured with an elastic public ip and a private ip
configuration of eth1 interface should match configuration of the target ENI in terms of:
       - subnet e.g. 192.168.1.0/24
       - private_ip e.g. 192.168.1.10



ENI requirements
-------------------

You can attach an elastic network interface in one subnet to an instance in another subnet in the same VPC;
however, both the elastic network interface and the instance must reside in the same Availability Zone.

see more at http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html#creating-a-management-network

Role Variables
--------------

The variables that can be passed to this role and a brief description about
them are as follows.

Assuming that your ENI has the following definition:
    - subnet: 192.168.1.0/24
    - network_address: 192.168.1.0
    - private_ip: 192.168.1.10
    - gateway: 192.168.1.1

Then define the following role variables as follows:
    eni_subnet: 192.168.1.0/24
    eni_network_address: 192.168.1.0
    eni_private_ip: 192.168.1.10
    subnet_mask: 255.255.255.0 # you can ditermine the subnet mask from the CIDR notaton 192.168.1.0/24
    eni_gateway: 192.168.1.1 #AWS automatically assigns the fist usable host address as gateway address of that network or subnet







Examples
========
To configure eth1 for use withe the ENI defined above, add the following to
your playbook (eg configure.yml) and run the playbook

    - hosts: load_balancers
      roles:
        - role: aws-ubuntu-eni
          eni_subnet: 192.168.1.0/24
          eni_network_address: 192.168.1.0
          eni_private_ip: 192.168.1.10
          subnet_mask: 255.255.255.0 # you can ditermine the subnet mask from the CIDR notaton 192.168.1.0/24
          eni_gateway: 192.168.1.1 #AWS automatically assigns the fist usable host address as gateway address of that network or subnet






Dependencies
------------

None

License
-------

MIT

Author Information
------------------

Kyrelos Obat


