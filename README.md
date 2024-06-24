<div align = "center">
<img src="https://i.ibb.co/dDr2KpR/Screenshot-2024-03-25-185451.png" width = "70%">
</div>

# **Menggunakan Layanan MAHAMERU HPC BRIN**

**_Hendry & Albert Andersen. Pusat Riset Fisika Kuantum (PRFK). Badan Riset dan Inovasi Nasional (BRIN)._**

<div align = "justify">
  
BRIN menyediakan layanan menggunakan komputasi performa tinggi (*high performance computing*/HPC) yang dapat Anda akses dari komputer pribadi Anda. Halaman ini dimaksudkan untuk menuntun Anda tentang penggunaan layanan ini secara komprehensif. Harapannya, Anda mengerti dasar-dasar cara kerja HPC dan cara untuk mengoperasikannya sebagai pengguna awam. 

### **DAFTAR ISI**
<a name="top"></a>
- [**Bagaimana Saya Dapat Mengakses HPC?**](#section1)
- [**Komunikasi Anda dan Sistem HPC**](#section2)
- [**Bekerja dengan Interactive Compute Node**](#section3)
- [**Bekerja dengan Batch Compute Node**](#section4)
- [**Menjalankan Kode ``Python``**](#section5)
    - [Apakah _package_ ``Python`` yang ingin saya gunakan tersedia?](#section5.1)
    - [Demonstrasi singkat: gerak harmonik sederhana](#section5.2)

---

## **Bagaimana Saya Dapat Mengakses HPC?**<a name="section1"></a>

[(Kembali ke daftar isi)](#top)

Langkah pertama yang harus Anda lakukan adalah mendaftarkan diri sebagai pengguna layanan HPC melalui laman ELSA BRIN. Panduan lengkapnya dapat Anda temukan di [**SINI**](https://elsa.brin.go.id/faq).

Setelah mendaftarkan diri mengikuti arahan tersebut dan status layanan Anda sudah aktif, Anda harus menyimpan informasi berikut yang dapat diperoleh di bagian **Profil > Transaksi > [Nama Layanan HPC]**:

- **USERNAME** yang dapat dilihat pada Tab **Akun** (diterangi kuning pada gambar berikut).

</div>

<br>
<div align="center">
<img src = "https://i.ibb.co/4tZyQjR/Screenshot-2024-03-25-163055.png" width = "90%">
</div>
<br>

<div align = "justify">
  
Komputer pribadi Anda dapat terhubung ke sistem HPC melalui sistem [**SSH**](https://www.cloudflare.com/learning/access-management/what-is-ssh/) dengan kunci yang Anda gunakan saat pendaftaran layanan. Oleh karena itu, sebaiknya Anda menggunakan Terminal yang sama dengan yang Anda gunakan untuk membuat kunci privat (dengan perintah ``ssh-keygen``) yang didaftarkan ke ELSA.

Untuk masuk ke dalam sistem HPC, buka Terminal Anda dan ketik (tanpa kurung kotak) 

```
ssh [USERNAME Anda]@login2.hpc.brin.go.id
```

Jika berhasil, maka Anda seharusnya melihat

</div>

<br>
<div align="center">
<img src="https://i.ibb.co/zXG93jF/Screenshot-2024-03-25-164629.png" width="50%">
</div>
<br>

<div align = "justify">
  
Ini menunjukkan bahwa Anda sudah berhasil menyambungkan komputer Anda ke sistem HPC. Untuk memutus sambungan, masukkan perintah ``exit``.

---

## **Komunikasi Anda dan Sistem HPC**<a name="section2"></a>

[(Kembali ke daftar isi)](#top)

Mulai dari sini kita asumsikan Terminal yang Anda gunakan menggunakan [**Bash**](https://en.wikipedia.org/wiki/Bash_(Unix_shell)) (contoh: [**Ubuntu**](https://ubuntu.com/download)). Secara umum, skema hubungan komputer Anda dan komputer pada sistem HPC BRIN adalah sebagai berikut.

<br>
<div align="center">
<img src="https://i.ibb.co/6PTjYLh/Screenshot-2024-03-25-190915.png" width="90%">
</div>
<br>

Mari kita lihat bagian kiri bawah bagan ini. Yang dimaksud dengan "masuk ke dalam sistem HPC", seperti yang dijelaskan pada bagian sebelumnya, adalah menghubungkan Terminal komputer Anda dengan **LOGIN NODE** dari HPC. Anggaplah seperti ada satu komputer khusus dalam sistem HPC yang dapat Anda kendalikan dari jarak jauh dengan sambungan yang sudah Anda buat. 

**Anda TIDAK DIIZINKAN untuk menjalankan perhitungan di LOGIN NODE**

Jika Anda menjalankan perhitungan di **LOGIN NODE**, tidak akan lama sebelum perhitungan Anda dihentikan paksa oleh sistem HPC. Walaupun kesannya seperti tidak mengapa melakukan perhitungan yang cepat, sebaiknya Anda tidak melakukan hal ini karena mengganggu kerja sistem HPC. **LOGIN NODE** hanya sebatas tempat Anda melakukan hal-hal berikut:

- Memanggil modul-modul yang disediakan HPC untuk digunakan dalam perhitungan (misalnya ``Anaconda``) dan melakukan berbagai pengaturan dengan modul-modul tersebut (misalnya menggunakan Anaconda untuk memasang modul ``scikit-learn``).
- Mengurus file. Jangan lupa menyimpan semua file Anda di dalam folder ``_scratch`` untuk mendapat akses memori HPC.
- Menghubungkan komputer Anda ke **INTERACTIVE COMPUTE NODE** atau mengirim pekerjaan ke **BATCH COMPUTE NODE**.

Berangkat dari sini, mari kita lihat bagan di atas secara menyeluruh. Tujuan utama menggunakan HPC adalah untuk melakukan perhitungan yang memakan sumber daya, yang akan memakan waktu lama dan membebani perangkat keras komputer Anda, secara lebih cepat. [**SLURM**](https://en.wikipedia.org/wiki/Slurm_Workload_Manager) adalah sistem manajemen sumber daya yang digunakan oleh HPC. SLURM mengurus permintaan Anda kepada sistem HPC untuk melakukan perhitungan tertentu pada **COMPUTE NODE**, yakni komputer pada sistem HPC yang memang ditujukan untuk melakukan perhitungan. 

Ada dua cara untuk melakukan perhitungan pada HPC. 

---

## **Bekerja dengan Interactive Compute Node** <a name="section3"></a>

[(Kembali ke daftar isi)](#top)

Dalam moda **INTERACTIVE**, Anda membawa Terminal Anda lebih jauh ke dalam sistem HPC dan mengendalikan **INTERACTIVE COMPUTE NODE** melalui Terminal Anda. Anggaplah Anda mengendalikan sebuah komputer perhitungan dari jauh, dengan komputer **LOGIN NODE**, yang juga Anda kendalikan dari jauh.

Untuk menyambungkan Terminal Anda pada **INTERACTIVE COMPUTE NODE**, Anda dapat menjalankan perintah berikut pada **LOGIN NODE**:

```
srun --partition=interactive --pty /bin/bash
```

</div>

<br>
<div align="center">
<img src="https://i.ibb.co/n3zd5YL/Screenshot-2024-03-25-193007.png" width="60%">
</div>
<br>

<div align="justify">

Gambar di atas menunjukkan apa yang seharusnya Anda lihat ketika berhasil tersambung pada **INTERACTIVE COMPUTE NODE**. Anda akan berpindah dari **LOGIN NODE** (``trembesi02`` pada gambar) ke **INTERACTIVE COMPUTE NODE** (``trembesi91`` pada gambar). Di dalam **INTERACTIVE COMPUTE NODE** Anda dapat sebebasnya melakukan perhitungan, selama mematuhi batasan yang berlaku (misalnya jumlah CPU yang digunakan).

Jika perintah ``exit`` diberikan di sini, sambungan Anda akan terputus dari **INTERACTIVE COMPUTE NODE** dan kembali ke **LOGIN NODE**. Jika Anda menutup Terminal, maka semua sambungan ke sistem HPC akan diputus dan perhitungan yang sedang berlangsung pada **INTERACTIVE COMPUTE NODE** akan dihentikan.

---

## **Bekerja dengan Batch Compute Node**<a name="section4"></a>

[(Kembali ke daftar isi)](#top)

Jika perhitungan Anda memakan banyak waktu dan Anda ingin menjalankan semuanya dengan satu perintah, lalu menutup Terminal dan melakukan hal lain, maka **BATCH COMPUTE NODE** sangat berguna bagi Anda. Berbeda dengan moda **INTERACTIVE**, dengan moda **BATCH** Anda hanya mengirimkan tugas dari **LOGIN NODE** kepada **BATCH COMPUTE NODE** untuk dikerjakan. Komputer perhitungan jenis ini tidak peduli dengan apa yang Anda lakukan di **LOGIN NODE**, jadi Anda dapat menutup sambungan Anda ke **LOGIN NODE** tanpa menganggu perhitungan.

Dengan **BATCH COMPUTE NODE**, Anda harus meminta "jatah berhitung" kepada sistem HPC melalui SLURM dengan perintah ``sbatch``. Ini mencakup jumlah CPU yang ingin Anda gunakan, dan berapa lama ingin menggunakannya. Secara umum, Anda dapat mengirimkan sebuah *bash shell script file* (dengan ekstensi ``.sh``) yang dituliskan dengan format berikut:

```
!/bin/bash

#SBATCH --job-name=[Nama pekerjaan Anda sebagai pengenal]
#SBATCH --partition=[Pilih antara short atau medium-small]
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=[Masukkan jumlah CPU yang ingin Anda gunakan. Pastikan tidak melewati batas (biasanya 64)]

ulimit -l  unlimited

[Masukkan rangkaian perintah untuk pekerjaan Anda di sini]

```

(Perhatikan bahwa opsi-opsi pada perintah ``SBATCH`` ditulis tanpa spasi.)

Simpan file ini dengan ekstensi ``.sh``. Kemudian, Anda dapat mengajukan pekerjaan Anda dengan menjalankan perintah

```
sbatch [Nama].sh
```

Mari kita lihat apa saja perintah yang diberikan di dalam file ``.sh`` di atas:

- ``!/bin/bash`` memberi tahu **BATCH COMPUTE NODE** untuk menjalankan file ini dengan ``bash``.
- ``#SBATCH --job-name`` mengatur nama pekerjaan Anda yang nantinya akan muncul dalam antrian. Ini berguna sebagai tanda pengenal dan Anda dapat sebebasnya memberikan nama.
- ``#SBATCH --partition`` mengatur seberapa lama sejumlah CPU pada **BATCH COMPUTE NODE** digunakan oleh Anda. Di sini ``short`` memberi Anda jatah 1x24 jam, sementara ``medium-small`` memberi Anda jatah 3x24 jam. Pastikan memilih waktu yang cukup. Jika waktu Anda habis, maka perhitungan Anda akan diberhentikan oleh HPC dan hasil yang tidak tersimpan akan hilang.
- ``#SBATCH --ntasks`` memberi tahu **BATCH COMPUTE NODE** berapa banyak pekerjaan yang akan dikerjakan dan mengatur alokasi CPU untuk menjalankan semua tugas yang ada. Sebaiknya, perintah ini tidak diganggu.
- ``#SBATCH --cpus-per-task`` mengatur seberapa banyak CPU yang Anda ingin gunakan per pekerjaan. Dengan nilai bawaan ``#SBATCH --ntasks=1``, perintah ini memberi tahu ``BATCH COMPUTE NODE`` jumlah CPU yang dikerahkan untuk satu-satunya pekerjaan Anda.
- ``ulimit -l unlimited`` memberi tahu sistem HPC untuk tidak membatasi memori yang dapat Anda gunakan.

Dengan semua perintah di atas, SLURM akan menandai sebagian dari **BATCH COMPUTE NODE** untuk digunakan oleh Anda dan hanya Anda selama periode penggunaan yang Anda minta. Dengan kata lain, Anda tidak akan berbagi CPU dengan pengguna lain dalam perhitungan Anda. 

Semua perintah ``sbatch [Nama].sh`` yang Anda berikan akan dimasukkan SLURM ke dalam antrian. Jika ada jatah yang kosong dan giliran Anda tiba, maka HPC akan mengalokasikan sebagian **BATCH COMPUTE NODE** yang Anda minta untuk mengerjakan tugas yang Anda kirimkan. 

Anda dapat memeriksa status antrian Anda dengan memberi perintah berikut pada **LOGIN NODE**

```
squeue --me
```

</div>

<br>
<div align="center">
<img src="https://i.ibb.co/jRggmgw/Screenshot-2024-03-25-202832.png" width = "60%">
</div>
<br>

<div align="justify">

Pada gambar di atas sebuah pekerjaan dengan nama ``Nama_Pekerjaan`` baru saja diajukan kepada sistem HPC. 

- ``JOBID`` menunjukkan nomor identifikasi untuk perkerjaan tersebut.
- ``PARTITION`` menunjukkan perintah ``#SBATCH --partition``.
- ``NAME`` menunjukkan perintah ``#SABTCH --job-name``.
- ``USER`` menunjukkan **USERNAME** Anda.
- ``ST`` menunjukkan status antrian Anda. ``R`` berarti pekerjaan Anda sedang dijalankan oleh ``BATCH COMPUTE NODE`` (*running*), sementara ``PD`` berarti antrian Anda masih tertahan (*pending*).
- ``TIME`` menunjukkan sudah seberapa lama pekerjaan Anda berjalan.
- ``NODES`` menunjukkan jumlah ``BATCH COMPUTE NODE`` yang digunakan sistem HPC untuk menjalankan pekerjaan Anda.
- ``NODELIST(REASON)`` menunjukkan ``BATCH COMPUTE NODE`` mana dari HPC yang sedang digunakan untuk menjalankan pekerjaan Anda. Misalnya pekerjaan yang baru saja dikumpulkan pada gambar di atas dijalankan oleh node ``trembesi13``. Jika Anda sedang berada dalam antrian, Anda mungkin akan melihat ``priority`` yang berarti pekerjaan Anda akan segera dijalankan jika jumlah CPU yang Anda minta sudah luang. Jika Anda melebihi batas jatah pekerjaan yang dapat diajukan, maka Anda akan melihat ``(AssocMaxJobsLimit)``.

Untuk membatalkan sebuah tugas, Anda dapat memberikan perintah
```
scancel [JOBID pekerjaan yang ingin dibatalkan]
```

Untuk melihat histori tugas yang pernah Anda kumpulkan, berikan perintah
```
sacct
```

Perintah lainnya yang disediakan SLURM dapat anda baca di [**SINI**](https://slurm.schedmd.com/quickstart.html).

</div>

## **Menjalankan Kode ``Python``**<a name="section5"></a>

[(Kembali ke daftar isi)](#top)

Untuk menjalankan kode ``Python`` pada sistem HPC BRIN, kita perlu memuat beberapa modul terlebih dahulu. _Interpreter_ dari ``Python`` disediakan oleh modul ``Anaconda`` yang dapat kita panggil dengan menulis
```
module load gnu12
module load gcc
module load Anaconda
```
pada bagian perintah file ``.sh`` yang kita punya di bagian sebelumnya (untuk **BATCH COMPUTE NODE**) atau pada terminal **INTERACTIVE COMPUTE NODE**. Modul ``gnu12`` dan ``gcc`` diperlukan oleh interpreter ``Python`` dari ``Anaconda`` untuk melakukan kompilasi kode sebelum dijalankan, jadi ketiganya harus dimuat. 

Selanjutnya, Anda tinggal memanggil file ``Python`` yang ingin Anda jalankan seperti menjalankannya di terminal sendiri. Tuliskan
```
python3 -u [nama_file_python_anda].py
```
tanpa kurung kotak. Pilihan ``-u`` mengatur ``Python`` agar berjalan dalam moda _unbuffered_, yang diperlukan agar output standarnya (misalnya perintah ``print``) bisa dituliskan ke dalam file _logging_ dari SLURM (biasanya ``slurm-[job_ID_anda].out``). Jika Anda menggunakan **INTERACTIVE COMPUTE NODE**, perintah ``-u`` dapat Anda buang. 

### Apakah _package_ ``Python`` yang ingin saya gunakan tersedia?<a name="section5.1"></a>

Anda dapat melihat _package_ bawaan dari ``Anaconda`` pada sistem HPC BRIN dengan perintah
```
ls /mgpfs/shared/apps/app/Anaconda/3-2023.9-0/lib/python3.11/site-packages/
```

Pada umumnya, _package_ yang ingin Anda gunakan mungkin tidak disediakan secara bawaan oleh sistem HPC ketika Anda memanggil ``module load Anaconda``. Dalam kasus awam, _package_ Anda dapat ditemukan di [PyPi](pypi.org) sehingga Anda dapat menggunakan ``pip`` untuk memasang modul Anda dengan perintah 
```
pip install [nama_package]
``` 
tanpa kurung kotak, di **LOGIN NODE**. Jalankan perintah ini seteleh ``module load Anaconda``. 

Anda hanya perlu melakukan ini **satu kali** (kecuali ingin _update_ versi) untuk akun HPC Anda&mdash;_package_ yang Anda install akan tersimpan atas nama akun Anda dan dapat digunakan baik oleh **INTERACTIVE COMPUTE NODE** maupun **BATCH COMPUTE MODE**. 

Beberapa _package_ juga tersedia sebagai modul pada sistem HPC yang dapat Anda muat. Misalnya, Anda dapat memuat ``qutip`` dengan menulis ``module load quantum/qutip`` pada terminal **INTERACTIVE COMPUTE NODE** maupun pada file ``.sh`` untuk **BATCH COMPUTE MODE**. 

Sayangnya, dengan pilihan ini Anda harus memuat modul setiap kali memulai sebuah _job_, dan belum tentu _package_-nya merupakan versi yang Anda mau. Oleh karena itu, kami menyarankan Anda untuk melakukan pemasangan modul pribadi. 

### Apakah saya dapat menggunakan JupyterLab atau Jupyter Notebook dengan SLURM?

Fitur ini belum didukung oleh sistem HPC BRIN. Jadi, Anda harus menggunakan file ``.py`` untuk menjalankan program ``Python``.

### Demonstrasi singkat: getaran harmonik sederhana

Agar lebih inklusif, kami memberikan demonstrasi dengan fisika SMA. Kami akan menggunakan _package_ ``scipy`` yang tersedia secara bawaan, tapi kami akan melakukan instalasi akun pribadi di sini. _Package_ ini memiliki fitur penyelesaian persamaan diferensial biasa yang dapat kita gunakan untuk menyelesaikan permasalahan kita. Persamaan gerak sistem diberikan sebagai berikut:
$$
\ddot{x}+\omega^2x=0
$$
Syarat awal kita berikan $x_0=10$ dan $\dot{x}=0$. Kita ingin luarannya berupa gambar dalam format ``.png`` dan titik data dalam format ``.txt``. 

Kita akan menggunakan **BATCH COMPUTE NODE** dan tidak akan menggunakan **INTERACTIVE COMPUTE NODE** karena sama seperti menjalankan program biasanya. 

Andaikan saja kita belum punya _package_ ``scipy`` untuk digunakan. Kita bisa buka terminal kita di **LOGIN NODE** lalu menulis
```
module load Anaconda
pip install scipy
```
Setelah instalasi selesai, ``scipy`` sudah tersimpan di akun kita dan tidak perlu lagi diinstalasi, kecuali untuk keperluan _update_. 

Kita akan membuat dua file: file ``.sh`` untuk SLURM, dan file ``.py`` yang ingin kita jalankan. File ``.sh`` kita buat sesuai penjelasan di atas:

```
!/bin/bash

#SBATCH --job-name=demo_1_getaran_harmonik_sederhana
#SBATCH --partition=short
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=8

ulimit -l  unlimited

moduel load gnu12
module load gcc
module load Anaconda

python3 -u ./run.py
```

Kita namakan file ini ``run.sh``. Di sini kita menamai nama file ``Python`` yang ingin kita jalankan ``./run.py``. Pastikan nama yang dituliskan dalam file ``.sh`` adalah nama dari file yang ingin Anda jalankan. Tulisan ``./`` di awal menunjukkan posisi relatif dari file ``.py`` terhadap file ``.sh``, yakni di folder yang sama. Jika file ``.py`` Anda terletak di tempat lain, berikan lokasi secara tepat. Anda dapat juga menggunakan posisi absolut. 

Karena ``scipy`` sudah terpasang untuk akun kita, kita tinggal menulis file ``.py``-nya seperti biasa:
```
import numpy as np 
import scipy as sp
import matplotlib.pyplot as plt     # untuk plot gambar

# Keadaan awal
x0 = 10
v0 = 0

# Kita memilih [omega=pi] dan mengambil solusi 
# untuk 10 satuan waktu pertama. 

omega = np.pi

t = np.linspace(0, 10, 1001)

def solve_eq(t, x):
    # Misalkan:
    #   x[0] adalah posisi 
    #   x[1] = dx[0]/dt adalah kecepatan

    return [x[1],               # dx[0]/dt
            -omega**2 * x[0]]   # dx[1]/dt

res = sp.integrate.solve_ivp(solve_eq,
                            t_span = [0, t[-1]],
                            y0 = [x0, v0],
                            dense_output = True)

x = res.sol(t)[0]
# scipy memberikan solusi untuk x dan v, kita ambil 
# yang pertama saja. 

# Buat gambar

plt.plot(t, x)
plt.savefig("out.png")

# Buat file data

to_write = np.array([[t[i], x[i]] 
                    for i in range(len(t))])
with open("out.txt", "w") as f:
    np.savetxt(f, to_write)
```
**Tidak ada** perlakuan tambahan yang diperlukan pada file ``.py`` jika kita ingin menjalankannya menggunakan **BATCH COMPUTE MODE**. Tulis saja seperti biasa. 

Setelah memastikan kode tidak lagi mengalami bug, kita dapat mengirimkan _job_ kepada SLURM:
```
sbatch run.sh 
```
lalu tunggu hingga perhitungannya selesai. Berikut tampilan akhir dari direktori kita:

<br>
<div align="center">
<img src = "https://i.ibb.co.com/7VGHgmj/Selection-001.png" width = "60%">
</div>
<br>

Berikut gambar dan titik datanya:

<br>
<div align="center">
<img src = "https://i.ibb.co.com/CM8WbW4/Selection-003.png" width = "60%">
</div>
<br>
