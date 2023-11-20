# Weak Session IDS
ID sesi yang lemah dapat memungkinkan pengguna lain dibajak sesinya. Jika ID sesi diambil dari rentang nilai yang kecil, penyerang hanya perlu menyelidiki ID sesi yang dipilih secara acak hingga menemukan kecocokan.

## Security Low
- Klik tombol **Generate** lalu klik kanan pada halaman dan pilih **Inspect**
- Generate pertama

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/weak%20session%20ids/weak%20session%201.JPG)

- Generate kedua

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/weak%20session%20ids/weak%20session%202.JPG)

- Generate ketiga

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/weak%20session%20ids/weak%20session%203.JPG)

- Disini terlihat bahwa **dvwaSession** terbentuk dari angka 1,2,3 dan seterusnya sehingga nilainya sangat mudah diprediksi

## Security Medium
- Generate pertama

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/weak%20session%20ids/weak%20session%204.JPG)

- Generate kedua

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/weak%20session%20ids/weak%20session%205.JPG)

- Generate ketiga

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/weak%20session%20ids/weak%20session%206.JPG)

- Nilai **dvwaSession** adalah 1692868183, 1692868290, 1692868433 dimana angka ini adalah **unix time**. Anda bisa mengkonversi nilai tersebut secara online untuk pembuktian

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/weak%20session%20ids/weak%20session%207.JPG)

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/weak%20session%20ids/weak%20session%208.JPG)

## Security High
- Generate pertama

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/dt%2078.JPG)

- Generate kedua

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/weak%20session%20ids/weak%20session%209.JPG)

- Generate ketiga

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/weak%20session%20ids/weak%20session%2010.JPG)

- Nilai **dvwaSession** adalah c81e728d9d4c2f636f067f89cc14862c, eccbc87e4b5ce2fe28308fd9f2a7baf3, a87ff679a2f3e71d9181a67b7542122c. Nilai tersebut menunjukkan sebuah hash. Anda bisa melakukan cracking di https://crackstation.net/

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/weak%20session%20ids/weak%20session%2011.JPG)