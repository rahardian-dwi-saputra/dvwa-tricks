## Authorisation Bypass
Insecure direct object references (IDOR) adalah jenis kerentanan yang disebabkan oleh kesalahan penerapan kontrol akses (access control) sehingga pengguna biasa dapat mengakses fitur-fitur khusus yang hanya dimiliki oleh admin atau mengakses data milik pengguna lain

### Security Low
- Login dengan username **gordonb** dan password **abc123** kemudian akses halaman **Authorisation Bypass** dengan url sebagai berikut
```sh
http://<IP_Server>/DVWA/vulnerabilities/authbypass/
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2061.JPG)

### Security Medium
```sh
http://<IP_Server>/DVWA/vulnerabilities/authbypass/get_user_data.php
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2062.JPG)

### Security High
- Ulangi langkah pada level **Security Low**
- Tekan salah satu tombol Update di halaman **Authorisation Bypass** dan tangkap requestnya menggunakan tool Burp Suite. Klik kanan pada kolom request dan pilih **Send to Repeater**

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2063.JPG)

- Ubah security menjadi **high** dan klik tombol **send**

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2064.JPG)