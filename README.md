# How-to-configure-vyatta-router
In this document is explained how it is the Vyatta router configured to be used as bridge between an internal and external network


How to configure Vyatta
-----------------------

Assumptions:
------------

External interface: eth0
External network: 10.90.149.0/24

Internal interface: eth1
Internal network: 192.168.0.0/24



Configuration in vyatta console:
--------------------------------
Login
    User: Vyatta
    Pass: Vyatta
configure
----- Network interfaces ---------

set interfaces ethernet eth0 address dhcp
set interfaces ethernet eth1 address 192.168.0.1/24
commit
save

----Nat rule -------
set nat source rule 10 source address 192.168.0.0/24
set nat source rule 10 outbound-interface eth0
set nat source rule 10 translation address masquerade
commit
save


Configuration in Domain Controller (AD):
---------------------------------------
Router option of the DHCP has to be 192.168.0.1 to use as GW by the internal machines

Configuration in client (local) machine:
----------------------------------------

10.90.149.36 is the ip that the vyatta eth0 interface got from the dhcp server of the external network:

route -p add 192.168.0.0 mask 255.255.255.0 10.90.149.36
