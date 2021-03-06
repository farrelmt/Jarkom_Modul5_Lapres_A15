# Jarkom_Modul5_Lapres_A15

SETTING TOPOLOGI

A. Membuat Topologi Jaringan
  ```
  # Switch
  uml_switch -unix switch0 > /dev/null < /dev/null & 
  uml_switch -unix switch1 > /dev/null < /dev/null & 
  uml_switch -unix switch2 > /dev/null < /dev/null & 
  uml_switch -unix switch3 > /dev/null < /dev/null & 
  uml_switch -unix switch4 > /dev/null < /dev/null & 
  uml_switch -unix switch5 > /dev/null < /dev/null & 

  # Router
  xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA eth0=tuntap,,,10.151.72.65 eth1=daemon,,,switch0 eth2=daemon,,,switch3 mem=96M &
  xterm -T BATU -e linux ubd0=BATU,jarkom umid=BATU eth0=daemon,,,switch3 eth1=daemon,,,switch2 eth2=daemon,,,switch5 mem=96M &
  xterm -T KEDIRI -e linux ubd0=KEDIRI,jarkom umid=KEDIRI eth0=daemon,,,switch0 eth1=daemon,,,switch1 eth2=daemon,,,switch4 mem=96M &

  # Server
  xterm -T PROBOLINGGO -e linux ubd0=PROBOLINGGO,jarkom umid=PROBOLINGGO eth0=daemon,,,switch1 mem=128M &
  xterm -T MADIUN -e linux ubd0=MADIUN,jarkom umid=MADIUN eth0=daemon,,,switch1 mem=128M &
  xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch2 mem=128M &
  xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch2 mem=128M &

  # Klien 1
  xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch4 mem=96M &
  xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch5 mem=96M &
  ```
  
B. Membuat Subnet Dengan Teknik VLSM
  - Menggambar subnet
  
  ![fotooooo](https://github.com/farrelmt/Jarkom_Modul5_Lapres_A15/tree/main/screenshot/1.png)
  
  - Menentukan ip dengan pohon
  
  ![fotooooo](https://github.com/farrelmt/Jarkom_Modul5_Lapres_A15/blob/main/screenshot/2.png)
  https://github.com/farrelmt/Jarkom_Modul5_Lapres_A15/tree/main/screenshot
  
  - Menentukan ip
  
  ![fotooooo](https://github.com/farrelmt/Jarkom_Modul5_Lapres_A15/blob/main/screenshot/3.png)
  
  - Setting interfaces di tiap uml
  
  ```
  SURABAYA
  auto lo
  iface lo inet loopback

  auto eth0 
  iface eth0 inet static
  address 10.151.72.66
  netmask 255.255.255.252
  gateway 10.151.72.65


  auto eth1
  iface eth1 inet static
  address 192.168.0.5
  netmask 255.255.255.252


  auto eth2
  iface eth2 inet static
  address 192.168.0.1
  netmask 255.255.255.252
  ```
  
  
  ```
  BATU
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet static
  address 192.168.0.2
  netmask 255.255.255.252
  gateway 192.168.0.1

  auto eth1
  iface eth1 inet static
  address 10.151.73.129
  netmask 255.255.255.248

  auto eth2
  iface eth2 inet static
  address 192.168.1.1
  netmask 255.255.252.0
  ```
  
  
  ```
  KEDIRI
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet static
  address 192.168.0.6
  netmask 255.255.255.252
  gateway 192.168.0.5

  auto eth1
  iface eth1 inet static
  address 192.168.0.9
  netmask 255.255.255.248

  auto eth2
  iface eth2 inet static
  address 192.168.2.1
  netmask 255.255.255.0
  ```
  
  
  ```
  PROBOLINGGO
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet static
  address 192.168.0.10
  netmask 255.255.255.248
  gateway 192.168.0.9
  ```
  
  
  ```
  MADIUN
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet static
  address 192.168.0.11
  netmask 255.255.255.248
  gateway 192.168.0.9
  ```
  
  
  ```
  MALANG
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet static
  address 10.151.73.130
  netmask 255.255.255.248
  gateway 10.151.73.129
  ```
  
  ```
  MOJOKERTO
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet static
  address 10.151.73.131
  netmask 255.255.255.248
  gateway 10.151.73.129
  ```
  
  ```
  GRESIK
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet static
  address 192.168.2.2
  netmask 255.255.255.0
  gateway 192.168.2.1
  ```
  
  ```
  SIDOARJO
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet static
  address 192.168.1.2
  netmask 255.255.255.0
  gateway 192.168.1.1
  ```

C. Melakukan Routing Pada Jaringan
   - Membuat bash file untuk dijalankan di surabaya
  ```
  ip route add 10.151.73.128/29 via 192.168.0.1         #VIA BATU
  ip route add 192.168.1.0/24 via 192.168.0.1            #VIA BATU
  ip route add 192.168.2.0/24 via 192.168.0.5            #VIA KEDIRI
  ip route add 192.168.0.8/29 via 192.168.0.5            #VIA KEDIRI
  ```
  - melakukan ```bash route.sh```

D. Membuat Subnet SIDOARJO dan GRESIK secara dinamis 

  - Setting Server Mojokerto Sebagai DHCP Server
    
    1. Install DHCP Server pada MOJOKERTO dengan  ```apt-get install isc-dhcp-server```
    
    2. Set Interfaces = "eth0" pada ```nano /etc/default/isc-dhcp-server``` 
    
    3. Set pada dhcp-server ```nano /etc/dhcp/dhcpd.conf``` sesuai gambar berikut :
    
    ![fotooooo](https://github.com/farrelmt/Jarkom_Modul5_Lapres_A15/blob/main/screenshot/5.png)
    
  
  - Setting DHCP Relay
  
    1. Install DHCP Server pada BATU, SURABAYA, dan KEDIRI dengan ```apt-get install isc-dhcp-relay```
    
    2. Pada ```nano /etc/default/isc-dhcp-relay ``` untuk SERVERS set dengan SERVERS="10.151.73.131"
    
    3. Lalu untuk INTERFACES set dengan INTERFACES
      
      ![fotooooo](https://github.com/farrelmt/Jarkom_Modul5_Lapres_A15/blob/main/screenshot/6.png)
      
      ![fotooooo](https://github.com/farrelmt/Jarkom_Modul5_Lapres_A15/blob/main/screenshot/7.png)
      
      ![fotooooo](https://github.com/farrelmt/Jarkom_Modul5_Lapres_A15/blob/main/screenshot/8.png)
  
  
  - Setting DHCP Client
  
    1. Pada SIDOARJO dan GRESIk atur interfacesnya dengan ```nano /etc/network/interfaces``` seperti berikut :
      
       ![fotooooo](https://github.com/farrelmt/Jarkom_Modul5_Lapres_A15/blob/main/screenshot/9.png)
       
       ![fotooooo](https://github.com/farrelmt/Jarkom_Modul5_Lapres_A15/blob/main/screenshot/10.png)
    
    2. Setelah diganti restart network dengan ```service networking restart```
    


FIREWALL

1. Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi
SURABAYA menggunakan iptables, namun Bibah tidak ingin kalian menggunakan
MASQUERADE.

  ```iptables -t nat -A POSTROUTING -s 192.168.0.0/16 -o eth0 -j SNAT --to-source 10.151.72.66```
  
  ![fotooooo](https://github.com/farrelmt/Jarkom_Modul5_Lapres_A15/blob/main/screenshot/11.png)
  
  ![fotooooo](https://github.com/farrelmt/Jarkom_Modul5_Lapres_A15/blob/main/screenshot/12.png)

