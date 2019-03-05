# Script_SX
Script per iniciar SX


#!/bin/bash
# ------ Creado por Marc------------
# Per executar fem: sudo su, sx2019 i ./Script
# Creem els bridges i els aixecem
brctl addbr br1
ip link set dev br1 up
brctl addbr br2
ip link set dev br2 up
brctl addbr br3
ip link set dev br3 up
# Engeguem els contenidors
lxc-start -n R1
lxc-start -n R2
lxc-start -n host1
lxc-start -n host2
#Entrem a host1 i afagim R1 com a default gw
lxc-attach -n host1 -- bash -c 'route add default gw 192.168.1.1 eth0'
#Entrem a host2 i afagim R2 com a default gw
lxc-attach -n host2 -- bash -c 'route add default gw 192.168.2.1 eth0'
#Entrem a R1 i afagim br2 com a ruta estàtica
lxc-attach -n R1 -- bash -c 'ip route add 192.168.2.0/24 via 10.0.0.2 dev eth0'
#Entrem a R2 i afagim br1 com a ruta estàtica
lxc-attach -n R2 -- bash -c 'ip route add 192.168.1.0/24 via 10.0.0.1 dev eth0'
