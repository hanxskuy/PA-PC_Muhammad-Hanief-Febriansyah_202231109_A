
# Penjelasan Deteksi Tepi

## Langkah - Langkah Pengerjaan

### 1. Import Library
```bash
import numpy as np 
import cv2 
import matplotlib.pyplot as plt
%matplotlib inline

import skimage
```
Terdapat 5 library yang dipakai pada pengerjaan project ini yaitu :
- import numpy as np: Mengimpor library NumPy dan memberi alias np. NumPy adalah library untuk komputasi numerik di Python, yang menyediakan dukungan untuk array besar dan fungsi matematika yang efisien.
- import cv2: Mengimpor library OpenCV, yang merupakan library open-source untuk pemrosesan gambar dan video.
- import matplotlib.pyplot as plt: Mengimpor modul pyplot dari library Matplotlib dan memberi alias plt. pyplot digunakan untuk membuat visualisasi grafik.
- %matplotlib inline: Magic function di Jupyter Notebook yang memungkinkan grafik Matplotlib ditampilkan langsung dalam notebook.
- import skimage: Mengimpor library scikit-image, yang merupakan library untuk pemrosesan gambar dalam Python.

### 2. Membaca dan Menampilkan gambar
#### Memuat Gambar dengan OpenCV
```bash
hanip = cv2.imread("diri.jpg")
```
- cv2.imread("diri.jpg"): Fungsi imread dari OpenCV digunakan untuk membaca gambar dari file. Dalam hal ini, gambar yang akan dibaca adalah "diri.jpg".
- Hasil dari cv2.imread disimpan dalam variabel hanip. Gambar ini akan disimpan sebagai array NumPy dengan tiga saluran warna (BGR - Blue, Green, Red).

#### Menampilkan Gambar dengan OpenCV
```bash
cv2.imshow("gambar hanip", hanip)
```
- cv2.imshow("gambar hanip", hanip): Fungsi imshow digunakan untuk menampilkan gambar dalam jendela. Parameter pertama adalah nama jendela ("gambar hanip") dan parameter kedua adalah variabel yang berisi gambar (hanip).
- Jendela baru akan muncul dengan gambar yang dimuat dan ditampilkan di dalamnya.

#### Menunggu Input dari Keyboard
```bash
cv2.waitKey(0)
```
- cv2.waitKey(0): Fungsi waitKey digunakan untuk menunggu input dari keyboard. Angka 0 berarti menunggu tanpa batas waktu sampai ada tombol yang ditekan. Jika diberikan nilai lain (misalnya 1000), itu akan menunggu selama 1000 milidetik (1 detik).
- Program akan berhenti di baris ini dan menunggu pengguna menekan tombol apapun pada keyboard. Setelah tombol ditekan, program akan melanjutkan eksekusi ke baris berikutnya.

#### Menutup Semua Jendela OpenCV
```bash
cv2.destroyAllWindows()
```
- cv2.destroyAllWindows(): Fungsi destroyAllWindows digunakan untuk menutup semua jendela yang dibuat oleh OpenCV.
- Setelah tombol ditekan dan cv2.waitKey(0) selesai, fungsi ini akan dipanggil untuk menutup jendela yang menampilkan gambar.

### 3. Mengonversi Gambar ke Grayscale dan Deteksi Tepi dengan Algoritma Canny

#### Konversi Gambar ke Grayscale
```bash
gray = cv2.cvtColor(hanip, cv2.COLOR_BGR2GRAY)
```
- cv2.cvtColor: Fungsi dari OpenCV yang digunakan untuk mengubah ruang warna gambar.
- hanip: Variabel yang berisi gambar asli dalam format BGR (Blue, Green, Red).
- cv2.COLOR_BGR2GRAY: Parameter yang menentukan bahwa konversi yang diinginkan adalah dari BGR ke Grayscale.
- gray: Variabel baru yang akan menyimpan gambar yang telah dikonversi ke grayscale.
#### Deteksi Tepi dengan Algoritma Canny
```bash
edges = cv2.Canny(hanip, 100, 150)
```
- cv2.Canny: Fungsi dari OpenCV yang digunakan untuk melakukan deteksi tepi menggunakan algoritma Canny.
- hanip: Variabel yang berisi gambar asli dalam format BGR.
- 100: Ambang batas bawah untuk deteksi tepi.
- 150: Ambang batas atas untuk deteksi tepi.
- edges: Variabel baru yang akan menyimpan hasil gambar yang telah melalui deteksi tepi.

### 4. Perbandingan Gambar hasil dan hasil deteksi Canny
#### Membuat Subplot
```bash
fig, axs = plt.subplots(1, 2, figsize=(10, 10))
```
- plt.subplots(1, 2, figsize=(10, 10)): Membuat sebuah figure dan array dari subplots.
    - 1, 2: Menentukan bahwa kita ingin membuat 1 baris dan 2 kolom subplots.
    - figsize=(10, 10): Menentukan ukuran keseluruhan figure (10x10 inci).
- fig: Objek figure yang mewakili keseluruhan kanvas tempat subplot akan digambar.
- axs: Array dari objek subplot yang dibuat, dalam hal ini akan ada dua subplot.
#### Meratakan Array Subplots
```bash
ax = axs.ravel()
```
- axs.ravel(): Meratakan array subplot menjadi array 1D. Ini berguna ketika ingin mengakses subplot dengan cara yang lebih mudah.
#### Menampilkan Gambar Grayscale di Subplot Pertama
```bash
axs[0].imshow(gray, cmap="gray")
axs[0].set_title("Original Image")
```
- axs[0]: Mengakses subplot pertama (kiri).
- imshow(gray, cmap="gray"): Menampilkan gambar gray di subplot pertama dengan colormap gray.
    - gray: Variabel yang menyimpan gambar grayscale.
    - cmap="gray": Menentukan bahwa gambar akan ditampilkan dalam skala abu-abu.
- set_title("Original Image"): Menetapkan judul "Original Image" untuk subplot pertama.
#### Menampilkan Gambar Hasil Deteksi Tepi di Subplot Kedua
```bash
axs[1].imshow(edges, cmap="gray")
axs[1].set_title("Canny Edge Detection")
```
- axs[1]: Mengakses subplot kedua (kanan).
- imshow(edges, cmap="gray"): Menampilkan gambar edges di subplot kedua dengan colormap gray.
    - edges: Variabel yang menyimpan hasil deteksi tepi dengan algoritma Canny.
    - cmap="gray": Menentukan bahwa gambar akan ditampilkan dalam skala abu-abu.
- set_title("Canny Edge Detection"): Menetapkan judul "Canny Edge Detection" untuk subplot kedua.

### 5. Menemukan dan Menggambar countour pada gambar
#### Menemukan Kontur pada Gambar
```bash
contours, hierarchy = cv2.findContours(edges, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
```
- cv2.findContours(edges, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE): Fungsi ini digunakan untuk menemukan kontur dalam gambar biner.
    - edges: Gambar biner hasil dari deteksi tepi menggunakan algoritma Canny.
    - cv2.RETR_TREE: Mode retrieval kontur. RETR_TREE mengambil semua kontur dan membuat hierarki penuh dari kontur-kontur yang ditemukan.
    - cv2.CHAIN_APPROX_SIMPLE: Metode aproksimasi kontur. CHAIN_APPROX_SIMPLE menghapus semua titik redundan dan hanya menyimpan titik penting yang membentuk kontur.
- contours: List dari kontur yang ditemukan. Setiap kontur adalah array dari titik-titik (koordinat) yang membentuk kontur tersebut.
- hierarchy: Informasi tentang hierarki kontur. Ini berguna jika Anda ingin mengetahui hubungan antara kontur, seperti kontur yang ada di dalam kontur lain.
#### Menggambar Kontur pada Gambar Asli
```bash
contours_image = cv2.drawContours(hanip.copy(), contours, -1, (0, 255, 0), 2)
```
- cv2.drawContours(hanip.copy(), contours, -1, (0, 255, 0), 2): Fungsi ini digunakan untuk menggambar kontur pada gambar.
    - hanip.copy(): Membuat salinan dari gambar asli hanip agar gambar asli tidak diubah.
    - contours: List dari kontur yang ditemukan yang akan digambar.
    - -1: Mengindikasikan bahwa semua kontur akan digambar. Jika Anda ingin menggambar kontur tertentu, Anda bisa mengganti -1 dengan indeks kontur yang diinginkan.
    - (0, 255, 0): Warna kontur yang akan digambar (dalam format BGR). Dalam hal ini, kontur akan digambar dengan warna hijau.
    - 2: Ketebalan garis kontur yang digambar.

### 5. Perbandingan Gambar hasil, hasil deteksi Canny dan hasil deteksi countour
#### Membuat Subplot
```bash
fig, axs = plt.subplots(1, 3, figsize=(10, 10))
```
- plt.subplots(1, 3, figsize=(10, 10)): Membuat sebuah figure dan array dari subplots.
    - 1, 3: Menentukan bahwa kita ingin membuat 1 baris dan 3 kolom subplots.
    - figsize=(10, 10): Menentukan ukuran keseluruhan figure (10x10 inci).
- fig: Objek figure yang mewakili keseluruhan kanvas tempat subplot akan digambar.
- axs: Array dari objek subplot yang dibuat, dalam hal ini akan ada tiga subplot.
#### Meratakan Array Subplots
```bash
ax = axs.ravel()
```
- axs.ravel(): Meratakan array subplot menjadi array 1D. Ini berguna ketika ingin mengakses subplot dengan cara yang lebih mudah.
#### Menampilkan Gambar Grayscale di Subplot Pertama
```bash
axs[0].imshow(gray, cmap="gray")
axs[0].set_title("Original Image")
```
- axs[0]: Mengakses subplot pertama (kiri).
- imshow(gray, cmap="gray"): Menampilkan gambar gray di subplot pertama dengan colormap gray.
    - gray: Variabel yang menyimpan gambar grayscale.
    - cmap="gray": Menentukan bahwa gambar akan ditampilkan dalam skala abu-abu.
- set_title("Original Image"): Menetapkan judul "Original Image" untuk subplot pertama.
#### Menampilkan Gambar Hasil Deteksi Tepi di Subplot Kedua
```bash
axs[1].imshow(edges, cmap="gray")
axs[1].set_title("Canny Edge Detection")
```
- axs[1]: Mengakses subplot kedua (tengah).
- imshow(edges, cmap="gray"): Menampilkan gambar edges di subplot kedua dengan colormap gray.
    - edges: Variabel yang menyimpan hasil deteksi tepi dengan algoritma Canny.
    - cmap="gray": Menentukan bahwa gambar akan ditampilkan dalam skala abu-abu.
- set_title("Canny Edge Detection"): Menetapkan judul "Canny Edge Detection" untuk subplot kedua.
#### Menampilkan Gambar dengan Kontur di Subplot Ketiga
```bash
axs[2].imshow(contours_image, cmap="gray")
axs[2].set_title("Contours Image")
```
- axs[2]: Mengakses subplot ketiga (kanan).
- imshow(contours_image, cmap="gray"): Menampilkan gambar contours_image di subplot ketiga dengan colormap gray.
    - contours_image: Variabel yang menyimpan gambar dengan kontur yang digambar di atasnya.
    - cmap="gray": Menentukan bahwa gambar akan ditampilkan dalam skala abu-abu (meskipun gambar sebenarnya berwarna, ini masih akan menampilkan gambar dengan benar, tetapi hanya mengabaikan informasi warna).
    - set_title("Contours Image"): Menetapkan judul "Contours Image" untuk subplot ketiga.




    

