# Lapres_Modul5_JA07

#Tahap Persiapan

Topologi Soal
![image](https://user-images.githubusercontent.com/45744801/69426401-c7997a80-0d5f-11ea-91b1-ac91a1d7ff93.png)

A) Melakukan Subnetting. Disini kami menggunakan teknik VLSM.

![topologi5](https://user-images.githubusercontent.com/45744801/69427154-84400b80-0d61-11ea-8fd1-9bd99a5f5ca6.PNG)
note : tulisan berwarna kuning merupakan subnet dan biru merupakan switch. 
![image](https://user-images.githubusercontent.com/45744801/69427105-62df1f80-0d61-11ea-8a59-b27a58500ef5.png)

B) Membuat file topologi.sh di putty sesuai dengan Topologi soal.
```
# Switch
uml_switch -unix switch0 > /dev/null < /dev/null &
uml_switch -unix switch1 > /dev/null < /dev/null &
uml_switch -unix switch2 > /dev/null < /dev/null &
uml_switch -unix switch3 > /dev/null < /dev/null &
uml_switch -unix switch4 > /dev/null < /dev/null &
uml_switch -unix switch5 > /dev/null < /dev/null &
uml_switch -unix switch6 > /dev/null < /dev/null &

# Router
xterm -T PIKACHU -e linux ubd0=PIKACHU,jarkom umid=PIKACHU eth0=tuntap,,,10.151.72.33 eth1=daemon,,,switch6 eth2=daemon,,,switch5 mem=64M &
xterm -T VENUSAUR -e linux ubd0=VENUSAUR,jarkom umid=VENUSAUR eth0=daemon,,,switch5 eth1=daemon,,,switch4 eth2=daemon,,,switch3 mem=64M &
xterm -T BLASTOISE -e linux ubd0=BLASTOISE,jarkom umid=BLASTOISE eth0=daemon,,,switch6 eth1=daemon,,,switch0 eth2=daemon,,,switch1 mem=64M &
xterm -T ARCEUS -e linux ubd0=ARCEUS,jarkom umid=ARCEUS eth0=daemon,,,switch4 eth1=daemon,,,switch2 mem=64M &

# DNS + Web Server
xterm -T ARTICUNO -e linux ubd0=ARTICUNO,jarkom umid=ARTICUNO eth0=daemon,,,switch0 mem=128M &
xterm -T MEW -e linux ubd0=MEW,jarkom umid=MEW eth0=daemon,,,switch0 mem=128M &
xterm -T MEWTWO -e linux ubd0=MEWTWO,jarkom umid=MEWTWO eth0=daemon,,,switch2 mem=128M &
xterm -T MOLTRES -e linux ubd0=MOLTRES,jarkom umid=MOLTRES eth0=daemon,,,switch2 mem=128M &

# Client
xterm -T PSYDUCK -e linux ubd0=PSYDUCK,jarkom umid=PSYDUCK eth0=daemon,,,switch3 mem=64M &
xterm -T SNORLAX -e linux ubd0=SNORLAX,jarkom umid=SNORLAX eth0=daemon,,,switch1 mem=64M &

```

C) bash file topologi yang telah dibuat.

D) login ke semua uml.

E) Hilangkan tanda pagar (#) bagian net.ipv4.ip_forward=1 dengan perintah nano /etc/sysctl.conf pada semua router. yaitu PIKACHU, BLASTOISE, VENUSAUR, ARCEUS.

F) kemudian jalankan perintah sysctl -p disetiap router.

G) setting IP dari setiap UML dengan dengan perintah nano /etc/network/interfaces.

● PIKACHU 
```
auto eth0
iface eth0 inet static
address 10.151.72.34
netmask 255.255.255.252
gateway 10.151.72.33

auto eth1
iface eth1 inet static
address 192.168.1.137
netmask 255.255.255.252

auto eth2
iface eth2 inet static
address 192.168.1.141
netmask 255.255.255.252
```

● VENUSAUR
```
auto eth0
iface eth0 inet static
address 192.168.1.142
netmask 255.255.255.252
gateway 192.168.1.141

auto eth1
iface eth1 inet static
address 192.168.1.145
netmask 255.255.255.252

auto eth2
iface eth2 inet static
address 192.168.0.1
netmask 255.255.255.0
```
