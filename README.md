Installing OpenShift 4 with an iPXE boot iso
============================================

[Using PXE](https://docs.openshift.com/container-platform/4.4/installing/installing_bare_metal/installing-bare-metal.html#installation-user-infra-machines-pxe_installing-bare-metal) is a good way to automate UPI installations of OpenShift. In an environments without DHCP or where DHCP cannot be configured for PXE, a custom iPXE boot iso can be used instead.

Building the iso
----------------
1. Clone the iPXE repository locally:

   `git clone git://git.ipxe.org/ipxe.git`

2. Note the build dependencies at [https://ipxe.org/download](https://ipxe.org/download)

3. Create a custom menu in the src directory. See the rhcos.ipxe example in this repository. This configures a menu that includes five static ips that can be used for installation. It chain loads the node-specific iPXE files from an http server based on the node's mac address.

4. Build the iso with the custom menu embedded:

   `make bin/ipxe.iso EMBED=./rhcos.ipxe`
   
5. After the build is complete, the src/bin directory will contain an ipxe.iso file that can be used to boot your nodes.

Node-specific iPXE files
------------------------
The example menu serves the node-specific iPXE files over http using the hyphenated mac address of the node interface. Generating these can be automated using any process that makes sense for the environment. 

The ansible-example directory in this repo shows an example that uses Ansible to generate iPXE files from an inventory. It also demonstrates how to configure static ips with bonded interfaces.
