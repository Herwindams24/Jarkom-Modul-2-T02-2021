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

    ![Image of Run Foosha](https://raw.githubusercontent.com/Herwindams24/Jarkom-Modul-2-T02-2021/main/Gambar/Foosha/chrome_JFysuv9QHq.png?)

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

        ![Image of bashrc](https://raw.githubusercontent.com/Herwindams24/Jarkom-Modul-2-T02-2021/main/Gambar/Foosha/bashrc.png)
   
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

    

   **Dokumentasi**
   
   
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



   **Dokumentasi**

---

## Nomor 05
   **Soal**
   
   Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak, maka buat Water7 sebagai DNS Slave untuk domain utama
   
   **Jawaban**

   **EniesLobby**
   
   1. Edit zone franky.ti2.com maupun zone super.franky.ti2.com dengan menggantikan IP pada bagian also-notify dan allow-transfer dengan IP Water7, yaitu: ``192.212.2.3``
      
      ![no05_DNSSLAVE]()

   2. Dikarenakan pada zone franky.ti2.com penulis menggunakan tanda ``>`` yang menandakan isi pada file ``named.conf.local`` tersebut akan di overwrite sehingga isi sebelumnya hilang, maka penulis akan menambahkan zone untuk ``2.212.192.in-addr.arpa``

      ![no05_2.212.192.in-addr.arpa]()
   
   3. Restart service bind9
        
        ```
        service bind9 restart
        service bind9 restart
        ```

   4. Matikan service bind9
        
        ```
        service bind9 stop
        ```

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
   
   ![no05_nameserver]()

2. Lakukan ping ke franky.ti2.com pada client Loguetown. Jika ping berhasil maka konfigurasi DNS slave telah berhasil

    ![no05_ping]()


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


## No 08

### Soal
Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.franky.yyy.com. Pertama, luffy membutuhkan webserver dengan DocumentRoot pada /var/www/franky.yyy.com.

### Jawaban
**TAHAP PERSIAPAN**

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

1. Install unzip pada Ubuntu
   
   ```
   apt-get install unzip
   
   ```
2. Install wget
    
    ```
    apt-get install wget
    
    ```
**Directory Alias**
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

## No 10

### Soal
Setelah itu, pada subdomain www.super.franky.yyy.com, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/super.franky.yyy.com.

### Jawaban
**Inisialisasi Documment Root**
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
