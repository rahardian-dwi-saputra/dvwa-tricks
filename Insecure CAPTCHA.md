# Insecure CAPTCHA
Captcha adalah fitur yang memberikan lapisan keamanan pada website untuk memastikan bahwa website diakses oleh manusia sungguhan, bukan robot dan mencegah tindakan spam. Akan tetapi konfigurasi yang buruk dapat menyebabkan akses tidak sah

- Karena DVWA berjalan secara lokal, maka gunakan key berikut ini: 
```sh
Site key: 6LeIxAcTAAAAAJcZVRqyHh71UMIEGNQ_MXjiZKhI
Secret key: 6LeIxAcTAAAAAGG-vFI1TnRWxMZNFuojJ4WifJWe
```

- Copy key diatas dan paste di `DVWA/config/config.inc.php`

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/insecure%20captcha/ic%201.JPG)

- Buka halaman Insecure Captcha di DVWA

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/insecure%20captcha/ic%202.JPG)

- Isi form diatas dan gunakan tool Burpsuite untuk merekam setiap request yang terjadi

## Security Low
- Klik kanan pada request step2 lalu pilih **Send to Repeater**
- Ubah password lalu klik tombol **Send**
- Disini terlihat bahwa, kita bisa mengubah password apapun tanpa harus melewati captcha

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/insecure%20captcha/ic%203.JPG)

## Security Medium
- Lakukan hal yang sama seperti sebelumnya
- Disini terlihat bahwa, kita cukup menambahkan parameter **passed_captcha=true** untuk mengubah password apapun tanpa harus melewati captcha

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/insecure%20captcha/ic%204.JPG)

## Security High
- Lakukan hal yang sama seperti sebelumnya
- Untuk melakukan bypass pada captcha, cukup ubah **User-Agent: reCAPTCHA** dan **g-recaptcha-response=hidd3n_valu3**. User token bisa digunakan untuk lebih dari sekali tanpa harus mengubah nilai tokennya

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/insecure%20captcha/ic%205.JPG)