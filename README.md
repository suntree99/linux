# Linux

## Menjalankan Web Server

1. Menginstall NVM (Node Version Manager) pada Ubuntu
	```console
	curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
	```

2. Menginstall Node.js versi tertentu
	```console
	nvm install v14.15.4
	```

3. Menginstall NPM (Node Package Manager) untuk memasang beberapa dependencies pada project (pastikan berada dalam folder project)
	```console
	npm install
	```

4. Menjalankan web server
	```console
	npm run start
	```

## Memasang NGINX pada EC2 Instance

1. Menginstall NGINX di konsol EC2 instance
	```console
	sudo apt update
	sudo apt-get install nginx -y
	```

2. Memeriksa status NGINX
	```console
	sudo systemctl status nginx
	```

## Mengonfigurasi NGINX sebagai Reverse Proxy Server

1. Memasuki berkas NGINX
	```console
	cat /etc/nginx/sites-available/default
	```

2. Mengedit berkas NGINX
	```console
	sudo nano /etc/nginx/sites-available/default
	```

3. Mengedit location / (hapus kode lain yang berada di dalam blok location)
	```console
	location / {
		proxy_pass http://localhost:8000;
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header Connection 'upgrade';
		proxy_set_header Host $host;
		proxy_cache_bypass $http_upgrade;
	}
	```

4. Simpan perubahan dengan CTRL+X, lalu Y, dan Enter. jalankan ulang NGINX
	```console
	sudo systemctl restart nginx
	}
	```

5. Masuk kembali ke folder project dan jalankan kembali Web Server
	```console
	npm run start
	```

## Menerapkan Limit Access dengan NGINX di EC2 instance

1. Buka kembali berkas konfigurasi web server NGINX
	```console
	sudo nano /etc/nginx/sites-available/default
	```

2. Sebelum blok server, tuliskan kode berikut
	```console
	limit_req_zone $binary_remote_addr zone=one:10m rate=30r/m;
	```

3. Tambahkan kode berikut di dalam blok location /
	```console
	limit_req zone=one;
	```
	* Catatan: Kode pada langkah ke-2 dan ke-3 merupakan sintaks untuk melimitasi akses pada web server NGINX. Berdasarkan kode tersebut, kita menginginkan pengguna yang mengakses resource / (root) hanya dapat membuat permintaan setiap 2 detik (rate=30r/m berarti 30 request per menit atau setara dengan 2 detik sekali).

4. Simpan perubahan dengan CTRL+X, lalu Y, dan Enter. jalankan ulang NGINX
	```console
	sudo systemctl restart nginx
	}
	```

5. Masuk kembali ke folder project dan jalankan kembali Web Server
	```console
	npm run start
	```

6. Akses kembali IP address EC2, lakukan reload dengan cepat sebelum 2 detik, maka seharusnya akan terblock (503 Service Temporarily Unavailable), tunggu 2 detik dan reload kembali makan akan kembali normal.

	* Dalam praktik ini, penerapan limit access berlaku pada seluruh cakupan path alias root (/). Jika Anda ingin menerapkan limit access pada resource secara spesifik (contohnya /authentications), definisikan pada blok *location /authentications*.

7. Terakhir, hapus inbound rule pada Security Group yang mengarah langsung ke aplikasi (yakni rule yang mengizinkan port 8000). Tujuannya supaya jalur untuk mengakses aplikasi hanya tersedia melalui reverse proxy server.

## Konfigurasi Subdomain di Amazon EC2

1. Mendapatkan subdomain dari dcdg.xyz dengan cURL melalui *Command prompt* atau *Terminal*
	```cmd
	curl -X POST -H "Content-type: application/json" -d "{ \"ip\": \"<public IP EC2 instance>\" }" "https://sub.dcdg.xyz/dns/records"
	```
	* Pastikan Anda mengubah <public IP EC2 instance> dengan public IP address EC2 instance Anda.

2. Catat nilai *hostname* dari response.

3. Menddaftarkan hostname sebagai domain NGINX, akses kembali EC2 Instance dan buka konfigurasi NGINX
 	```console
	sudo nano /etc/nginx/sites-available/default
	```
4. Tulis (ganti _ dengan) dua hostname server_name di dalam blok server (di atas location /).
	* Contoh
 	```console
	server_name calm-puma-33.a276.dcdg.xyz www.calm-puma-33.a276.dcdg.xyz;
	```

5. Simpan perubahan dengan CTRL+X, lalu Y, dan Enter. jalankan ulang NGINX
	```console
	sudo systemctl restart nginx
	}
	```

6. Masuk kembali ke folder project dan jalankan kembali Web Server
	```console
	npm run start
	```

## Konfigurasi Firewall dengan iptables

1. Memeriksa versi iptables
	```console
	sudo iptables -V
	```

2. Jika iptables belum terinstall, lakukan instalasi
	```console
	sudo apt-get update
	sudo apt-get install iptables
	```

3. Memeriksa status konfigurasi
	```console
	sudo iptables -L -v
	```
	* Perintah -L berfungsi untuk melihat list (daftar) semua aturan yang ada, sedangkan -v untuk menampilkan list tersebut secara detail.

4. Mengecek ipconfig
	* Windows
	```cmd
	ipconfig
	```
	* Linux
	```console
	ifconfig
	```

5. Melakukan ping
	* Windows
	```cmd
	ping <inet eth0>
	```
	* Linux
	```console
	ping <default gateway>
	```

6. Memblokir komunikasi jaringan
	```console
	sudo iptables -A OUTPUT -p icmp --icmp-type echo-request -j DROP
	```
	```console
	sudo iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
	```
	* sudo : Langkah untuk mendapatkan akses superuser atau administrator, biasanya akan meminta password.
	* -A OUTPUT : Rule atau aturan akan ditambahkan ke chain OUTPUT (yang mengelola akses keluar).
	* -A INPUT : Rule atau aturan akan ditambahkan ke chain INPUT (yang mengelola akses masuk).
	* -p icmp : Menentukan protokol mana yang akan diblokir, dalam hal ini adalah ICMP (protokol untuk melakukan ping).
	* --icmp-type echo-request : Memilih tipe ICMP yang dimaksud, yakni echo-request.
	* -j DROP : Menentukan aksi yang akan dilakukan, dalam kasus ini berarti DROP.

7. Mereset semua rule
	```console
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
