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

## Soal 8
Pada Loguetown, proxy harus bisa diakses dengan nama jualbelikapal.yyy.com dengan port yang digunakan adalah 5000

## Soal 9
Agar transaksi jual beli lebih aman dan pengguna website ada dua orang, proxy dipasang autentikasi user proxy dengan enkripsi MD5 dengan dua username, yaitu luffybelikapalyyy dengan password luffy_yyy dan zorobelikapalyyy dengan password zoro_yyy 

## Soal 10
## Soal 11
## Soal 12
## Soal 13
