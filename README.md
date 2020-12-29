# Lapres Praktikum Modul 5

## Soal A

(A) Tugas pertama kalian yaitu membuat topologi jaringan sesuai dengan rancangan yang diberikan Bibah seperti dibawah ini :


- SURABAYA diberikan IP TUNTAP

- MALANG merupakan DNS Server diberikan IP DMZ

- MOJOKERTO merupakan DHCP Server diberikan IP DMZ

- MADIUN dan PROBOLINGGO merupakan WEB Server

- Setiap Server diberikan memory sebesar 128M

- Client dan Router diberikan memori sebesar 96M

- Jumlah host pada subnet SIDOARJO 200 Host

- Jumlah host pada subnet GRESIK 210 Host


**Jawaban**


menyusun konfigurasi pada file `topologi.sh` seperti berikut.

### Switch
```
uml_switch -unix switch1 > /dev/null < /dev/null &
uml_switch -unix switch2 > /dev/null < /dev/null &
uml_switch -unix switch3 > /dev/null < /dev/null &
uml_switch -unix switch4 > /dev/null < /dev/null &
uml_switch -unix switch5 > /dev/null < /dev/null &
uml_switch -unix switch6 > /dev/null < /dev/null &
```

### Router
```
xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA eth0=tuntap,,,10.151.72.37 eth1=daemon,,,switch3 eth2=daemon,,,switch4 mem=96M &
xterm -T BATU -e linux ubd0=BATU,jarkom umid=BATU eth0=daemon,,,switch3 eth1=daemon,,,switch2 eth2=daemon,,,switch1 mem=96M &
xterm -T KEDIRI -e linux ubd0=KEDIRI,jarkom umid=KEDIRI eth0=daemon,,,switch4 eth1=daemon,,,switch5 eth2=daemon,,,switch6 mem=96M &
```

### Server
```
xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch1 mem=128M &
xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch1 mem=128M &
xterm -T PROBOLINGGO -e linux ubd0=PROBOLINGGO,jarkom umid=PROBOLINGGO eth0=daemon,,,switch6 mem=128M &
xterm -T MADIUN -e linux ubd0=MADIUN,jarkom umid=MADIUN eth0=daemon,,,switch6 mem=128M &
```

### Client
```
xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch2 mem=96M &
xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch5 mem=96M &
```


![A](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/A.png)


## Soal B
(B) karena kalian telah mempelajari Subnetting dan Routing, Bibah meminta kalian untuk membuat topologi tersebut menggunakan teknik CIDR atau VLSM. Setelah melakukan subnetting, 


**Jawaban**


topologi sistem diatur dengan menggunakan **VLSM** seperti pada gambar berikut. subnet yang dipakai adalah yang dilingkari warna *pink*.

![B](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/B1.png)

dengan pembagian IP tiap subnet seperti pada gambar berikut.

![B](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/B2.png)

berikut tabel untuk pembagian IP sistem.

| UML      | ETH  | DEST        | SWITCH  | SUBNET | NID          | IP           | NETMASK | NETMASK         | GATEWAY      |
|----------|------|-------------|---------|--------|--------------|--------------|---------|-----------------|--------------|
| SURABAYA | eth0 | cloud       |         |        | 10.151.72.36 | 10.151.72.38 |         |                 | 10.151.72.37 |
|          | eth1 | batu        | switch3 | A2     | 192.168.0.0  | 192.168.0.1  | 30      | 255.255.255.252 |              |
|          | eth2 | kediri      | switch4 | A3     | 192.168.0.4  | 192.168.0.5  | 30      | 255.255.255.252 |              |
| BATU     | eth0 | surabaya    | switch3 | A2     | 192.168.0.0  | 192.168.0.2  | 30      | 255.255.255.252 | 192.168.0.1  |
|          | eth1 | sidoarjo    | switch2 | A1     | 192.168.1.0  | 192.168.1.1  | 24      | 255.255.255.0   |              |
|          | eth2 | malang/mojo | switch1 |        | 10.151.73.72 | 10.151.73.73 | 29      | 255.255.255.248 |              |
| KEDIRI   | eth0 | surabaya    | switch4 | A3     | 192.168.0.4  | 192.168.0.6  | 30      | 255.255.255.252 | 192.168.0.5  |
|          | eth1 | gresik      | switch5 | A4     | 192.168.2.0  | 192.168.2.1  | 24      | 255.255.255.0   |              |
|          | eth2 | madiun/prob | switch6 | A5     | 192.168.0.8  | 192.168.0.9  | 29      | 255.255.255.248 |              |
| SIDOARJO | eth0 | batu        | switch2 | A1     | 192.168.1.0  | 192.168.1.2  | 24      | 255.255.255.0   | 192.168.1.1  |
| GRESIK   | eth0 | kediri      | switch5 | A4     | 192.168.2.0  | 192.168.2.2  | 24      | 255.255.255.0   | 192.168.2.1  |
| MALANG   | eth0 | batu        | switch1 |        | 10.151.73.72 | 10.151.73.74 | 29      | 255.255.255.248 | 10.151.73.73 |
| MOJO     | eth0 | batu        | switch1 |        | 10.151.73.72 | 10.151.73.75 | 29      | 255.255.255.248 | 10.151.73.73 |
| PROB     | eth0 | kediri      | switch6 | A5     | 192.168.0.8  | 192.168.0.10 | 29      | 255.255.255.248 | 192.168.0.9  |
| MADIUN   | eth0 | kediri      | switch6 | A5     | 192.168.0.8  | 192.168.0.11 | 29      | 255.255.255.248 | 192.168.0.9  |

selanjutnya mengatur `/etc/network/interfaces` pada tiap uml dengan keterangan sebagai berikut.

### ROUTER
#### Surabaya
```
auto lo
iface lo inet loopback

#cloud
auto eth0
iface eth0 inet static
address 10.151.72.38
netmask 255.255.255.252
gateway 10.151.72.37

#batu
auto eth1
iface eth1 inet static
address 192.168.0.1
netmask 255.255.255.252

#kediri
auto eth2
iface eth2 inet static
address 192.168.0.5
netmask 255.255.255.252
```

#### Batu
```
auto lo
iface lo inet loopback

#sby
auto eth0
iface eth0 inet static
address 192.168.0.2
netmask 255.255.255.252
gateway 192.168.0.1

#sidaorjo
auto eth1
iface eth1 inet static
address 192.168.1.1
netmask 255.255.255.0

#malang/mojo
auto eth2
iface eth2 inet static
address 10.151.73.73
netmask 255.255.255.248
```

#### Kediri
```
auto lo
iface lo inet loopback

#sby
auto eth0
iface eth0 inet static
address 192.168.0.6
netmask 255.255.255.252
gateway 192.168.0.5

#gresik
auto eth1
iface eth1 inet static
address 192.168.2.1
netmask 255.255.255.0

#madiun/prob
auto eth2
iface eth2 inet static
address 192.168.0.9
netmask 255.255.255.248
```

### SERVER
#### Malang
```
auto lo
iface lo inet loopback

#batu
auto eth0
iface eth0 inet static
address 10.151.73.74
netmask 255.255.255.248
gateway 10.151.73.73
```

#### Mojokerto
```
auto lo
iface lo inet loopback

#batu
auto eth0
iface eth0 inet static
address 10.151.73.75
netmask 255.255.255.248
gateway 10.151.73.73
```

#### Probolinggo
```
auto lo
iface lo inet loopback

#kediri
auto eth0
iface eth0 inet static
address 192.168.0.10
netmask 255.255.255.248
gateway 192.168.0.9
```

#### Madiun
```
auto lo
iface lo inet loopback

#kediri
auto eth0
iface eth0 inet static
address 192.168.0.11
netmask 255.255.255.248
gateway 192.168.0.9
```

### CLIENT
#### Sidoarjo
```
auto lo
iface lo inet loopback

#batu
auto eth0
iface eth0 inet static
address 192.168.1.2
netmask 255.255.255.0
gateway 192.168.1.1
```

#### Gresik
```
auto lo
iface lo inet loopback

#kediri
auto eth0
iface eth0 inet static
address 192.168.2.2
netmask 255.255.255.0
gateway 192.168.2.1
```

## Soal C
(C) kalian juga diharuskan melakukan routing agar setiap perangkat pada jaringan tersebut dapat terhubung.


**Jawaban**


berikut adalah tabel routing yang digunakan.

| ROUTER   | SUBNET      | NID          | NETMASK         | GATEWAY     |
|----------|-------------|--------------|-----------------|-------------|
| SURABAYA | malang/mojo | 10.151.73.72 | 255.255.255.248 | 192.168.0.2 |
|          | A1          | 192.168.1.0  | 255.255.255.0   | 192.168.0.2 |
|          | A4          | 192.168.2.0  | 255.255.255.0   | 192.168.0.6 |
|          | A5          | 192.168.0.8  | 255.255.255.248 | 192.168.0.6 |
| BATU     | A3          | 192.168.0.4  | 255.255.255.252 | 192.168.0.1 |
|          | A4          | 192.168.2.0  | 255.255.255.0   | 192.168.0.1 |
|          | A5          | 192.168.0.8  | 255.255.255.248 | 192.168.0.1 |
|          | static      | 0.0.0.0      | 0.0.0.0         | 192.168.0.1 |
| KEDIRI   | malang/mojo | 10.151.73.72 | 255.255.255.248 | 192.168.0.5 |
|          | A1          | 192.168.1.0  | 255.255.255.0   | 192.168.0.5 |
|          | A2          | 192.168.0.0  | 255.255.255.252 | 192.168.0.5 |
|          | static      | 0.0.0.0      | 0.0.0.0         | 192.168.0.5 |

routing dilakukan dengan membuat file `route.sh` pada UML dengan isi sebagai berikut.

### Surabaya
```
route add -net 10.151.73.72 netmask 255.255.255.248 gw 192.168.0.2
route add -net 192.168.1.0 netmask 255.255.255.0 gw 192.168.0.2
route add -net 192.168.2.0 netmask 255.255.255.0 gw 192.168.0.6
route add -net 192.168.0.8 netmask 255.255.255.248 gw 192.168.0.6
```


![C](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/C1.png)


### Batu
```
route add -net 192.168.0.4 netmask 255.255.255.252 gw 192.168.0.1
route add -net 192.168.2.0 netmask 255.255.255.0 gw 192.168.0.1
route add -net 192.168.0.8 netmask 255.255.255.248 gw 192.168.0.1
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.168.0.1
```


![C](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/C2.png)


### Kediri
```
route add -net 10.151.73.72 netmask 255.255.255.248 gw 192.168.0.5
route add -net 192.168.1.0 netmask 255.255.255.0 gw 192.168.0.5
route add -net 192.168.0.0 netmask 255.255.255.252 gw 192.168.0.5
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.168.0.5
```

![C](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/C3.png)


untuk menjalanakan routing, digunakan perintah `source route.sh`.

## Soal D
(D) Tugas berikutnya adalah memberikan ip pada subnet SIDOARJO dan GRESIK secara dinamis menggunakan bantuan DHCP SERVER (Selain subnet tersebut menggunakan ip static). Kemudian kalian mengingat bahwa kalian harus setting DHCP RELAY pada router yang menghubungkannya, seperti yang kalian telah pelajari di masa lalu.


**Jawaban**


### DHCP Server
- yaitu pada UML Mojokerto. 
- menginstall **isc-dhcp-server** dengan perintah `apt-get install isc-dhcp-server`.
- konfigurasi interface pada `/etc/default/isc-dhcp-server` dengan konfigurasi `INTERFACES="eth0"`
- diberikan tambahan konfigurasi pada `/etc/dhcp/dhcpd.conf ` seperti berikut.
```
subnet 10.151.73.72 netmask 255.255.255.248 {
	option routers 10.151.73.73;
	option broadcast-address 10.151.73.79;
}

#sidoarjo 200 host
subnet 192.168.1.0 netmask 255.255.255.0 {
	range 192.168.1.2 192.168.1.203;
	option routers 192.168.1.1;
	option broadcast-address 192.168.1.255;
	option domain-name-servers 10.151.73.74,202.46.129.2;
	default-lease-time 600;
	max-lease-time 7200;
}

#gresik 210 host
subnet 192.168.2.0 netmask 255.255.255.0 {
	range 192.168.2.2 192.168.2.213;
	option routers 192.168.2.1;
	option broadcast-address 192.168.2.255;
	option domain-name-servers 10.151.73.74,202.46.129.2;
	default-lease-time 600;
	max-lease-time 7200;
}
```

![D](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/D3.png)

![D](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/D4.png)

![D](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/D5.png)


### DHCP Relay
- yaitu pada UML Surabaya, Batu, dan Kediri.
- menginstall **isc-dhcp-relay** dengan perintah `apt-get install isc-dhcp-relay`.
- konfigurasi interface pada `/etc/default/isc-dhcp-relay` dengan konfigurasi `SERVERS="10.151.73.75"`, di mana *10.151.73.75* merupakan IP Mojokerto.
- dan juga mengatur interfaces seperti berikut.
  - Surabaya : `INTERFACESv4="eth1 eth2"`
    
    ![D](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/D7.png)
    
  - Batu : `INTERFACESv4="eth0 eth1 eth2"`
  
    ![D](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/D8.png)
    
  - Kediri : `INTERFACESv4="eth0 eth1 eth2"`
  
    ![D](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/D6.png)

### DHCP Client
yaitu pada UML Gresik dan Sidoarjo diatur agar IPnya dinamis.

![D](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/D2.png)

![D](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/D1.png)

---

## No. 1

(1) Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi SURABAYA menggunakan iptables, namun Bibah tidak ingin kalian menggunakan MASQUERADE.


**Jawaban**


mengatur konfigurasi pada `iptables` di Surabaya seperti berikut.
```
iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to 10.151.72.38
```
- *10.151.72.38* merupakan IP eth0 Surabaya.
- perintah dijalankan dengan `bash iptables.sh` di Surabaya. 
- kemudian, untuk mengecek apakah perintah berhasil, dilakukan ping ke `its.ac.id` di semua UML seperti berikut.

![1](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/1.1.png)

![1](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/1.2.png)

![1](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/1.3.png)

![1](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/1.4.png)

![1](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/1.5.png)

![1](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/1.6.png)

![1](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/1.7.png)


## No. 2

(2) Kalian diminta untuk mendrop semua akses SSH dari luar Topologi (UML) Kalian pada server yang memiliki ip DMZ (DHCP dan DNS SERVER) pada SURABAYA demi menjaga keamanan.

**Jawaban**


pada `iptables` di Surabaya, ditambah konfigurasi seperti berikut.
```
iptables -A FORWARD -p tcp --dport 22 -d 10.151.73.72/29 -i eth0 -j DROP
```
- *10.151.73.72/29* merupakan IP DMZ (DHCP dan DNS Server). 
- perintah dijalankan dengan `bash iptables.sh` di Surabaya. 
- kemudian, untuk mengecek apakah perintah berhasil, di Malang diberi perintah `nc -l -p 22`
- pada PuTTy ARJUNA, diberi perintah `nc 10.151.73.74 22`, di mana *10.151.73.74* merupakan IP Malang

hasil dapat dilihat pada foto berikut.


![2](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/2.1.png)


![2](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/2.2.png)

## No. 3
(3) Karena tim kalian maksimal terdiri dari 3 orang, Bibah meminta kalian untuk membatasi DHCP dan DNS server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan yang berasal dari mana saja menggunakan iptables pada masing masing server, selebihnya akan di DROP. 


**Jawaban**


pada `iptables` di Malang dan Mojokerto, diberi konfigurasi seperti berikut.
```
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```
- `connlimit` digunakan untuk membatasi koneksi. dalam kasus ini, koneksi paling banyak yang diperbolehkan adalah 3 koneksi. selain itu, maka koneksi akan di-DROP.
- perintah dijalankan dengan `bash iptables.sh` di Malang dan Mojokerto. 
- untuk mengecek apakah perintah berhasil, dilakukan ping ke Malang dan Mojokerto pada 4 UML yang berbeda.

### Untuk Malang
pengecekan dengan perintah `ping 10.151.73.74` pada 4 UML. pada UML keempat, tidak mendapat respon.

![4](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/3.png)

### Untuk Mojokerto
pengecekan dengan perintah `ping 10.151.73.75` pada 4 UML. pada UML keempat, tidak mendapat respon.

![4](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/3.1.png)


## No. 4

(4) Akses dari subnet SIDOARJO hanya diperbolehkan pada pukul 07.00 - 17.00 pada hari Senin sampai Jumat.

Selain itu paket akan di REJECT.

**Jawaban**


pada `iptables` di Malang, diberi konfigurasi seperti berikut.
```
iptables -A INPUT -s 192.168.1.2/24 -m time --timestart 00:00 --timestop 06:59 --weekdays Mon,Tue,Wed,Thu,Fri -j REJECT
iptables -A INPUT -s 192.168.1.2/24 -m time --timestart 17:01 --timestop 23:59 --weekdays Mon,Tue,Wed,Thu,Fri -j REJECT
iptables -A INPUT -s 192.168.1.2/24 -m time --weekdays Sat,Sun -j REJECT
```
- line 1 dan 2 menunjukkan penolakan pada hari Senin, Selasa, Rabu, kamis, Jumat untuk waktu selain 07.00 - 17.00.
- line 3 menunjukkan penolakan untuk hari Sabtu dan Minggu.
- untuk mengecek apakah perintah berhasil, diatur waktu pada UML Malang dan dilakukan ping dari Sidoarjo.

### Waktu boleh
- set waktu di UML Malang menjadi `Senin, 20 Januari 2020, 09:00:00`
- ping Malang dari Sidoarjo dengan perintah `ping 10.151.73.74`

![4](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/4.3.png)

![4](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/4.4.png)

### Waktu tidak boleh
- set waktu di UML Malang menjadi `Minggu, 19 Januari 2020, 12:00:00`
- ping Malang dari Sidoarjo dengan perintah `ping 10.151.73.74`

![4](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/4.png)

![4](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/4.2.png)


## No. 5

(5) Akses dari subnet GRESIK hanya diperbolehkan pada pukul 17.00 hingga pukul 07.00 setiap harinya. Selain itu paket akan di REJECT.


**Jawaban**


pada `iptables` di Malang, diberi konfigurasi seperti berikut.
```
iptables -A INPUT -s 192.168.2.2/24 -m time --timestart 07:01 --timestop 16:59 -j REJECT 
```
- menunjukkan bahwa paket akan di REJECT pada waktu selain pukul 17.00 - 07.00 tiap harinya.

### Waktu boleh
- set waktu di UML Malang menjadi `Minggu, 19 Januari 2020, 06:00:00`
- ping Malang dari Gresik dengan perintah `ping 10.151.73.74`

![5](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/5.3.png)

![5](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/5.4.png)

### Waktu tidak boleh
- set waktu di UML Malang menjadi `Minggu, 19 Januari 2020, 11:00:00`
- ping Malang dari Gresik dengan perintah `ping 10.151.73.74`

![5](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/5.1.png)

![5](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/5.2.png)

## No. 6

Karena kita memiliki 2 buah WEB Server, (6) Bibah ingin SURABAYA disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada PROBOLINGGO port 80 dan MADIUN port 80.


**Jawaban**


pada `iptables` di Surabaya, ditambah konfigurasi seperti berikut.
```
iptables -A PREROUTING -t nat -p tcp -d 192.168.0.15:80 --dport 80 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 192.168.0.10
iptables -A PREROUTING -t nat -p tcp -d 192.168.0.15:80 --dport 80 -j DNAT --to-destination 192.168.0.11

iptables -t nat -A POSTROUTING -p tcp --dport 80 -d 192.168.0.10 -j SNAT --to-source 192.168.0.15:80
iptables -t nat -A POSTROUTING -p tcp --dport 80 -d 192.168.0.11 -j SNAT --to-source 192.168.0.15:80
```
- `PREROUTING` digunakan untuk mengubah alamat tujuan sebelum routing.
- `POSTROUTING` digunakan untuk mengubah alamat tujuan setelah routing.


![6](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/6.1.png)


![6](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/6.2.png)

## No. 7
(7) Bibah ingin agar semua paket didrop oleh firewall (dalam topologi) tercatat dalam log pada setiap UML yang memiliki aturan drop. Bibah berterima kasih kepada kalian karena telah mau membantunya. Bibah juga mengingatkan agar semua aturan iptables harus disimpan pada sistem atau paling tidak kalian menyediakan script sebagai backup.

**Jawaban**


pada `iptables` di Surabaya, ditambah konfigurasi seperti berikut.
```
iptables -N LOGGING
iptables -A FORWARD -p tcp --dport 22 -d 10.151.73.72/29 -i eth0 -j LOGGING
iptables -A LOGGING -j LOG --log-prefix "IPTables-Dropped: " --log-level 4
iptables -A LOGGING -j DROP
```
pada `iptables` di Malang dan Mojokerto, ditambah konfigurasi seperti berikut.
```
iptables -N LOGGING
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j LOGGING
iptables -A LOGGING -j LOG --log-prefix "IPTables-Dropped: " --log-level 4
iptables -A LOGGING -j DROP
```


![7](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/7.1.png)

![7](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/7.2.png)

![7](https://github.com/wardahnab/Jarkom_Modul5_Lapres_A08/blob/main/Gambar/7.3.png)
