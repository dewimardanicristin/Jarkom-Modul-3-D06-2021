# Jarkom-Modul-3-D06-2021

Anggota :
- Fitrah Mutiara 05111940000126
- Ahdan Amanullah Irfan Muzhaffar 05111940000197
- Dewi Mardani 05111940000225

## Soal 1
Luffy bersama Zoro berencana membuat peta tersebut dengan kriteria EniesLobby sebagai DNS Server, Jipangu sebagai DHCP Server, Water7 sebagai Proxy Server
- Pada EniesLobby, lakukan `apt-get update` kemudian `apt-get install bind9`
- Pada Jipangu, laukan `apt-get update` kemudian `apt-get install isc-dhcp-server`
- Pada Water7, lakukan `apt-get update` kemudian `apt-get install squid`

## Soal 2
Foosha sebagai DHCP Relay
- Pada Foosha, lakukan `apt-get update` kemudian `apt-get install isc-dhcp-relay`
- Edit file `/etc/default/isc-dhcp-relay` seperti berikut

## Soal 3
Client yang melalui Switch1 mendapatkan range IP dari `[prefix IP].1.20 - [prefix IP].1.99` dan `[prefix IP].1.150 - [prefix IP].1.169`
### Pada Jipangu
- Edit file `/etc/dhcp/dhcpd.conf`
- Tambahkan 
```
2-4, 6. subnet 10.24.2.0 netmask 255.255.255.0{
}

subnet 10.24.1.0 netmask 255.255.255.0{
    range 10.24.1.20 10.24.1.99;
    range 10.24.1.150 10.24.1.169;
    option routers 10.24.1.1;
    option broadcast-address 10.24.1.255;
    option domain-name-servers 10.24.2.2;
    default-lease-time 600;
    max-lease-time 7200;
}
```

## Soal 4
### Pada Jipangu
- Edit file `/etc/dhcp/dhcpd.conf`
- Tambahkan 
```
subnet 10.24.3.0 netmask 255.255.255.0{
    range 10.24.3.30 10.24.3.50;
    option routers 10.24.3.1;
    option broadcast-address 10.24.3.255;
    option domain-name-servers 10.24.2.2;
    default-lease-time 600;
    max-lease-time 7200;
}

```
- kemudian restart dengan melakukan `service isc-dhcp-server restart`

## Soal 5
Client mendapatkan DNS dari EniesLobby dan client dapat terhubung dengan internet melalui DNS tersebut
### Pada Skypie 
- Edit file `/etc/network/interfaces` seperti berikut 
- ![image](https://user-images.githubusercontent.com/81247727/140744476-6cffaa56-6085-4afc-b4e6-baf1247219e5.png)
- Cek nameserver dengan `cat /etc/resolv.conf`
- ![image](https://user-images.githubusercontent.com/81247727/140744581-3505468c-99c9-4295-8941-b12f94387c27.png)
### Pada Tottoland 
- Edit file `/etc/network/interfaces` seperti berikut 
- ![image](https://user-images.githubusercontent.com/81247727/140744766-3d38431e-c8b3-4f15-87d4-911d70a29372.png)
- Cek nameserver dengan `cat /etc/resolv.conf`
- ![image](https://user-images.githubusercontent.com/81247727/140744802-3d680f47-6ed7-45e2-a3f4-e3fc438a771c.png)
### Pada Loguetown 
- Edit file `/etc/network/interfaces` seperti berikut 
- ![image](https://user-images.githubusercontent.com/81247727/140744870-6f033acf-d311-469b-81d2-6f74bcb6e75a.png)
- Cek nameserver dengan `cat /etc/resolv.conf`
- ![image](https://user-images.githubusercontent.com/81247727/140744895-4bc9c62a-3ce3-4b94-b698-2bc07e309b98.png)
### Pada Alabasta 
- Edit file `/etc/network/interfaces` seperti berikut 
- ![image](https://user-images.githubusercontent.com/81247727/140744954-64585ee7-57a0-400a-aaa5-aff25835d52c.png)
- Cek nameserver dengan `cat /etc/resolv.conf`
- ![image](https://user-images.githubusercontent.com/81247727/140744980-b49df64c-b2ee-458f-bc56-4dedbdc9f7ab.png)

## Soal 6
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 6 menit sedangkan pada client yang melalui Switch3 selama 12 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 120 menit.
### Pada Jipangu
- Edit file konfigurasi isc-dhcp-server pada `/etc/dhcp/dhcpd.conf`
- Ganti `default-lease-time 600;` menjadi `default-lease-time 360;` pada switch1 atau subnet `10.24.1.0`
- Ganti `default-lease-time 600;` menjadi `default-lease-time 720;` pada switch1 atau subnet `10.24.3.0`

## Soal 7
Luffy dan Zoro berencana menjadikan Skypie sebagai server untuk jual beli kapal yang dimilikinya dengan alamat IP yang tetap dengan IP [prefix IP].3.69
### Pada Jipangu
- Edit file `/etc/dhcp/dhcpd.conf` seperti berikut
- ![image](https://user-images.githubusercontent.com/81247727/140745776-1a9446cd-c5e3-4d50-a460-c6713601a0d1.png)
- hardware ethernet didapat dari pengecekan `link/ether` `eth0` di Skypie dengan menjalankan perintah `ip a`
- kemudian restart dengan melakukan `service isc-dhcp-server restart`
### Pada Skypie
- Edit file `/etc/network/interfaces` seperti berikut
- ![image](https://user-images.githubusercontent.com/81247727/140746421-c4b22831-ccd4-46d6-bbd0-1c6700e01451.png)
- restart node dengan klik kanan pada router, kemudian `stop` lalu `start` kembali
- Jika dicek dengan `ip a` maka dapat dilihat bahwa IP Skypie telah berubah
- ![image](https://user-images.githubusercontent.com/81247727/140746629-40f16fd1-80b4-4f68-a18f-0ca3f67a5473.png)

## Soal 8-10
- Pada Loguetown, proxy harus bisa diakses dengan nama jualbelikapal.yyy.com dengan port yang digunakan adalah 5000
- Agar transaksi jual beli lebih aman dan pengguna website ada dua orang, proxy dipasang autentikasi user proxy dengan enkripsi MD5 dengan dua username, yaitu luffybelikapalyyy dengan password luffy_yyy dan zorobelikapalyyy dengan password zoro_yyy 
- Transaksi jual beli tidak dilakukan setiap hari, oleh karena itu akses internet dibatasi hanya dapat diakses setiap hari Senin-Kamis pukul 07.00-11.00 dan setiap hari Selasa-Jum’at pukul 17.00-03.00 keesokan harinya (sampai Sabtu pukul 03.00)

Atur pengaturan pada `/etc/squid/squid.conf` seperti berikut:
```
include /etc/squid/acl.conf
http_port 5000
visible_hostname jualbelikapal.d06.com

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on

acl USERS proxy_auth REQUIRED
http_access allow AVAILABLE_WORKING1 USERS
http_access allow AVAILABLE_WORKING2 USERS
http_access allow AVAILABLE_WORKING3 USERS
http_access deny all
```

Lalu pada `/etc/squid/acl.conf` kita isi:
```
acl AVAILABLE_WORKING time MTWH 07:00-11:00
acl AVAILABLE_WORKING time TWHF 17:00-24:00
acl AVAILABLE_WORKING time TWHF 00:00-03:00
```

Settingan diatas adalah untuk mengatur proxy server (no8)
pada pengaturan squid.conf adalah mengatur agar bisa login(no9) dan hanya bisa akses pada waktu tertentu lalu pada acl.conf adalah mengatur waktu tertentunya(no10).

Untuk menambahkan user dan pass bisa menggunakan command
```
htpasswd -b -c /etc/squid/passwd lufffybelikapald06 luffy_d06
htpasswd -b /etc/squid/passwd zorobelikapald06 zoro_d06
```
## Soal 11
### Pada Water7(Proxy Server)
Pertama sambungkan terlebih dahulu Water7 dengan Enieslobby (ubah nameserver):
![image](https://user-images.githubusercontent.com/58657135/140869173-b7bb51b5-e544-467a-a06f-77871e4a99cd.png)

### Pada Enieslobby(DNS Server)
atur dns pada EniesLobby agar mengarahkan domain `franky.d06.com` dan sub-domain `super.franky.d06.com` ke IP Skypie:
![image](https://user-images.githubusercontent.com/58657135/140869387-b0fb961a-3ac5-4fbc-b4da-21f6dca69aea.png)

dan atur reverse ptr nya:
![image](https://user-images.githubusercontent.com/58657135/140869472-9d907d8a-b1a3-42a8-ba66-b9775586df76.png)

dan atur `named.conf.local`:
![image](https://user-images.githubusercontent.com/58657135/140869534-854552b9-21ab-435f-ad3f-c7b2ee64eb7a.png)

### Pada Skypie(Web Server)
Install apache2 terlebih dahulu lalu extract zip `super.franky.d06.com`, membuat file conf pada folder sites-available, melakukan `a2ensite super.franky.d06.com` dan lakukan `service apache2 restart`.:
![image](https://user-images.githubusercontent.com/58657135/140912669-3a0ff23a-3187-4359-b58f-925695ef175b.png)

### Akses dengan client
Test dengan menyalakan proxy lalu mengakses 'google.com'

![image](https://user-images.githubusercontent.com/58657135/140913740-003e5e91-fe8f-4367-8d97-b8b57d67e140.png)

![image](https://user-images.githubusercontent.com/58657135/140913779-4de3e73a-e943-444d-9ef7-08eb098c6ca5.png)

![image](https://user-images.githubusercontent.com/58657135/140913804-f20ede10-c2c5-4051-aa23-291a39c0c302.png)

(jika tidak bisa test dulu apakah di proxy server bisa ngakses ke super.fraanky.d06.com pakai lynx)
## Soal 12
## Soal 13
