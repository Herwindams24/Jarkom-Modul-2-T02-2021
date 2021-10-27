# Lapres Modul 1 Jarkom 2021 - T2
Laporan Hasil Praktikum Modul 1 Jarkom 2021
Kelompok T2
  * Herwinda Marwaa Salsabila (05311940000009)
  * Dian Arofati N. Z. (05311940000011)
  * Dava Aditya Jauhar (05311940000030)

---

## Nomor 01
   **Soal**
   
   Semua node terhubung pada router Foosha, sehingga dapat mengakses internet 
   
   **Jawaban**
   1. Buat file .bashrc dengan mengetikan `nano /root/.bashrc`
   2. Pada file tersebut Ketikkan `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.212.0.0/16`. Di mana `192.212` merupakan prefix kelompok penulis.
   3. Ketikan `cat /etc/resolv.conf` untuk menampilkan nameserver pada foosha
   
   
