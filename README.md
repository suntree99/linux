# Linux

## Konfigurasi Firewall dengan iptables

1. Memeriksa versi iptables
	```cmd
	sudo iptables -V
	```

2. Jika iptables belum terinstall, lakukan instalasi
	```cmd
	sudo apt-get update
	sudo apt-get install iptables
	```

3. Memeriksa status konfigurasi
	```cmd
	sudo iptables -L -v
	```
	Catatan: 
	* Perintah -L berfungsi untuk melihat list (daftar) semua aturan yang ada, sedangkan -v untuk menampilkan list tersebut secara detail.

4. Mengecek IP Config
	* Windows
	```cmd
	ipconfig
	```
	* Linux
	```cmd
	ifconfig
	```

5. Melakukan ping
	* Windows
	```cmd
	ping <inet eth0>
	```
	* Linux
	```cmd
	ping <default gateway>
	```

6. Memblokir komunikasi jaringan
	```cmd
	sudo iptables -A OUTPUT -p icmp --icmp-type echo-request -j DROP
	```
	```cmd
	sudo iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
	```
	Catatan:
	* sudo : Langkah untuk mendapatkan akses superuser atau administrator, biasanya akan meminta password.
	* -A OUTPUT : Rule atau aturan akan ditambahkan ke chain OUTPUT (yang mengelola akses keluar).
	* -A INPUT : Rule atau aturan akan ditambahkan ke chain INPUT (yang mengelola akses masuk).
	* -p icmp : Menentukan protokol mana yang akan diblokir, dalam hal ini adalah ICMP (protokol untuk melakukan ping).
	* --icmp-type echo-request : Memilih tipe ICMP yang dimaksud, yakni echo-request.
	* -j DROP : Menentukan aksi yang akan dilakukan, dalam kasus ini berarti DROP.

7. Mereset semua rule
	```cmd
	sudo iptables -F
	```

##
##

# Heading 1 / Judul Utama (gunakan #)

## Heading 2 / Sub Judul (gunakan ##)

Text biasa (ditulis biasa tanpa format apapun)

[Hyperlink](https://www.google.com) (nama hyperlink dibungkus kurung siku, urlnya dibungkus tanda kurung biasa)

```bash
git add .
git commit -m "baris code menggunakan backtick 3x di awal(sertakan bahasanya) dan akhir code"
git push
```

Untuk `menyoroti` bungkus text dengan backtick 1x

## Package 
```go
Isi
```

Update README.md
