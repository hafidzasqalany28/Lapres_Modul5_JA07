# Lapres_Modul5_JA07

## Tahap Persiapan

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

● BLASTOISE
```
auto eth0
iface eth0 inet static
address 192.168.1.138
netmask 255.255.255.252
gateway 192.168.1.137

auto eth1
iface eth1 inet static
address 10.151.73.65
netmask 255.255.255.248

auto eth2
iface eth2 inet static
address 192.168.1.1
netmask 255.255.255.128
```
● ARCEUS
```
auto eth0
iface eth0 inet static
address 192.168.1.146
netmask 255.255.255.252
gateway 192.168.1.145

auto eth1
iface eth1 inet static
address 192.168.1.129
netmask 255.255.255.248
```

● ARTICUNO
```
auto eth0
iface eth0 inet static
address 10.151.73.66
netmask 255.255.255.248
gateway 10.151.73.65
```
● MEW
```
auto eth0
iface eth0 inet static
address 10.151.73.67
netmask 255.255.255.248
gateway 10.151.73.65
```
● MEWTWO
```
auto eth0
iface eth0 inet static
address 192.168.1.130
netmask 255.255.255.248
gateway 192.168.1.129
```
● MOLTRES
```
auto eth0
iface eth0 inet static
address 192.168.1.131
netmask 255.255.255.248
gateway 192.168.1.129
```
● PSYDUCK
```
auto eth0
iface eth0 inet static
address 192.168.0.2
netmask 255.255.255.0
gateway 192.168.0.1
```
● SNORLAX
```
auto eth0
iface eth0 inet static
address 192.168.1.2
netmask 255.255.255.128
gateway 192.168.1.1

```

H) ketikkan perintah service networking restart di setiap uml.

I) Routing semua router yang ada dengan membuat file rout.sh disetiap uml.

● PIKACHU
```
route add -net 10.151.73.64 netmask 255.255.255.248 gw 192.168.1.138
route add -net 192.168.1.0 netmask 255.255.255.128 gw 192.168.1.138
route add -net 192.168.1.144 netmask 255.255.255.252 gw 192.168.1.142
route add -net 192.168.0.0 netmask 255.255.255.0 gw 192.168.1.142
route add -net 192.168.1.128 netmask 255.255.255.248 gw 192.168.1.142
```
● VENUSAUR
```
route add -net 192.168.1.128 netmask 255.255.255.248 gw 192.168.1.146
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.168.1.141
```
● BLASTOISE
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.168.1.137
```
● ARCEUS
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.168.1.145
```
J) bash file rout.sh disetiap uml.

K) ketikkan perintah atau membuat file script yang berisi `iptables –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.0.0/16.` dan bash file tersebut.

L) Export proxy pada setiap UML dengan sintaks seperti di bawah ini:
```
export http_proxy="http://ITS-564415-2da29:6a7ed@proxy.its.ac.id:8080"
export https_proxy="http://ITS-564415-2da29:6a7ed@proxy.its.ac.id:8080"
export ftp_proxy="http://ITS-564415-2da29:6a7ed@proxy.its.ac.id:8080"
```
M) setelah itu lakukan perintah apt-get update di setiap uml.

N) Install DHCP-server di ARTICUNO  dengan perintah `apt-get install isc-dhcp-server`.

O) lalu ketikkan nano /etc/default/isc-dhcp-server dan ganti INTERFACES menjadi eth0.

P) kemudian ketikkan nano /etc/dhcp/dhcpd.conf lalu tambahkan config seperti dibawah.
```
subnet 10.151.73.64 netmask 255.255.255.248 {
} 

subnet 192.168.1.0 netmask 255.255.255.128 {
    range 192.168.1.2 192.168.1.126;
    option routers 192.168.1.1;
    option broadcast-address 192.168.1.127;
    option domain-name-servers 10.151.73.67, 202.46.129.2, 10.151.36.7;
    default-lease-time 600;
    max-lease-time 7200;
} 

subnet 192.168.0.0 netmask 255.255.255.0 {
    range 192.168.0.2 192.168.0.254;
    option routers 192.168.0.1;
    option broadcast-address 192.168.0.255;
    option domain-name-servers 10.151.73.67, 202.46.129.2, 10.151.36.7;
    default-lease-time 600;
    max-lease-time 7200;
}
```
Q) Setting IP Client SNORLAX dan PSYDUCK di `nano /etc/network/interface` seperti berikut :
```
auto eth0
iface eth0 inet dhcp
```
R) ketikkan service networking restart di client SNORLAX dan PSYDUCK.

S) Restart dengan perintah `service isc-dhcp-server restart`. Jika terjadi failed!, maka stop dulu, kemudian start kembali `service isc-dhcp-server stop` `service isc-dhcp-server start`.

T) Install DHCP-relay di BLASTOISE, PIKACHU, VENUSAUR  dengan perintah `apt-get install isc-dhcp-relay`.

U) Setelah berhasil, masukkan IP ARTICUNO (10.151.73.66) sesuai yang diminta DHCP relay.

V) Masukkan interface yang diminta dengan, 
●BLASTOISE
```
eth0 eth1 eth2
```
●PIKACHU
```
eth1 eth2
```
●VENUSAUR
```
eth0 eth2
```

## Soal

1) Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi PIKACHU menggunakan iptables, namun Satoshi melarang kalian menggunakan MASQUERADE karena terlalu mudah.

Jawab :
buat file script 1.sh (UML PIKACHU). kemudian bash script tersebut.
```
iptables -t nat -A POSTROUTING -s 192.168.0.0/16 -o eth0 -j SNAT --to-source 10.151.72.14
```
2) Karena keberadaan jaringan tersebut sudah mulai diketahui dari oleh jaringan luar, Satoshi pun merasa panik, karena merasa jaringannya masih belum aman. Oleh karena itu maka kalian diminta untuk mendrop semua akses SSH dari luar Topologi (UML) Kalian pada server yang memiliki ip DMZ (DHCP dan DNS SERVER) pada PIKACHU demi menjaga keamanan.

Jawab :
buat file script 2.sh (UML PIKACHU). kemudian bash script tersebut.
```
iptables -A FORWARD -p tcp --dport 22 -d 10.151.73.24/29 -i eth0 -j DROP
```
3) Karena tim kalian maksimal terdiri dari 2 atau 3 orang saja, Satoshi meminta kalian untuk hanya membatasi DHCP dan DNS server hanya boleh menerima maksimal 2 atau 3(jumlah kelompok) koneksi ICMP secara bersamaan yang berasal dari mana saja menggunakan iptables pada masing masing server , selebihnya akan di DROP.

Jawab :
buat file script 3.sh (UML MEW dan ARTICUNO). kemudian bash script tersebut.
```
iptables -A -p icmp -m connlimit --connlimit-above 2 --connlimit-mask 0 -j DROP
```
(4) Kalian juga diminta untuk mengkonfigurasi PIKACHU untuk dapat membedakan ketika MEW
diakses dari subnet AJK, akan diarahkan pada MEWTWO dengan port 1234.

Jawab :
buat file script 4.sh (UML PIKACHU). kemudian bash script tersebut.
```
iptables -t nat -A PREROUTING -d 10.151.73.67 -s 10.151.36.0/24 -p tcp --dport 1234 -j DNAT --to-destination 192.168.1.130:1234
```
5) Sedangkan ketika diakses dari subnet INFORMATIKA akan diarahkan pada MOLTRES dengan port 1234.

Jawab :
buat file script 5.sh (UML PIKACHU). kemudian bash script tersebut.
```
iptables -t nat -A PREROUTING -d 10.151.73.67 -s 10.151.252.0/22 -p tcp --dport 1234 -j DNAT --to-destination 192.168.1s.131:1234
```

kemudian kalian diminta untuk membatasi akses ke MEW yang berasal dari SUBNET AJK dan SUBNET INFORMATIKA dengan peraturan dibawah ini. Selain itu paket akan di REJECT.

(6) Akses dari subnet AJK hanya diperbolehkan pada pukul 08.00 - 17.00 pada hari Senin sampai
Jumat.

Jawab :
buat file script 6.sh (UML MEW). kemudian bash script tersebut.
```
iptables -A INPUT -s 10.151.36.0/24 -m time --timestart 08:00 --timestop 17:00 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -s 10.151.36.0/24 -m time --timestart 17:01 --timestop 07:59 -j REJECT
```
(7) Akses dari subnet INFORMATIKA hanya diperbolehkan pada pukul 17.00 hingga pukul
09.00 setiap harinya.

Jawab :
buat file script 7.sh (UML MEW). kemudian bash script tersebut.
```
iptables -A INPUT -s 10.151.252.0/22 -m time --timestart 09:01 --timestop 16:59 -j REJECT

```
10) Karena banyak paket yang di drop oleh tim kalian, Satoshi ingin agar semua paket didrop oleh firewall (dalam topologi) tercatat dalam log pada setiap UML yang memiliki aturan drop.

Jawab : 
buat file script 10.sh (UML PIKACHU). kemudian bash script tersebut.
```
iptables -N LOGGING 
iptables -A FORWARD -j LOGGING 
iptables -A LOGGING -m limit --limit 2/min -j LOG --log-prefix "IPTables-Dropped: " --log-level 4 
iptables -A LOGGING -j DROP
```
buat file script 10.sh (UML MEW). kemudian bash script tersebut.
```
iptables -N LOGGING 
iptables -A INPUT -j LOGGING 
iptables -A LOGGING -m limit --limit 2/min -j LOG --log-prefix "IPTables-Dropped: " --log-level 4 
iptables -A LOGGING -j DROP
```
buat file script 10.sh (UML ARTICUNO). kemudian bash script tersebut.
```
iptables -N LOGGING 
iptables -A INPUT -j LOGGING 
iptables -A LOGGING -m limit --limit 2/min -j LOG --log-prefix "IPTables-Dropped: " --log-level 4 
iptables -A LOGGING -j DROP
```
dan jangan lupa untuk mengganti semua kata DROP menjadi LOGGING.
