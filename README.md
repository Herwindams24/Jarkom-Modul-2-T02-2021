# Lapres Modul 2 Jarkom 2021 - T02
Laporan Hasil Praktikum Modul 1 Jarkom 2021
Kelompok T02
  * Herwinda Marwaa Salsabila (05311940000009)
  * Dian Arofati N. Z. (05311940000011)
  * Dava Aditya Jauhar (05311940000030)

---

## Persiapan

   **Soal**
    
   EniesLobby akan dijadikan sebagai DNS Master, Water7 akan dijadikan DNS Slave, dan Skypie akan digunakan sebagai Web Server. Terdapat 2 Client yaitu Loguetown, dan Alabasta

   **Jawaban**
   
   1. Dikarenakan soal mirip dengan latihan soal, buat topologi yang sama seperti latihan soal. Selanjutnya tambahkan node baru yaitu Skypie yang bertindak sebagai Web Server nantinya. 
   2. Pada node skypie, konfigurasikan:
   
      ```
         auto eth0
         iface eth0 inet static
         address 192.212.2.4
         netmask 255.255.255.0
         gateway 192.212.2.1
       ```

## Nomor 01
   **Soal**
   
   Semua node terhubung pada router Foosha, sehingga dapat mengakses internet 
   
   **Jawaban**
   
   **Pada Foosha**
   1. Buat file .bashrc dengan mengetikan `nano /root/.bashrc`
   2. Pada file tersebut Ketikkan `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.212.0.0/16`. Di mana `192.212` merupakan prefix kelompok penulis.
   3. Ketikan `cat /etc/resolv.conf` untuk menampilkan IP DNS
   4. Coba ping google.com
   
   **Pada Node Ubuntu Lain**
   1. Ketikkan ommand ini pada file script.sh di node ubuntu yang lain `echo nameserver 192.168.122.1 > /etc/resolv.conf`.
   2. Bash script.sh 
   3. Ping google.com. Jika Semua node sekarang sudah bisa melakukan ping ke google, yang artinya adalah sudah tersambung ke internet

 **Dokumentasi**
   1. Pada Foosha
   
   
   2. Pada Loguetown
   
   
   
   3. Pada Skypie
   
   
   
   4. Pada EnniesLobby
   
   
   
   5. Pada Water7


---

## Nomor 02
   **Soal**
   
   Luffy ingin menghubungi Franky yang berada di EniesLobby dengan denden mushi. Kalian diminta Luffy untuk membuat website utama dengan mengakses franky.yyy.com dengan alias www.franky.yyy.com pada folder kaizoku.
   
   **Jawaban**
   
   Soal no. 02 meminta kami untuk:
   1. Buat folder kaizoku
   2. Buat domain utama yang mengakses franky.ti2.com
   3. Buat alias domain utama dengan nama www.franky.ti2.com
 
   **EniesLobby**
  
   1. Update dan Cek di EniesLobby
    
      ```
       apt-get update
       apt-get install nano
       ```
    
   2. Install Bind9
    
      ```
      apt-get install bind9 -y
      ```
   
   3. Konfigurasi zone pada `/etc/bind/named.conf.local`
      
      ```
      zone "franky.ti2.com" {
	     type master;
	     file "/etc/bind/kaizoku/franky.ti2.com";
      };
      ```
   4. Buat direktori baru untuk zone tadi
      `mkdir /etc/bind/kaizoku`
   5. Copy dari db.local 
      ```
      /etc/bind/db.local /etc/bind/kaizoku/franky.ti2.com
      ```
   7. Konfigurasi franky.ti2.com pada `/etc/bind/kaizoku/franky.ti2.com`
      * Semua yang berbau localhost ganti dengan `franky.ti2.com`
      * Arahkan alamat IP ke `..` dengan ip `..` yang di sini sebagai master domain. 
      
      ```
      ```
      
   8. Restart Bind9 2x
      ```
      service bind9 restart
      ```
   
   **Pada Client**
   
   Lakukan perubahan pada script.sh Lougetown dan Alabasta berupa nameserver menuju IP Enieslobby 192.212.2.2 untuk dapat mengakses dari domain yang telah kita buat.

   ```
   echo nameserver 192.212.2.2 > /etc/resolv.conf
   ```
   
   **Dokumentasi**
   
---

## Nomor 03
   **Soal**
   
   Setelah itu buat subdomain super.franky.yyy.com dengan alias www.super.franky.yyy.com yang diatur DNS nya di EniesLobby dan mengarah ke Skypie
   
   **Jawaban**
   
   
   
   **Dokumentasi**
   
   
---

## Nomor 04
   **Soal**
   
   Buat juga reverse domain untuk domain utama
   
   **Jawaban**
   
   
   
   **Dokumentasi**



---

## Nomor 05
   **Soal**
   
   Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak, maka buat Water7 sebagai DNS Slave untuk domain utama
   
   **Jawaban**
   
   
   
   **Dokumentasi**
   
