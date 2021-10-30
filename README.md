# Lapres Modul 2 Jarkom 2021 - T02
Laporan Hasil Praktikum Modul 2 Jarkom 2021
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
   
   <img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Foosha/bashrc.png?raw=true" width="500">

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

        <img src="https://user-images.githubusercontent.com/57980125/139372087-e79da388-2b06-4517-ad0a-706e40168c7e.png" width="500">

   2. Pada Loguetown
   
        <img src="https://user-images.githubusercontent.com/57980125/139371923-b752196e-8af0-4398-b1af-cf59126fec3e.png" width="500">
        <img src="https://user-images.githubusercontent.com/57980125/139371989-2c1cecfd-52e2-45c8-a374-5f177f19f40b.png" width="500">
   
   3. Pada Skypie
   
       <img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Skypie/no01_skypie.png?raw=true" width="500">
   
   4. Pada EnniesLobby
    
        <img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/EnniesLobby/no01_EniesLobby.png?raw=true" width="500">
   
   5. Pada Water7
   
        <img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Water7/no1_water7.png?raw=true" width="500">
   
   6. Pada Alabasta
        
        <img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Alabasta/no01_alabasta.png?raw=true" width="500">
       
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
      echo 'zone "franky.ti2.com"{
            type master;
            file "/etc/bind/kaizoku/franky.ti2.com";
        };' > /etc/bind/named.conf.local
      ```

   4. Buat direktori baru untuk zone tadi
   
      ```
      mkdir /etc/bind/kaizoku
      ```

   5. Copy dari db.local
    
      ```typescript
      /etc/bind/db.local /etc/bind/kaizoku/franky.ti2.com
      ```

   6. Konfigurasi franky.ti2.com pada `/etc/bind/kaizoku/franky.ti2.com`
      * Semua yang berbau localhost ganti dengan `franky.ti2.com`
      * Arahkan alamat IP ke `..` dengan ip `..` yang di sini sebagai master domain. 
      
      ```typescript
      echo "\
      \$TTL    604800
        @       IN      SOA     franky.ti2.com. root.franky.ti2.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
        ;
        @       IN      NS      franky.ti2.com.
        @       IN      A       192.212.2.2 
        " > /etc/bind/kaizoku/franky.ti2.com
      ```
      
   7. Restart Bind9 2x

        ```
        service bind9 restart
        service bind9 restart
        ```

   8. Buat alias ``www.franky.ti2.com`` menggunakan CNAME

        ```
        echo "\
        \$TTL    604800
        @       IN      SOA     franky.ti2.com. root.franky.ti2.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
        ;
        @       IN      NS      franky.ti2.com.
        @       IN      A       192.212.2.2 
        www     IN      CNAME   franky.ti2.com.
        " > /etc/bind/kaizoku/franky.ti2.com
        ```

   9.  Restart Bind9 2x

        ```
        service bind9 restart
        service bind9 restart
        ```
 
   **Pada Client**
   
   Lakukan perubahan pada script.sh Lougetown dan Alabasta berupa nameserver menuju IP Enieslobby 192.212.2.2 untuk dapat mengakses dari domain yang telah kita buat. Perubahan dilakukan dengan cara:
   
   1. Ketikan `nano root/script.sh`
   2. Di bawah  echo nameserver 192.168.122.1 > /etc/resolv.conf, ketikan:
   ```
   echo nameserver 192.212.2.2 > /etc/resolv.conf
   ```
   3. Lakukan bash pada script.sh
   4. ping franky.yyy.com beserta alias www.franky.yyy.com
   
   <img src="https://user-images.githubusercontent.com/57980125/139376191-8d77bcdb-40c8-4e70-8ab5-34b771d9978b.png" width="500">

---

## Nomor 03
   **Soal**
   
   Setelah itu buat subdomain super.franky.yyy.com dengan alias www.super.franky.yyy.com yang diatur DNS nya di EniesLobby dan mengarah ke Skypie
   
   **Jawaban**
   
   **Pada EnniesLobby**

   1. Buka `/etc/bind/kaizoku/franky.ti2.com` lalu edit sebagai berikut:
    
       ```typescript

       # Subdomain 1

            echo "\
            \$TTL    604800
            @       IN      SOA     franky.ti2.com. root.franky.ti2.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
            ;
            @       IN      NS      franky.ti2.com.
            @       IN      A       192.212.2.2 ; IP EniesLobby
            www     IN      CNAME   franky.ti2.com.
            super   IN      A       192.212.2.4 ; IP Skypie
            " > /etc/bind/kaizoku/franky.ti9.com

       ```
    
   2. Restart Bind9 2x

        ```
        service bind9 restart
        service bind9 restart
        ```

   3. Buat zone super.franky.ti2.com dengan mengedit file /etc/bind/named.conf.local menjadi baris kode di bawah. Izinkan untuk menotifikasi dan mentransfer ke IP Water7.

        ```
        echo 'zone "super.franky.ti2.com"{
            type master;
            also-notify { 192.212.2.3; };
            allow-transfer { 192.212.2.3; };
            file "/etc/bind/kaizoku/super.franky.ti2.com";
        };' >> /etc/bind/named.conf.local
        ```

   4. Copy db.local dan buat file super.franky.ti2.com
        
        ```
        cp /etc/bind/db.local /etc/bind/kaizoku/super.franky.ti2.com
        ```

   5. Konfigurasikan super.franky.ti2.com
        
        ```typescript
        echo "\
            \$TTL    604800
            @       IN      SOA     super.franky.ti2.com. root.super.franky.ti2.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
            ;
            @       IN      NS     super.franky.ti2.com.
            @       IN      A      192.212.2.4
            www     IN      CNAME  super.franky.ti2.com.
            " > /etc/bind/kaizoku/super.franky.ti2.com
        ```

   6. Restart Bind9 2x

        ```
        service bind9 restart
        service bind9 restart
        ```
   
   **Pada Loguetown**

1. Ketikan script seperti pada gambar berikut
    
   <img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Loguetown/no03_script.png?raw=true" width="500">
    
2. Setelah itu, kembali ke terminal gns3 dan jalankan
    
 ```
 bash /root/script.sh
 ```
3. Ping subdomain super.franky.yyy.com beserta alias www.super.franky.yyy.com
    
   <img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Loguetown/no3_Loguetown.png?raw=true" width="500">
   
---

## Nomor 04
   **Soal**
   
   Buat juga reverse domain untuk domain utama
   
   **Jawaban**
   
   **A. EniesLobby**
   
   1. Tambahkan zone ``2.212.192.in-addr.arpa``
        
        ```
        echo 'zone "2.212.192.in-addr.arpa"{
        type master;
        file "/etc/bind/kaizoku/2.212.192.in-addr.arpa";
        };' >> /etc/bind/named.conf.local
        ```

   2. Copy db.local yang akan digunakan untuk file 2.212.192.in-addr.arpa
   
        ```
        cp /etc/bind/db.local /etc/bind/kaizoku/2.212.192.in-addr.arpa
        ```

   3. Konfigurasikan reverse DNS pada file 2.212.192.in-addr.arpa. Gantikan localhost dengan franky.ti2.com. 
        
        ```
        echo "\
            \$TTL    604800
            @       IN      SOA     franky.ti2.com. root.franky.ti2.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
            ;
            2.212.192.in-addr.arpa.       IN      NS      franky.ti2.com.
            4                             IN      PTR       franky.ti2.com.
            " > /etc/bind/kaizoku/2.212.192.in-addr.arpa
        ```

   4. Restart Bind9 2x

        ```
        service bind9 restart
        service bind9 restart
        ```
   
   **Loguetown**

1. Install dnsutils pada script.sh

<img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Loguetown/no04_install.png?raw=true" width="500">

2. Pastikan sudah echo nameserver Skypie

<img src ="https://user-images.githubusercontent.com/57980125/139377701-09568dc4-5142-4886-adfd-0013cde3726f.png" width="500">

3. Cek konfigurasi menggunakan `host -t PTR "IP Skypie"`
        
        ```
        host -t PTR 192.212.2.4
        ```

<img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Loguetown/no04_testing.png?raw=true" width="500">

Jika menampilkan 2.2.46.10.in-addr.arpa. points to franky.ti2.com seperti pada gambar, maka program sukses.

---

## Nomor 05
   **Soal**
   
   Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak, maka buat Water7 sebagai DNS Slave untuk domain utama
   
   **Jawaban**

   **EniesLobby**
   
   1. Edit zone franky.ti2.com maupun zone super.franky.ti2.com dengan menggantikan IP pada bagian also-notify dan allow-transfer dengan IP Water7, yaitu: ``192.212.2.3``
      
      <img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/EnniesLobby/no05_DNSSlave.png?raw=true" width="500">

   2. Dikarenakan pada zone franky.ti2.com penulis menggunakan tanda ``>`` yang menandakan isi pada file ``named.conf.local`` tersebut akan di overwrite sehingga isi sebelumnya hilang, maka penulis akan menambahkan zone untuk ``2.212.192.in-addr.arpa``

      <img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/EnniesLobby/no05_2.212.192.in-addr.arpa.png?raw=true" width="500">
   
   3. Restart service bind9
        
        ```
        service bind9 restart
        service bind9 restart
        ```

   4. Matikan service bind9
        
        ```
        service bind9 stop
        ```
        
        <img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/EnniesLobby/no05_stopService.png?raw=true" width="500">

**Water7**

1. Lakukan update ubuntu dan installasi bind9
    
    ```
    apt-get update
    apt-get install nano
    apt-get install bind9 -y
    ```

2. Konfigurasi zone untuk node Water7
   
   ```
    echo 'zone "franky.ti2.com"{
        type slave;
        masters { 192.212.2.2;  }; 
        file "/var/lib/bind/franky.ti9.com";
    };' > /etc/bind/named.conf.local
   ```


3. Restart bind9
    
    ```
    # Restart
        service bind9 restart
        service bind9 restart
    ```

**Dokumentasi dan Testing**
   
1. Pada client Loguetown pastikan pengaturan nameserver mengarah ke IP EniesLobby dan IP Water7
   
   <img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/EnniesLobby/no05_Nameserver.png?raw=true" width="500">

2. Lakukan ping ke franky.ti2.com pada client Loguetown. Jika ping berhasil maka konfigurasi DNS slave telah berhasil. Namun pada gambar IP yang dituju merupakan IP Skypie dikarenakan pada file script.sh, eksekusi program paling mengarah pada IP Skypie.
    
    ```
    ping franky.ti2.com
    ```
    
    <img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Loguetown/no05_Loguetown.png?raw=true" width="500">


## No 06

### Soal

Setelah itu terdapat subdomain mecha.franky.yyy.com dengan alias www.mecha.franky.yyy.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo

### Jawaban

**EniesLobby**

1. Konfigurasikan franky.ti2.com
   
   ```
    echo "\
        \$TTL    604800
        @       IN      SOA     franky.ti2.com. root.franky.ti2.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
        ;
        @       IN      NS      franky.ti2.com.
        @       IN      A       192.212.2.4 
        www     IN      CNAME   franky.ti2.com.
        super   IN      A       192.212.2.4
        ns1     IN      A       192.212.2.3    
   ```

2. Configure  allow-query any and comment dnssec-validation auto
   
   ```
   options {
        directory "/var/cache/bind";
        allow-query{any;};
        auth-nxdomain no;
        listen-on-v6 { any; };
    };' > /etc/bind/named.conf.options
    ```

3. Restart bind9
    
    ```
        # restart
        service bind9 restart
        service bind9 restart
    ```

**Water7**

1. Konfigurasi allow-query any dan comment dnssec-validation auto seperti pada modul
    
    ```
    $ echo 'options {
        directory "/var/cache/bind";
        allow-query{any;};
        auth-nxdomain no;   
        listen-on-v6 { any; };
    };' > /etc/bind/named.conf.options
    ```

2. Ubah konfigurasi pada zone ``mecha.franky.ti2.com`` dan file ``named.conf.local``
    
    ```
    echo 'zone "mecha.franky.ti2.com"{
        type master;
        file "/etc/bind/sunnygo/mecha.franky.ti2.com";
    };' >> /etc/bind/named.conf.local
    ```

3. Buat folder ``sunnygo`` dan letakkan file hasil copy dari ``db.local`` yang sekarang bernama ``mecha.franky.ti2.com``
    
    ```
    mkdir /etc/bind/sunnygo
    cp /etc/bind/db.local /etc/bind/sunnygo/mecha.franky.ti2.com
    ```

4. Lakukan konfigurasi pada mecha.franky.ti2.com
    
    ```
    echo "\
        \$TTL    604800
        @       IN      SOA     mecha.franky.ti2.com. root.mecha.franky.ti2.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
        ;
        @         IN      NS      mecha.franky.ti2.com.
        @         IN      A       10.46.2.3
        www       IN      A       10.46.2.3
    " > /etc/bind/sunnygo/mecha.franky.ti2.co
    ```

5. Restart Bind9
    
    ```
    service bind9 restart
    ```

**Loguetown**

1. Lakukan ping pada subdomain delegasi
```
ping mecha.franky.ti2.com
```

2. Lakukan ping pada alias subdomain delegasi
```
ping www.mecha.franky.ti2.com
```

<img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Loguetown/no06_Loguetown.png?raw=true" width="500">

## No 07

### Soal
Untuk memperlancar komunikasi Luffy dan rekannya, dibuatkan subdomain melalui Water7 dengan nama general.mecha.franky.yyy.com dengan alias www.general.mecha.franky.yyy.com yang mengarah ke Skypie

### Jawaban

**Water7**

1. Buat Subdomain ``general.mecha.franky.ti9.com``
    
    ```
   
    echo "\
        \$TTL    604800
        @       IN      SOA     mecha.franky.ti2.com. root.mecha.franky.ti2.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
        ;
        @        IN      NS      mecha.franky.ti2.com.
        @         IN      A       192.212.2.3
        www       IN      A       192.212.2.3
        general   IN      A       192.212.2.4

        " > /etc/bind/sunnygo/mecha.franky.ti2.com
    ```

2. Restart Bind9
3. Buat atau konfigurasi Zona Subdomain www.general.mecha.franky.ti9.com
    
    ```
    echo 'zone "general.mecha.franky.ti2.com"{
        type master;
        also-notify { 192.212.2.4; };
        allow-transfer { 192.212.2.4; };
        file "/etc/bind/sunnygo/general.mecha.franky.ti2.com";
    };' >> /etc/bind/named.conf.local
    ```

4. Copy db.local dan ubah menjadi general.mecha.franky.ti2.com
    
    ```
    cp /etc/bind/db.local /etc/bind/sunnygo/general.mecha.franky.ti2.com
    ```

5. Konfigurasi general.mecha.franky.ti2.com
    
    ```
    echo "\   
        \$TTL    604800
        @       IN      SOA     general.mecha.franky.ti2.com. root.general.mecha.franky$
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
        ;
        @       IN      NS     general.mecha.franky.ti2.com.
        @       IN      A      192.212.2.4
        www     IN      CNAME  general.mecha.franky.ti2.com.
        " > /etc/bind/sunnygo/general.mecha.franky.ti2.com
    ```

6. Restart Bind9

**Loguetown**

Lakukan ping padasubdomain general.mecha
```
ping general.mecha.franky.ti2.com
ping www.general.mecha.franky.ti2.com
```
<img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Loguetown/no07_Loguetown.png?raw=true" width="500">

# Web Server

## No 08

### Soal
Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.franky.yyy.com. Pertama, luffy membutuhkan webserver dengan DocumentRoot pada /var/www/franky.yyy.com.

### Jawaban
**Tahap Persiapan**

1. Update node ubuntu
    
    ```
    apt-get update
    ```

2. Install dan start Apache
    
    ```
    apt-get install apache2
    service apache2 start
    ```

3. Install dan cek versi php
    
    ```
    apt-get install php
    php -v
    ```

4. Install libapache php
    
    ```
    apt-get install libapache2-mod-php7.0
    ```

**KONFIGURASI WEBSERVER**

1. Inisialisasi DocumentRoot ``/var/www/franky.ti2.com``
    
    ```
    echo "\
        <VirtualHost *:80>
            ServerAdmin webmaster@localhost
            DocumentRoot /var/www/franky.ti2.com
            ServerName franky.ti2.com
            ServerAlias www.franky.ti2.com
            ErrorLog \${APACHE_LOG_DIR}/error.log
            CustomLog \${APACHE_LOG_DIR}/access.log combined
        </VirtualHost>" > /etc/apache2/sites-available/franky.ti2.com.conf
    ```

2. Aktifkan (ENABLE) konfigurasi website yang telah dibuat dengan *command* ``a2ensite`` diikuti dengan nama file konfigurasi yang telah dibuat. Dalam praktikum ini, nama konfigurasi yang dibuat adalah ``franky.ti2.com.conf``. 
    
    ```
    a2ensite franky.ti2.com.conf
    ```

3. Buat directory baru
    
    ```
    mkdir /var/www/franky.ti2.com
    ```

4. Restart apache2
    
    ```
    service apache2 restart
    service apache2 restart
    ```


## No 09

### Soal
Setelah itu, Luffy juga membutuhkan agar url www.franky.yyy.com/index.php/home dapat menjadi menjadi www.franky.yyy.com/home. 

### Jawaban
**TAHAP PERSIAPAN**

1. Install unzip pada Ubuntu Skypie
   
   ```
   apt-get install unzip
   
   ```
2. Install wget
    
    ```
    apt-get install wget
    
    ```
**Directory Alias pada Skypie**
1. Unduh  franky.zip
    
    ```
    wget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/franky.zip
    ```

2. Unzip file franky.zip lalu remove file asli (file .zip)
   
    ```
    unzip -j /root/franky.zip -d /var/www/franky.ti2.com
    rm /root/franky.zip
    ``` 

3. Aktivasi modul rewrite
    
    ```
    a2enmod rewrite
    ```

4. Restart Apache2
5. Hapus index.php dengan menambahkan htaccess 
    
    ```
    echo '
        RewriteEngine On
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteCond %{REQUEST_FILENAME} !-d
        RewriteRule ^(.*)$ index.php/$1 [L]' > /var/www/franky.ti2.com/.htaccess
    ```

6. Konfigurasikan pada sites-available 
    
    ```
    echo "\
        <VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/franky.ti2.com
        ServerName franky.ti2.com
        ServerAlias www.franky.ti2.com
        <Directory /var/www/franky.ti2.com>
                Options +FollowSymLinks -Multiviews
                AllowOverride All
        </Directory>
        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log combined
        </VirtualHost>" > /etc/apache2/sites-available/franky.ti2.com.conf
    ```

7. Restart Apache2

**Testing pada Lougetown**
1. Tanpa menghilangkan index.php
   
   ```
   lynx www.franky.ti2.com/index.php/home
   ```
   
   <img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Loguetown/no09_01.png?raw=true" width="500">
   
2. Sesuai minta soal (tanpa index.php)
   
   ```
   lynx www.franky.ti2.com/home
   ```
   
   <img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Loguetown/no09_02.png?raw=true" true="500">
   
## No 10

### Soal
Setelah itu, pada subdomain www.super.franky.yyy.com, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/super.franky.yyy.com.

### Jawaban
**Documment Root pada Skype**
1. Inisialisasi DocumentRoot /var/www/super.franky.ti2.com
    
    ```
    echo "\
        <VirtualHost *:80>
            ServerAdmin webmaster@localhost
            DocumentRoot /var/www/super.franky.ti2.com
            ServerName super.franky.ti2.com
            ServerAlias www.super.franky.ti2.com
            ErrorLog \${APACHE_LOG_DIR}/error.log
            CustomLog \${APACHE_LOG_DIR}/access.log combined
        </VirtualHost>" > /etc/apache2/sites-available/super.franky.ti2.com.conf
    ```

2. Aktivasi super.franky.ti2.com.conf
    
    ```
    a2ensite super.franky.ti2.com.conf
    ```

3. Buat directory baru 
    
    ```
    mkdir /var/www/super.franky.ti2.com
    ```

4. Restart Apache2

## No 11

### Soal
Akan tetapi, pada folder /public, Luffy ingin hanya dapat melakukan directory listing saja

### Jawaban
**Directory Listing**
1. Unduh file .zip yang ada pada github, lalu unzip file tersebut. Setelah itu, hapus file yang telah diekstrak.
    
    ```
    wget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/super.fra$
    unzip  /root/super.franky.zip 
    rm /root/super.franky.zip
    ```
    
2. Ubah nama ``super.franky/*``
    
    ```
    mv super.franky/* /var/www/super.franky.ti2.com/
    ```

3. Remove ``super.franky/``
    
    ```
    rm -r super.franky/
    ```

4. Aktifkan Directory Listing
    
    ```
    echo "\
    <VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/super.franky.ti2.com
        ServerName super.franky.ti2.com
        ServerAlias www.super.franky.ti2.com
        <Directory /var/www/super.franky.ti2.com/public>
                Options +Indexes
        </Directory>
        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>" > /etc/apache2/sites-available/super.franky.ti2.com.conf
    ```
5. Restart Apache2

**Testing pada Lougetown**

1. Uji coba directory listing public
```
lynx www.super.franky.ti2.com/public
```

<img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Loguetown/no11.png?raw=true" width="500">

## No 12

### Soal
Tidak hanya itu, Luffy juga menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache.

### Jawaban
**File Error 404**
1. Customize errordocument /var/www/super.franky.ti2.com/error/404.html
    
    ```
    echo "\
        <VirtualHost *:80>
            ServerAdmin webmaster@localhost
            DocumentRoot /var/www/super.franky.ti2.com
            ServerName super.franky.ti2.com
            ServerAlias www.super.franky.ti2.com
            <Directory /var/www/super.franky.ti2.com/public>
                Options +Indexes
            </Directory>
            ErrorLog \${APACHE_LOG_DIR}/error.log
            CustomLog \${APACHE_LOG_DIR}/access.log combined
            ErrorDocument 404 /error/404.html
        </VirtualHost>" > /etc/apache2/sites-available/super.franky.ti2.com.conf
    ```
2. Restart Apache2

**Testing pada Lougetown**

1. Uji coba error 404
```
lynx www.super.franky.ti2.com/error404
```

<img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Loguetown/no12.png?raw=true" width="500">

## No 13

### Soal
Luffy juga meminta Nami untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.super.franky.yyy.com/public/js menjadi www.super.franky.yyy.com/js

### Jawaban
**Konfigurasi Virtual Host**
1. Custom directory alias
    
    ```
        echo "\
            <VirtualHost *:80>
            ServerAdmin webmaster@localhost
            DocumentRoot /var/www/super.franky.ti2.com
            ServerName super.franky.ti2.com
            ServerAlias www.super.franky.ti2.com
            <Directory /var/www/super.franky.ti2.com/public>
                Options +Indexes
                <Directory /var/www/super.franky.ti2.com/public/js>
                    Options +Indexes
                </Directory>
            Alias "/js" "/var/www/super.franky.ti2.com/public/js"
            ErrorLog \${APACHE_LOG_DIR}/error.log
            CustomLog \${APACHE_LOG_DIR}/access.log combined
            ErrorDocument 404 /error/404.html
            </VirtualHost>" > /etc/apache2/sites-available/super.franky.ti2.com.conf
    ```
2. Restart Apache2

**Testing pada Lougetown**

1. Uji coba directory js
```
lynx www.super.franky.ti2.com/js
```

<img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Loguetown/no13.png?raw=true" width="500">

## No 14

### Soal
Dan Luffy meminta untuk web www.general.mecha.franky.yyy.com hanya bisa diakses dengan port 15000 dan port 15500

### Jawaban
**Listen Port**
1. Listen Port 80, 15000, dan 15500
2. Buat Virtual Host
3. Aktifkan konfigurasi   
    
    ```
    a2ensite general.mecha.franky.ti2.com.conf
    ```
4. Buat directory baru
    
    ```
    mkdir /var/www/general.mecha.franky.ti2.com
    ```
5. Unduh file .zip dari github, lalu unzip file tersebut ke dalam folder yang telah dibuat sebelumnya.
    
    ```
    wget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/general.mecha.franky.zip
    unzip -j /root/general.mecha.franky.zip -d /var/www/general.mecha.franky.ti2.com
    ```
6. Hapus file .zip
    
    ```
    rm /root/general.mecha.franky.zip
    ```
7. Restart Apache2

## Nomor 15

### Soal
Dengan authentikasi username luffy dan password onepiece dan file di `/var/www/general.mecha.franky.yyy`

## Jawaban  
**Skypie**     
1. Jalankan Command 
```
htpasswd -c -b /etc/apache2/.htpasswd luffy onepiece
```
2. Konfigurasi file `/etc/apache2/sites-available/general.mecha.franky.ti2.com.conf`
```
<VirtualHost *:15000>

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/general.mecha.franky.ti2.com
        ServerName general.mecha.franky.ti2.com
        ServerAlias www.general.mecha.franky.ti2.com

        <Directory \"/var/www/general.mecha.franky.ti2.com\">
                AuthType Basic
                AuthName \"Restricted Content\"
                AuthUserFile /etc/apache2/.htpasswd
                Require valid-user
        </Directory>

        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
<VirtualHost *:15500>        
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/general.mecha.franky.ti2.com
        ServerName general.mecha.franky.ti2.com
        ServerAlias www.general.mecha.franky.ti2.com
        
        <Directory \"/var/www/general.mecha.franky.ti2.com\">
                AuthType Basic
                AuthName \"Restricted Content\"
                AuthUserFile /etc/apache2/.htpasswd
                Require valid-user
        </Directory>
        
        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```    
3. Restart service apache2 
```
service apache2 restart
```   

**Loguetown**   
1. Uji coba directory port 15500 dan 15000
```
lynx general.mecha.franky.ti2.com:15500
```
<img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Loguetown/no14_01.png?raw=true" width="500">

```
lynx www.general.mecha.franky.ti2.com:15500
```

<img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Loguetown/no14_02.png?raw=true" width="500">

```
lynx general.mecha.franky.ti2.com:15000
```

<img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Loguetown/no14_03.png?raw=true" width="500">

```
lynx www.general.mecha.franky.ti2.com:15000
```

<img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Loguetown/no14_04.png?raw=true" width="500">


## Nomor 16

### Soal
Dan setiap kali mengakses IP Skypie akan diahlikan secara otomatis ke `www.franky.yyy.com`

### Jawaban Soal 16  
**Skypie**   
1. Konfigurasi file `/etc/apache2/sites-available/000-default.conf` sebagai berikut     
```
<VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html

        RewriteEngine On
        RewriteCond %{HTTP_HOST} !^franky.ti2.com$
        RewriteRule /.* http://franky.ti2.com/ [R]

        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
```    
2. Melakukan restart service apache2 dengan `service apache2 restart`  

**Loguetown**    

1. Ketikan `lynx 192.212.2.4` dan akan muncul seperti pada gambar
<img src="https://user-images.githubusercontent.com/57980125/139524766-b42ea204-2ba3-4a9d-bd4a-80241291910f.png" width="500">
<img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Loguetown/16_02.png?raw=true" width="500">


## Nomor 17

### Soal
Dikarenakan Franky juga ingin mengajak temannya untuk dapat menghubunginya melalui website `www.super.franky.yyy.com`, dan dikarenakan pengunjung web server pasti akan bingung dengan randomnya images yang ada, maka Franky juga meminta untuk mengganti request gambar yang memiliki substring `“franky”` akan diarahkan menuju `franky.png`. Maka bantulah Luffy untuk membuat konfigurasi dns dan web server ini!

### Jawaban   
**Skypie**   
1. konfigurasi file `/var/www/super.franky.ti2.com/.htaccess` dengan     
```
echo "
RewriteEngine On
RewriteCond %{REQUEST_URI} ^/public/images/(.*)franky(.*)
RewriteCond %{REQUEST_URI} !/public/images/franky.png
RewriteRule /.* http://super.franky.ti2.com/public/images/franky.png [L]
"
```    
2. konfigurasi file `/etc/apache2/sites-available/super.franky.ti2.com.conf` dengan     
```
echo "
<VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/super.franky.ti2.com
        ServerName super.franky.ti2.com
        ServerAlias www.super.franky.ti2.com

        ErrorDocument 404 /error/404.html
        ErrorDocument 500 /error/404.html
        ErrorDocument 502 /error/404.html
        ErrorDocument 503 /error/404.html
        ErrorDocument 504 /error/404.html

        <Directory /var/www/super.franky.ti2.com/public>
                Options +Indexes
        </Directory>

        Alias \"/js\" \"/var/www/super.franky.ti2.com/public/js\"

        <Directory /var/www/super.franky.ti2.com>
                Options +FollowSymLinks -Multiviews
                AllowOverride All
        </Directory>
        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log combined

        <Directory /var/www/franky.ti2.com>
                Options +FollowSymLinks -Multiviews
                AllowOverride All
        </Directory>
</VirtualHost>
"
```    
3. restart service apache2 dengan `service apache2 restart`  

**Loguetown**     
1. Ketikan command berikut pada Client
```
lynx super.franky.ti2.com/public/images/HAHAfrankyYUHU
```
2. Program akan menampilkan seperti pada gambar. Pilih opsi D atau Download
<img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Loguetown/17_01.png?raw=true" width="500">
3. Image akan terunduh
<img src="https://github.com/Herwindams24/Jarkom-Modul-2-T02-2021/blob/main/Gambar/Loguetown/17_02.png?raw=true" true="500">

## Kendala
