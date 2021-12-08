# Jarkom-Modul-5-C12-2021

## C12

- Daffa Amanullah Setyawan - 05111940000071<br>
- Satrio Hanif Wicaksono - 05111940000103<br>
- Shidqi Dhaifullah - 05111940000108<br>

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

