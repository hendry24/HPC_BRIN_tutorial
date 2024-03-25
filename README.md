<div align = "center">
<img src="https://i.ibb.co/dDr2KpR/Screenshot-2024-03-25-185451.png" width = "70%">
</div>

# **Menggunakan Layanan MAHAMERU HPC BRIN**

<div align = "justify">
  
BRIN menyediakan layanan menggunakan komputasi performa tinggi (*high performance computing*/HPC) yang dapat Anda akses dari komputer pribadi Anda. Halaman ini dimaksudkan untuk menuntun Anda tentang penggunaan layanan ini secara komprehensif. Harapannya, Anda mengerti dasar-dasar cara kerja HPC dan cara untuk mengoperasikannya sebagai pengguna awam. 

### **DAFTAR ISI**

- [Bagaimana Saya Dapat Mengakses HPC?](#section1)
- [Komunikasi Anda dan Sistem HPC](#section2)
- [Bekerja pada Interactive Compute Node](#section3)

---

## **Bagaimana Saya Dapat Mengakses HPC?**<a name="section1"></a>

Langkah pertama yang harus Anda lakukan adalah mendaftarkan diri sebagai pengguna layanan HPC melalui laman ELSA BRIN. Panduan lengkapnya dapat Anda temukan di [**SINI**](https://elsa.brin.go.id/faq).

Setelah mendaftarkan diri mengikuti arahan tersebut dan status layanan Anda sudah aktif, Anda harus menyimpan informasi berikut yang dapat diperoleh di bagian **Profil > Transaksi > [Nama Layanan HPC]**:

- **USERNAME** yang dapat dilihat pada Tab **Akun** (diterangi kuning pada gambar berikut).

</div>

<div align="center">
<img src = "https://i.ibb.co/4tZyQjR/Screenshot-2024-03-25-163055.png" width = "90%">
</div>

<div align = "justify">
  
Komputer pribadi Anda dapat terhubung ke sistem HPC melalui sistem [**SSH**](https://www.cloudflare.com/learning/access-management/what-is-ssh/) dengan kunci yang Anda gunakan saat pendaftaran layanan. Oleh karena itu, sebaiknya Anda menggunakan Terminal yang sama dengan yang Anda gunakan untuk membuat kunci privat (dengan perintah ``ssh-keygen``) yang didaftarkan ke ELSA.

Untuk masuk ke dalam sistem HPC, buka Terminal Anda dan ketik (tanpa kurung kotak) 

``ssh [USERNAME Anda]@login2.hpc.brin.go.id``

Jika berhasil, maka Anda seharusnya melihat

</div>

<div align="center">
<img src="https://i.ibb.co/zXG93jF/Screenshot-2024-03-25-164629.png" width="50%">
</div>

<div align = "justify">
  
Ini menunjukkan bahwa Anda sudah berhasil menyambungkan komputer Anda ke sistem HPC. 

---

## **Komunikasi Anda dan Sistem HPC**<a name="section2"></a>

Mulai dari sini kita asumsikan Terminal yang Anda gunakan menggunakan [**Bash**](https://en.wikipedia.org/wiki/Bash_(Unix_shell)) (contoh: [**Ubuntu**](https://ubuntu.com/download)). Secara umum, skema hubungan komputer Anda dan komputer pada sistem HPC BRIN adalah sebagai berikut.

<div align="center">
<img src="https://i.ibb.co/6PTjYLh/Screenshot-2024-03-25-190915.png" width="90%">
</div>

Mari kita lihat bagian kiri bawah bagan ini. Yang dimaksud dengan "masuk ke dalam sistem HPC", seperti yang dijelaskan pada bagian sebelumnya, adalah menghubungkan Terminal komputer Anda dengan **LOGIN NODE** dari HPC. Anggaplah seperti ada satu komputer khusus dalam sistem HPC yang dapat Anda kendalikan dari jarak jauh dengan sambungan yang sudah Anda buat. 

**Anda TIDAK DIIZINKAN untuk menjalankan perhitungan di LOGIN NODE**

Jika Anda menjalankan perhitungan di **LOGIN NODE**, tidak akan lama sebelum perhitungan Anda dihentikan paksa oleh sistem HPC. Walaupun kesannya seperti tidak mengapa melakukan perhitungan yang cepat, sebaiknya Anda tidak melakukan hal ini karena mengganggu kerja sistem HPC. **LOGIN NODE** hanya sebatas tempat Anda melakukan hal-hal berikut:

- Memanggil modul-modul yang disediakan HPC untuk digunakan dalam perhitungan (misalnya ``Anaconda``) dan melakukan berbagai pengaturan dengan modul-modul tersebut (misalnya menggunakan Anaconda untuk memasang modul ``scikit-learn``).
- Mengurus file.
- Menghubungkan komputer Anda ke **INTERACTIVE COMPUTE NODE** atau mengirim pekerjaan ke **BATCH COMPUTE NODE**.

Berangkat dari sini, mari kita lihat bagan di atas secara menyeluruh. Tujuan utama menggunakan HPC adalah untuk melakukan perhitungan yang memakan sumber daya, yang akan memakan waktu lama dan membebani perangkat keras komputer Anda, secara lebih cepat. [**SLURM**](https://en.wikipedia.org/wiki/Slurm_Workload_Manager) adalah sistem manajemen sumber daya yang digunakan oleh HPC. SLURM mengurus permintaan Anda kepada sistem HPC untuk melakukan perhitungan tertentu pada **COMPUTE NODE**, yakni komputer pada sistem HPC yang memang ditujukan untuk melakukan perhitungan. 

Ada dua cara untuk melakukan perhitungan pada HPC. 

---

## Bekerja pada Interactive Compute Node <a name="section3"></a>

Dalam moda **INTERACTIVE**, Anda membawa Terminal Anda lebih jauh ke dalam sistem HPC dan mengendalikan **INTERACTIVE COMPUTE MODE** melalui Terminal Anda. Anggaplah Anda mengendalikan sebuah komputer perhitungan dari jauh, dengan komputer **LOGIN NODE**, yang juga Anda kendalikan dari jauh.

Berbeda dengan **LOGIN NODE**, saat menyambungkan komputer Anda pada **INTERACTIVE COMPUTE MODE**, Anda harus meminta "jatah berhitung" pada HPC melalui SLURM. 

</div>
