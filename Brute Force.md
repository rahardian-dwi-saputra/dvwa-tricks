# Brute Force
Serangan brute-force terjadi saat penyerang menggunakan sistem coba-coba (trial and error) untuk menebak kredensial pengguna yang valid. Penyerang dapat mengotomatisasi proses penyerangan brute force dengan tool khusus sehingga penyerang dapat melakukan upaya login dalam jumlah besar dengan kecepatan tinggi.

## Security Low
- Brute force untuk menemukan password admin menggunakan tool Burp Suite. Pertama-tama bikin attack dengan tipe **Sniper** set payload hanya pada parameter password seperti pada gambar dibawah

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2026.JPG)

- Buat payload dengan tipe **Simple List** dan klik load lalu pilih wordlist `/usr/share/seclists/Passwords/darkweb2017-top100.txt` dan jalankan serangan dengan menekam tombol Start Attack

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2027.JPG)

- Setelah proses penyerangan selesai, kita bisa lakukan sorting berdasarkan length. Request yang memiliki length paling berbeda adalah hasil brute force nya

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2028.JPG)

- Brute force password admin menggunakan tool wfuzz
```sh
wfuzz --hs "incorrect" -c -w /usr/share/seclists/Passwords/darkweb2017-top100.txt -b 'PHPSESSID=hash; security=low' 'http://<IP_Server>/DVWA/vulnerabilities/brute/index.php?username=admin&password=FUZZ&Login=Login'
``` 

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2029.JPG)

-Brute force password semua user dengan tool wfuzz
```sh
wfuzz --hs "incorrect" -c -z file,user-dvwa.txt -z file,/usr/share/seclists/Passwords/darkweb2017-top100.txt -b 'PHPSESSID=hash; security=low' 'http://<IP_Server>/DVWA/vulnerabilities/brute/index.php?username=FUZZ&password=FUZ2Z&Login=Login'
``` 

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2030.JPG)

## Security Medium
- Disini kita dapat melakukan brute force dengan cara yang sama, namun tiap request terjeda selama 2 detik sehingga proses brute force menjadi sangat lama 
```sh
wfuzz --hs "incorrect" -c -w /usr/share/seclists/Passwords/darkweb2017-top100.txt -b 'PHPSESSID=hash; security=low' 'http://<IP_Server>/DVWA/vulnerabilities/brute/index.php?username=admin&password=FUZZ&Login=Login'
``` 

## Security High
- Disini kita akan melakukan bypass terhadap **user_token**

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2031.JPG)

- Buat attack dengan type **Pitchfork** dan tempatkan payload pada parameter **password** dan **user_token**

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2032.JPG)

- Buat payload 1 dengan tipe **Simple List** dan klik load lalu pilih wordlist `/usr/share/seclists/Passwords/darkweb2017-top100.txt`

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2033.JPG)

- Buat payload 2 dengan tipe **Recursive Grep**

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2036.JPG)

- Clear semua string di **Grep - Match** lalu Add string **incorrect**

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2037.JPG)

- Tekan tombol **Add** pada bagian **Grep Extract** lalu block hash token lalu pilih **Start at offset** dan **End at fixed length**

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2038.JPG)

- Pada bagian **Redirections** pilih **Always**

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2039.JPG)

- Pada tab **Resource pool** pilih **Create new resource pool** lalu centang **Maximum concurrent requests** dan isi 1. Jika sudah tekan tombol **Start Attack** untuk memulai penyerangan

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2040.JPG)

- Setelah proses penyerangan selesai, kita bisa lakukan sorting berdasarkan length. Request yang memiliki length paling berbeda adalah hasil brute force nya

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2041.JPG)