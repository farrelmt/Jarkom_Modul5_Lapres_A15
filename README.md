# Jarkom_Modul5_Lapres_A15

SETTING TOPOLOGI
A. Membuat Topologi Jaringan

B. Membuat Subnet Dengan Teknik CIDR

C. Melakukan Routing Pada Jaringan

D. Membuat Subnet SIDOARJO dan GRESIK secara dinamis 


FIREWALL
1. Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi
SURABAYA menggunakan iptables, namun Bibah tidak ingin kalian menggunakan
MASQUERADE.

  ```iptables -t nat -A POSTROUTING -s 192.168.0.0/16 -o eth0 -j SNAT --to-source 10.151.72.66```

