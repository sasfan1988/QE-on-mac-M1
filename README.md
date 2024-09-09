# Install Quantum ESPRESSO (QE) v7.2 on Mac M1 Chip

## 1. Install Package Manager
Sebagai langkah awal, kita perlu menginstall package manager untuk MacOS, dalam hal ini kita menggunakan `HOMEBREW` (silakan cek [https://brew.sh](https://brew.sh)).

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Setelah selesai, silakan ketik kedua perintah di bawah ini agar `brew` bisa diakses:

```sh
(echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> $HOME/.zprofile
```

```sh
eval "$(/opt/homebrew/bin/brew shellenv)"
```

## 2. Install Software Pendukung
Sebelum menginstall QE, kita perlu terlebih dahulu menginstall beberapa software pendukung (menggunakan `brew`), diantaranya: `GCC` sebagai compiler, `OpenMPI` untuk paralelisasi, `ScaLAPACK` sebagai library untuk aljabar linier, `wget` untuk mengunduh file dari internet, dan `cmake` untuk instalasi.

```sh
brew install gcc open-mpi scalapack wget cmake
```

## 3. Unduh file instalasi QE
Dengan menggunakan `wget` kita unduh file instalasi QE (di sini kita menggunakan versi 7.2).

```sh
wget https://gitlab.com/QEF/q-e/-/archive/qe-7.2/q-e-qe-7.2.tar.gz
```

## 4. Ekstraksi file instalasi QE
Lakukan ekstrasi (untar) menggunakan perintah `tar` untuk file `q-e-qe-7.2.tar.gz`.

```sh
tar -zxvf q-e-qe-7.2.tar.gz
```
Selesai ekstraksi, kita akan mendapatkan folder `q-e-qe-7.2`.

## 5. Konfigurasi persiapan instalasi QE
Lakukan konfigurasi sebagai persiapan instalasi QE. Caranya, kita hanya perlu mengeksekusi file `configure` di folder `q-e-qe-7.2`

```sh
cd q-e-qe-7.2
./configure
```
Selesai melakukan konfigurasi, kita akan memperoleh file `make.inc` di folder tersebut. 

## 6. Modifikasi file `make.inc`

Setelah melakukan konfigurasi, seharusnya kita dapat langsung melakukan instalasi. **NAMUN, PERLU DIPERHATIKAN** bahwa kita sebaiknya melakukan sedikit modifikasi terlebih dahulu pada file `make.inc`.

Silakan gunakan editor yang biasa digunakan (misalkan `vi`, `vim`, `emacs`, `nano`, dsb). Pada tutorial ini, kita akan gunakan `nano`.

```sh
nano make.inc
```

Ubah `CPP = cpp` menjadi `CPP = gcc -E`. Caranya, setelah file `make.inc` terbuka, lompat ke baris 107 dengan mengetikkan `ctrl+w+t`, ketik `107`, dan tekan `enter`. Lakukan perubahan seperti yang diperintahkan. Simpan dengan perintah `ctrl+o`, dan keluar dari editor dengan perintah `ctrl+x`. 

## 7. Instalasi QE

Setelah memodifikasi `make.inc`, lakukan instalasi dengan perintah berikut:

```sh
make all
```

Proses ini memakan waktu yang cukup lama, silakan menunggu hingga prosesnya selesai dengan baik.

## 8. Setup BASH
Agar bisa menggunakan QE, kita perlu melakukan beberapa penyesuaian terhadap setup BASH.

```sh
export OMP_NUM_THREADS=1
export PATH=$HOME/q-e-qe-7.2/bin:$PATH
```

## 9. Test
Instalasi selesai, silakan lakukan test dengan menjalankan perintah berikut:

```sh
pw.x
```

Apabila QE telah terinstall dengan baik, maka kita akan mendapatkan tampilan seperti berikut:
```
     This program is part of the open-source Quantum ESPRESSO suite
     for quantum simulation of materials; please cite
         "P. Giannozzi et al., J. Phys.:Condens. Matter 21 395502 (2009);
         "P. Giannozzi et al., J. Phys.:Condens. Matter 29 465901 (2017);
         "P. Giannozzi et al., J. Chem. Phys. 152 154105 (2020);
          URL http://www.quantum-espresso.org",
     in publications or presentations arising from this work. More details at
     http://www.quantum-espresso.org/quote
     Parallel version (MPI), running on     1 processors
     MPI processes distributed on     1 nodes
     0 MiB available memory on the printing compute node when the environment starts

     Waiting for input...
```
