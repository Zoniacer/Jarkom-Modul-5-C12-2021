# Jarkom-Modul-5-C12-2021

## C12

- Daffa Amanullah Setyawan - 05111940000071<br>
- Satrio Hanif Wicaksono - 05111940000103<br>
- Shidqi Dhaifullah - 05111940000108<br>

#### (A) Tugas pertama kalian yaitu membuat topologi jaringan sesuai dengan rancangan yang diberikan Luffy dibawah ini:
![map Jarkom-Modul-5-C12-2021](https://user-images.githubusercontent.com/81344394/145678380-82eb83fa-3e5a-470a-a28d-f76f5d1fe24f.png)<br>


#### (B) Karena kalian telah belajar subnetting dan routing, Luffy ingin meminta kalian untuk membuat topologi tersebut menggunakan teknik CIDR atau VLSM

##### Subnetting Dengan Teknik VLSM:
![vlsm map Jarkom-Modul-5-C12-2021](https://user-images.githubusercontent.com/81344394/145678395-53342010-999a-43a0-98ea-adcb5e17861c.png)<br>
![vlsm tree Jarkom-Modul-5-C12-2021](https://user-images.githubusercontent.com/81344394/145678397-aaad0364-4cce-45fe-aa14-3b21dc49f508.png)<br>


##### Pembagian IP:

Subnet  | NID           | Length    | Subnet Mask       | Jumlah IP
---     | ---           | ---       | ---               | ---
A1      | 10.20.0.0     | /30       | 255.255.255.252   | 2
A2      | 10.20.0.4     | /30       | 255.255.255.252   | 2
A3      | 10.20.0.128   | /25       | 255.255.255.128   | 101
A4      | 10.20.4.0     | /22       | 255.255.252.0     | 701
A5      | 10.20.0.16    | /29       | 255.255.255.248   | 3
A6      | 10.20.2.0     | /23       | 255.255.254.0     | 301
A7      | 10.20.1.0     | /24       | 255.255.255.0     | 201
A8      | 10.20.0.24    | /29       | 255.255.255.248   | 3

##### Setting Interface
- Foosha
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
address 10.20.0.1
netmask 255.255.255.252

auto eth2
iface eth2 inet static
address 10.20.0.5
netmask 255.255.255.252
```
- Water7
```
auto eth0
iface eth0 inet static
address 10.20.0.2
netmask 255.255.255.252
gateway 10.20.0.1

auto eth1
iface eth1 inet static
address 10.20.0.129
netmask 255.255.255.128

auto eth2
iface eth2 inet static
address 10.20.0.17
netmask 255.255.255.248

auto eth3
iface eth3 inet static
address 10.20.4.1
netmask 255.255.252.0
```
- Guanhao
```
auto eth0
iface eth0 inet static
address 10.20.0.6
netmask 255.255.255.252
gateway 10.20.0.5

auto eth1
iface eth1 inet static
address 10.20.2.1
netmask 255.255.254.0

auto eth2
iface eth2 inet static
address 10.20.1.1
netmask 255.255.255.0

auto eth3
iface eth3 inet static
address 10.20.0.25
netmask 255.255.255.248
```
- Doriki
```
auto eth0
iface eth0 inet static
address 10.20.0.18
netmask 255.255.255.248
gateway 10.20.0.17
```
- Jipangu
```
auto eth0
iface eth0 inet static
address 10.20.0.19
netmask 255.255.255.248
gateway 10.20.0.17
```
- Maingate
```
auto eth0
iface eth0 inet static
address 10.20.0.26
netmask 255.255.255.248
gateway 10.20.0.25
```
- Jorge
```
auto eth0
iface eth0 inet static
address 10.20.0.27
netmask 255.255.255.248
gateway 10.20.0.25
```
- Blueno
```
auto eth0
iface eth0 inet dhcp
```
- Cipher
```
auto eth0
iface eth0 inet dhcp
```
- Elena
```
auto eth0
iface eth0 inet dhcp
```
- Fukurou
```
auto eth0
iface eth0 inet dhcp
```

#### (C) Kalian juga diharuskan melakukan Routing agar setiap perangkat pada jaringan tersebut dapat terhubung.

Pada router `Foosha` masukkan perintah berikut pada terminal:
```
route add -net 10.20.0.128 netmask 255.255.255.128 gw 10.20.0.2
route add -net 10.20.0.16 netmask 255.255.255.248 gw 10.20.0.2
route add -net 10.20.4.0 netmask 255.255.252.0 gw 10.20.0.2
route add -net 10.20.2.0 netmask 255.255.254.0 gw 10.20.0.6
route add -net 10.20.1.0 netmask 255.255.255.0 gw 10.20.0.6
route add -net 10.20.0.24 netmask 255.255.255.248 gw 10.20.0.6
```

#### (D) Tugas berikutnya adalah memberikan ip pada subnet Blueno, Cipher, Fukurou, dan Elena secara dinamis menggunakan bantuan DHCP server. Kemudian kalian ingat bahwa kalian harus setting DHCP Relay pada router yang menghubungkannya.
- Install `isc-dhcp-server` pada DHCP Server (`Jipangu`)
- Install `isc-dhcp-relay` pada router `Foosha`, `Water7`, dan `Guanhao` sebagai DHCP Relay
- Pada masing-masing DHCP Relay, masukkan pada file `/etc/sysctl.conf` perintah berikut:
```
net.ipv4.ip_forward = 1
net.ipv4.conf.all.accept_source_route = 1
```
- Lakukan `sysctl -p`
- Arahkan DHCP Relay menuju DHCP Server `Jipangu` (Addr `10.20.0.19`) dengan mengedit file `/etc/default/isc-dhcp-relay` sehingga:
```
echo '# What servers should the DHCP relay forward requests to?
SERVERS="10.20.0.19"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES=""

# Additional options that are passed to the DHCP relay daemon?
OPTIONS="" '> /etc/default/isc-dhcp-relay
```
- Lakukan `service isc-dhcp-relay restart` pada DHCP Relay.
- Pada `Jipangu`, edit file `/etc/default/isc-dhcp-server` seperti berikut:
```
INTERFACES="eth0"
```
- Alokasikan IP Address untuk tiap subnet pada `/etc/dhcp/dhcpd.conf` di DHCP Server `Jipangu`:
```
subnet 10.20.0.128 netmask 255.255.255.128 {
    range 10.20.0.128  10.20.0.254 ;
    option routers 10.20.0.129;
    option broadcast-address 10.20.0.255;
    option domain-name-servers 10.20.0.19;
    default-lease-time 360;
    max-lease-time 7200;
}
subnet 10.20.0.16 netmask 255.255.255.248{}
subnet 10.20.4.0 netmask 255.255.252.0 {
    range 10.20.4.0 10.20.7.254 ;
    option routers 10.20.4.1;
    option broadcast-address 10.20.7.255;
    option domain-name-servers 10.20.0.19;
    default-lease-time 720;
    max-lease-time 7200;
}
subnet 10.20.2.0 netmask 255.255.254.0 {
    range 10.20.2.0 10.20.3.254 ;
    option routers 10.20.2.1;
    option broadcast-address 10.20.3.255;
    option domain-name-servers 10.20.0.19;
    default-lease-time 720;
    max-lease-time 7200;
}
subnet 10.20.1.0 netmask 255.255.255.0 {
    range 10.20.1.0 10.20.1.254 ;
    option routers 10.20.1.1;
    option broadcast-address 10.20.1.255;
    option domain-name-servers 10.20.0.19;
    default-lease-time 720;
    max-lease-time 7200;
}
```
- Lakukan `service isc-dhcp-server restart`

#### 1. Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE.
- Pada Foosha, jalankan perintah berikut:
```
iptables -t nat -A POSTROUTING -o eth0 -j SNAT -s 10.20.0.0/21 --to-source 192.168.122.[]
```
Keterangan:
  - `-t nat`: pada tabel `nat`
  - `-A POSTROUTING`: tambahkan pada chain `POSTROUTING`
  - `-o eth0`: output pada interface `eth0`
  - `-j SNAT`: target aturan untuk `SNAT`
  - `-s 10.20.0.0/21`: untuk semua paket dari subnet `10.20.0.0/21`
  - `--to-source 192.168.122.[]`: IP Source menggunakan rentang IP untuk `eth0` pada `Foosha` <br><br>

- Atur `nameserver` tiap node menuju DNS Server `Foosha`:
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
```

#### 2. Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan.
- Pada Foosha, Jalankan perintah `iptables -A FORWARD -p tcp --dport 80 -d 10.20.0.16/29 -i eth0 -j DROP `

##### Testing
- Pada Jipangu dan Doriki, install netcat melalui perintah `apt-get install netcat `
- Pada Jipangu dan Doriki, Jalankan perintah `nc -l -p 80 `
- Pada Foosha, Jalankan perintah `nmap -p 80 10.20.0.18 ` untuk menguji Doriki dan `nmap -p 80 10.20.0.19 ` untuk menguji Jipangu
- Foosha --> Doriki

![Screenshot 2021-12-11 163052](https://user-images.githubusercontent.com/73422724/145673505-1a557524-0536-401e-86ac-e98460a87085.png)

- Foosha --> Jipangu

![Screenshot 2021-12-11 163321](https://user-images.githubusercontent.com/73422724/145673517-8b0b5e93-db3a-432c-88b4-4c2425f2b24f.png)

- Doriki

![Screenshot 2021-12-11 163446](https://user-images.githubusercontent.com/73422724/145673522-24d63a9b-7686-4f12-a5e7-733d7feac770.png)

- Jipangu

![Screenshot 2021-12-11 163531](https://user-images.githubusercontent.com/73422724/145673527-8401c58f-0190-4c26-a329-fbd4d45a7bfa.png)

#### Kemudian kalian diminta untuk membatasi akses ke Doriki yang berasal dari subnet Blueno, Cipher, Elena dan Fukuro dengan beraturan sebagai berikut
#### 4. Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.

Dalam doriki dilakukan potongan kodingan berikut:

- Paket Blueno menggunakan kodingan:
```
iptables -A INPUT -s 10.20.0.128/25 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 10.20.0.128/25 -j REJECT
```
- Paket dari Chiper menggunakan kodingan:
```
iptables -A INPUT -s 10.20.4.0/22 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 10.20.4.0/22 -j REJECT
```
**keterangan** : <br>
`A INPUT` digunakan chain INPUT <br>
`m time` digunakan rule time<br>
`timestart dan timestop` Waktu mulai dan waktu berhenti<br>
`--weekdays` peraturan hari kemudian dilanjut hari yang diinginkan<br>
`-j ACCEPT dan -j REJECT` : Paket diterima dan ditolak<br>


**Testing**

- Blueno

![soal4 blueno](https://user-images.githubusercontent.com/63639703/145212575-5a2f7f0b-20fa-40ca-8b28-7f261d97d0a3.png)

- Chiper

![soal4 ciper](https://user-images.githubusercontent.com/63639703/145212621-91ca0fc4-1d90-478b-b52b-614ac7d0968d.png)

#### 5. Akses dari subnet Elena dan Fukuro hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya.

Dalam doriki dilakukan potongan kodingan berikut:

- Paket Elena menggunakan kodingan:
```
iptables -A INPUT -s 10.20.2.0/23 -m time --timestart 07:00 --timestop 15:00 -j REJECT
```
- Paket Fukurou menggunakan kodingan:
```
iptables -A INPUT -s 10.20.1.0/24 -m time --timestart 07:00 --timestop 15:00 -j REJECT
```
**keterangan** : <br>
`A INPUT` digunakan chain INPUT <br>
`m time` digunakan rule time<br>
`timestart dan timestop` Waktu mulai dan waktu berhenti<br>
`--weekdays` peraturan hari kemudian dilanjut hari yang diinginkan<br>
`-j ACCEPT dan -j REJECT` : Paket diterima dan ditolak<br>

**Testing**

- Elena

![soal5 elena](https://user-images.githubusercontent.com/63639703/145214673-a23cfda3-c088-4290-9125-e9a99a60fc96.png)

- Fukurou

![soal5 elena](https://user-images.githubusercontent.com/63639703/145214714-5af93bb5-393c-4c5a-984b-2d76f7be4250.png)


#### 6. Karena kita memiliki 2 Web Server, Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate
- Pada Doriki, install BIND9 lalu setting domain DNS Server pada file `/etc/bind/named.conf` sesuai gambar di bawah

![Screenshot 2021-12-11 163817](https://user-images.githubusercontent.com/73422724/145672241-947ec2c4-6adc-4bb0-ac3f-f1bf68f328bf.png)

- Pada Doriki, buat folder melalui perintah : `mkdir /etc/bind/jarkom `
- Pada Doriki, copy file pada `/etc/bind/db.local` melalui perintah `cp /etc/bind/db.local /etc/bind/jarkom/jarkomC12.com `
- Pada Doriki, edit file `/etc/bind/jarkom/jarkomC12.com ` sesuai gambar di bawah, untuk IP yang diatur menggunakan IP acak dalam hal ini 10.20.8.1

![Screenshot 2021-12-11 164102](https://user-images.githubusercontent.com/73422724/145672597-77a65db5-6f14-4ac5-8c5d-74eb8b071da7.png)

- Pada Guanhao, jalankan perintah berikut :
```
iptables -A PREROUTING -t nat -p tcp -d 10.20.8.1 --dport 80 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 10.20.0.26:80
iptables -A PREROUTING -t nat -p tcp -d 10.20.8.1 --dport 80 -j DNAT --to-destination 10.20.0.27:80
iptables -t nat -A POSTROUTING -p tcp -d 10.20.0.26 --dport 80 -j SNAT --to-source 10.20.8.1:80
iptables -t nat -A POSTROUTING -p tcp -d 10.20.0.27 --dport 80 -j SNAT --to-source 10.20.8.1:80
```
##### Testing
- Pada Guanhao, Jorge, Maingate Elena dan fukurou, install netcat melalui perintah `apt-get install netcat `
- Pada Jorge dan Maingate, Jalankan `nc -l -p 80 `
- Pada Client Elena dan Fukurou, Jalankan perintah `nc 10.20.8.1 80 ` lalu ketik kata-kata terserah, kata-kata tersebut akan muncul bergantian misal pertama kali di Jorge kemudian apabila di-run lagi `nc 10.20.8.1 80 ` lepas itu ketik kata-kata, maka berikutnya muncul di Maingate
- Elena

![Screenshot 2021-12-11 164440](https://user-images.githubusercontent.com/73422724/145672966-e76dc46b-ba1e-4f19-876b-edb308900bd9.png)

- Jorge

![Screenshot 2021-12-11 164542](https://user-images.githubusercontent.com/73422724/145672983-7211740a-5b19-4f19-826d-b83cce5229cc.png)

- Maingate

![Screenshot 2021-12-11 164519](https://user-images.githubusercontent.com/73422724/145672976-9194a55b-c90b-4c82-8199-8a1f21b87fd7.png)

#### Luffy berterima kasih pada kalian karena telah membantunya. Luffy juga mengingatkan agar semua aturan iptables harus disimpan pada sistem atau paling tidak kalian menyediakan script sebagai backup.

Kendala :
- Pada nomor 2, Dilakukan percobaan beberapa kali hingga status menjadi filtered yang kemudian status berubah open dan begitu seterusnya, dengan kata lain, kurang paham kapan berubah dan kenapa tidak stabil statusnya
