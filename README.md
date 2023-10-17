# Jarkom-Modul-2-E-4-2023
Anggota Kelompok:
 1. Yoga Firman Syahputra (5025221212)
 2. Vidiawan Nabiel Rasyid (5025221231)

Soal:
1. Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian sebagai berikut. Folder topologi dapat diakses pada drive berikut
![Sadewa - Google Chrome 10_17_2023 7_27_17 PM](https://github.com/yogs14/Jarkom-Modul-2-E04-2023/assets/121499055/c736b4bb-5d70-479c-8f3a-b257daf2eb69)
Step:
Buka console Yudhistira, buat domainnya dengan nama sesuai node nya “yudhistira.com”:
Yudhistira: apt-get update
Yudhistira: apt-get install bind9 -y
Yudhistira: cd /etc/bind
Yudhistira: nano named.conf.local
(edit konfigurasi nya dengan menambah:
zone "yudhistira.com" {
	type master;
	file "/etc/bind/yudhistira/yudhistira.com";
};
)

Yudhistira: mkdir /etc/bind/yudhistira
Yudhistira: cp db.local yudhistira/yudhistira.com
Yudhistira: nano yudhistira/yudhistira.com
(edit konfigurasi menjadi seperti ini:)
![Yudhistira - Google Chrome 10_10_2023 10_26_52 PM](https://github.com/yogs14/Jarkom-Modul-2-E04-2023/assets/121499055/45b53bee-9768-4bb1-a1bd-25644e5421d5)
Yudhistira: service bind9 restart
(lalu lakukan ping dari node mana saja dengan mengarahkan node tersebut ke Node Yudhistira dengan mengarahkan node tersebut ke IP Node Yudhistira)

Berikutnya, jadikan Yudhistira sebagai DNS Master dengan mengedit kembali konfigurasi pada configurasi pada named.conf.local:
![Yudhistira - Google Chrome 10_10_2023 10_38_26 PM](https://github.com/yogs14/Jarkom-Modul-2-E04-2023/assets/121499055/a9c83ee3-e2c8-4d14-be44-9b6394884e98)
Yudhistira: service bind9 restart

Berikutnya, jadikan Werkudara sebagai DNS Slave dengan mengedit  konfigurasi pada named.conf.local:
Werkudara: apt-get update
Werkudara: apt-get install bind9 -y
Werkudara: cd /etc/bind
Werkudara: nano named.conf.local
![Screenshot 2023-10-10 225518](https://github.com/yogs14/Jarkom-Modul-2-E04-2023/assets/121499055/168a95aa-f400-49e1-9ab7-59ec73ab81b0)
Werkudara: service bind9 restart

Berikutnya, kita bisa melakukan testing dengan men-stop service bind9 pada Yudhistira dan lakukan PING kepada “yudhistira.com” dari node mana saja dengan mengarahkan pada juga node tersebut pada alamat IP dari Werkudara dan akan kita dapatkan upaya PING akan berhasil!

2. Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias 
www.arjuna.yyy.com dengan yyy merupakan kode kelompok.
Arjuna: apt-get update
Arjuna: apt-get install bind9 -y
Arjuna: nano named.conf.local
![Screenshot 2023-10-10 234750](https://github.com/yogs14/Jarkom-Modul-2-E04-2023/assets/121499055/b598a4ee-9fd1-4ec5-9d3e-fa2e1496b001)
Arjuna: mkdir arjuna
Arjuna: cp db.local arjuna/arjuna.e04.com
Arjuna: nano arjuna/arjuna.e04.com
(anggap kita belum memberi www)
![Screenshot 2023-10-10 235049](https://github.com/yogs14/Jarkom-Modul-2-E04-2023/assets/121499055/8cf24c9f-371d-40cc-9a88-d43296dd98cd)
Arjuna: service bind9 restart Saatnya kita membuat Canonical Name-nya, lakukan edit konfigurasi pada named.conf.local
Arjuna: nano named.conf.local
Arjuna: service bind9 restart
(Lakukan cek dengan melakukan host -t CNAME www.arjuna.e04.com atau ping arjuna.e04.com -c 5. Hasilnya harus mengarah ke host dengan IP Arjuna)

4. Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke 
abimanyu.yyy.com dan alias www.abimanyu.yyy.com.
Step: 
Abimanyu: apt-get update
Abimanyu: apt-get install bind9 -y
Abimanyu: cd /etc/bind Abimanyu: nano named.conf.local
(tambakan syntax dibawah ini:
zone "abimanyu.e04.com" {
type master;
   file "/etc/bind/abimanyu/abimanyu.e04.com";
    };
)
Abimanyu: service bind9 restart
Abimanyu: mkdir abimanyu
Abimanyu: cp dc.local abimanyu/abimanyu.e04.com
Abimanyu: nano abimanyu/abimanyu.e04.com
(edit konfigurasi menjadi seperti di bawah ini:
![Screenshot 2023-10-11 002412](https://github.com/yogs14/Jarkom-Modul-2-E04-2023/assets/121499055/98edbe78-f57f-4802-a6dc-7d86bf478dc1)
Abimanyu: service bind9 restart
(Lalu cek dengan melakukan host -t CNAME www.abimanyu.e04.com atau ping www.abimanyu.e04.com    -c 5. Hasilnya harus mengarah ke host dengan IP Abimanyu.)

4. Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.
Step:
(buka console Yudhistira)
Yudhistira:  nano /etc/bind/yudhistira/yudhistira.com
(edit konfigurasi menjadi seperti di bawah ini:
![Screenshot 2023-10-11 200836](https://github.com/yogs14/Jarkom-Modul-2-E04-2023/assets/121499055/b6a92583-59c5-4ca3-b3fb-01c0d41dae83)
Yudhistira: service bind9 restart

5. Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)
(Di dalam Yudhistira)
Yudhistira: nano named.conf.local
(tambahkan syntax sebagai berikut: 
![Screenshot 2023-10-11 203145](https://github.com/yogs14/Jarkom-Modul-2-E04-2023/assets/121499055/b0c20bc2-13e4-4d8b-8599-2a923b80f403)
Yudhistira: cp db.local yudhistira/3.208.192.in-addr.arpa
Yudhistira: nano named.conf.local
(edit konfigurasi sehingga menjadi seperti di bawah ini:
![Screenshot 2023-10-11 202037](https://github.com/yogs14/Jarkom-Modul-2-E04-2023/assets/121499055/310913b4-c071-442d-bf21-175929ebce7e)
)
Yudhistira: service bind9 restart

6. Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.
Yudhistira: nano named.conf.local
(edit konfigurasi menjadi seperti di bawah ini:
![Screenshot 2023-10-11 201856](https://github.com/yogs14/Jarkom-Modul-2-E04-2023/assets/121499055/455f1206-e4e7-4da7-8f4d-abaf2fad749b)
)
Yudhistira: service bind9 restart
(Beralih kepada Werkudara sebagai DNS Slave)
Werkudara: apt-get update
Werkudara: apt-get install bind9 -y
Werkudara: nano named.conf.local
(tambahkan syntax seperti di bawah ini:
![Screenshot 2023-10-11 203145](https://github.com/yogs14/Jarkom-Modul-2-E04-2023/assets/121499055/3f03b5b8-73ca-4a7f-9259-4861beee1556)
)
Yudhistira: service bind9 restart
(Beralih kepada Werkudara sebagai DNS Slave)
Werkudara: apt-get update
Werkudara: apt-get install bind9 -y
Werkudara: nano named.conf.local
(tambahkan syntax seperti di bawah ini:
![Screenshot 2023-10-11 203625](https://github.com/yogs14/Jarkom-Modul-2-E04-2023/assets/121499055/9bd16963-64db-47e6-af3d-a1954b2bba4e)
)
Werkudara: service bind9 restart
(Berikutnya testing dari Nakula dengan mengarahkannya ke IP Yudhistira dan IP Werkudara)
Nakula: nano /etc/resolv.conf
(arahkan ke dua IP tersebut:
![Screenshot 2023-10-11 203941](https://github.com/yogs14/Jarkom-Modul-2-E04-2023/assets/121499055/37c8a93d-0054-4721-b9b8-173cb5667e13)
)
(Matikan bind9 pada Yudhistira)
Yudhistira: service bind9 stop
Nakula: ping abimanyu.e04.com
![Screenshot 2023-10-11 204130](https://github.com/yogs14/Jarkom-Modul-2-E04-2023/assets/121499055/7fbf3975-d132-4590-8f7d-3e712185633d)
