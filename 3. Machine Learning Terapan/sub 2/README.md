# Laporan Proyek Machine Learning - Ivan Andrianto

## Project Overview

Buku merupakan salah satu sumber informasi yang sangat diperlukan dalam menambah wawasan serta pengetahuan. Dengan membaca buku dapat menjadikan pribadi seseorang semakin baik dan menurunkan efek negatif seperti kenakalan pada anak dan semisalnya. Namun, sangat sulit dalam menentukan buku yang akan dibaca, baik untuk memulai maupun yang akan dibaca setelahnya. Hal ini dikarenakan banyaknya ketersediaan jumlah buku yang dapat dibaca. Akibatnya, pembaca kemungkinan besar memutuskan membaca buku-buku yang memiliki reputasi penjualan terbaik. Ada pula pembaca yang memutuskan membaca buku yang mirip dengan buku-buku yang pernah dibaca sebelumnya. Tidak jarang juga ditemui pembaca yang memutuskan untuk membaca buku yang memiliki rating terbaik. Semakin tinggi rating dari buku tersebut, semakin tertarik pula pembaca untuk membacanya. Semakin rendah rating dari buku tersebut, maka pembaca cenderung enggan untuk membacanya. Tinggi rendahnya rating tersebut mempengaruhi buku-buku yang akan direkomendasikan. Nilai kemiripan antar buku dan rating buku dapat dijadikan landasan untuk memberikan rekomendasi buku kepada pembaca.

Sistem rekomendasi memberikan solusi terhadap permasalahan dalam menentukan buku yang belum pernah dibaca oleh pengguna. Sistem rekomendasi buku menggunakan metode collaborative filtering diharapkan dapat membantu pembaca buku untuk menentukan buku apa saja yang termasuk dalam preferensi atau selera pembaca dan yang tidak termasuk berdasarkan kemiripan antar buku. Penentuan rekomendasi dengan collaborative filtering ini diambil berdasarkan fitur yang dimiliki sistem yang memungkinkan penggunanya untuk memberikan rating atau nilai terhadap buku-buku yang telah dibaca sebelumnya. Dengan mencari kemiripan antara buku-buku yang pernah dinilai akan didapatkan nilai kemiripan yang dapat digunakan sistem untuk memberikan rekomendasi buku-buku yang belum pernah dinilai oleh pembaca[1].

## Business Understanding
### Problem Statements
Berdasarkan latar belakang yang telah dipaparkan, berikut rincian masalah yang akan disolusikan pada proyek ini:
- Bagaimana cara membuat sistem rekomendasi berdasarkan penilaian buku yang pernah dilakukan?

### Goals
Berdasarkan permasalahan diatas, berikut tujuan dari proyek ini:
- Memberikan informasi dalam bentuk rekomendasi buku yang ditentukan berdasarkan rating dari pembaca.

## Data Understanding
Dataset yang digunakan pada proyek ini dapat di-download melalui [Kaggle](https://www.kaggle.com/arashnic/book-recommendation-dataset). Dataset tersebut memiliki 3 (tiga) file yang merepresentasikan 3 (tiga) buah tabel yaitu `Users`, `Books`, dan `Ratings`.
1. Users
File ini berisikan 3 kolom yang memiliki 278.858  sampel data. Detail dari masing-masing kolom dapat diuraikan sebagai berikut:
    - `User-ID`, merupakan id pembaca.
    - `Location`, merupakan lokasi dari pembaca.
    - `Age`, merupakan usia dari pembaca.
2. Books
File ini berisikan 8 kolom yang memiliki 271.379  sampel data. Detail dari masing-masing kolom dapat diuraikan sebagai berikut:
    - `ISBN`, merupakan kode pengidentifikasian suatu buku.
    - `Book-Title`, merupakan judul dari buku terkait.
    - `Book-Author`, merupakan penulis dari buku terkait.
    - `Year-Of-Publication`, merupakan tahun keluaran dari buku terkait.
    - `Publisher`, merupakan pencetak dari buku terkait.
    - `Image-URL-S`, merupakan link URL dari buku terkait berukuran kecil (small).
    - `Image-URL-M`, merupakan link URL dari buku terkait berukuran kecil (medium).
    - `Image-URL-L`, merupakan link URL dari buku terkait berukuran kecil (large).
3. Ratings
File ini berisikan 3 kolom yang memiliki 1.149.780  sampel data. Detail dari masing-masing kolom dapat diuraikan sebagai berikut:
    - `User-ID`, merupakan id pembaca.
    - `ISBN`, merupakan kode pengidentifikasian suatu buku.
    - `Book-Rating`, merupakan rating dari buku yang dinilai oleh pembaca.

File `Users` lebih mengarah pada penginformasian pembaca, file `Books` lebih mengarah pada detail penginformasian buku, dan file `Ratings` yang menyimpan data rating atau penilaian buku dari pengguna. Pada proyek ini akan menggunakan file `Books` dan `Ratings` di mana rating akan dimanfaatkan sebagai acuan pembuatan model sistem rekomendasi. Barulah kemudian digunakan data buku yang akan ditampilkan sebagai keluaran dari sistem rekomendasi yang dibuat.

## Data Preparation
Pada tahap ini dilakukan beberapa preprocessing untuk memastikan data yang digunakan sudah siap pakai untuk dimodelkan, tahapan yang dilalui diuraikan sebagai berikut:
- Pertama, dilakukan pengecekan data, mulai dari apakah terdapat data yang kosong hingga apakah terdapat tipe data yang tidak sesuai dengan kolom. Pada dataset  terdapat nilai yang kosong dan langsung ditangani pada file `Books`. Kemudian data pada file `Ratings` dilakukan penyaringan untuk mengambil data buku yang memiliki rating `1-10` saja, dan mengabaikan data buku yang memiliki nilai rating `0` dengan asumsi pembaca belum menyelesaikan bacaan atau memang belum memberikan rating. Kemudian mengubah tipe data dari kolom rating tersebut yang semula `int` mejadi `float` karena akan melakukan proses perhitungan normalisasi nantinya.
- Kedua, dilakukan encoding yaitu `Ordinal Transform` terhadap kolom `User-ID` dan `ISBN` yaitu mengubah nilai kategorikal kolom tersebut menjadi nilai bilangan. Penggunaan nilai bilangan inilah yang akan memudahkan proses pembelajaran data nantinya sebagai data fitur atau variabel independen yang dicocokkan sebagai satu value.
- Ketiga, dilakukan proses normalisasi yaitu `Min-Max Normalization` terhadap nilai rating untuk menyamakan skala data di antara nilai `0` dan `1` yang juga mempermudah proses pelatihan data nantinya. Persamaan `Min-Max Normlaization` dapat dilihat pada gambar di bawah ini.
![minmax](https://editor.analyticsvidhya.com/uploads/95312f1.png)
- Keempat, dilakukan pembagian data menjadi data latih dan data uji di mana 80% digunakan sebagai data latih, dan 20% sisanya sebagai data uji.

## Modeling and Result
Setelah melakukan preprocessing maka tahapan modeling data mulai dilakukan. Berikut tahapan dari proses modeling hingga percobaan hasil pada proyek ini dalam membuat sistem rekomendasi:
1. Melakukan teknik embedding
   Tahapan selanjutnya yaitu menghitung skor kemiripan antara data pengguna dan buku menggunakan teknik embedding dengan implementasi kelas `RecommenderNet` dari `keras`.
2. Melakukan proses compile pada model
   Model di-compile menggunakan `binary crossentropy` untuk menghitung loss function, `Adam`  sebagai optimizer, dan `root mean squared error (RMSE)` sebagai metrik evaluasi
3. Mulai training
   Setelah tahapan sebelumnya telah dilakukan, barulah tiba saatnya proses training data dilakukan.
4. Melihat hasil prediksi
   Setelah model sudah terlatih, tentunya ingin dilihat bagaimana hasil rekomendasi yang diberikan oleh model, apakah sudah cukup sesuai atau belum. Dimulai dari mengambil sampel pembaca secara acak, kemudian menyimpan data buku yang pernah dibaca untuk menyaring buku yang belum dibaca untuk direkomendasikan kepada sampel pembaca tersebut. Kemudian dilakukan prediksi dan ditampilan buku yang pernah dibaca dan yang direkomendasikan sebagai pembanding apakah hasil rekomendasi sudah cukup sesuai.

Hasil dari modeling yang dilakukan akan menampilkan 10 hasil rekomendasi kepada pembaca. Berikut tabel hasil yang didapatkan.
> User-ID: 239423

Buku yang telah dibaca  | Buku yang direkomendasikan
------------- | -------------
Harry Potter and the Sorcerer's Stone (Book 1) | Harry Potter and the Sorcerer's Stone (Harry Potter (Paperback))
Pope Joan (Ballantine Reader's Circle) | Harry Potter and the Chamber of Secrets (Book 2)
Emily of New Moon | To Kill a Mockingbird
Poodles: A Book of Postcards | Girl with a Pearl Earring
The Onion Girl (Newford) | The Da Vinci Code
Prophecy: Child of Earth | The Red Tent (Bestselling Backlist)
Vincalis the Agitator | The Lovely Bones: A Novel
The Gates of Sleep | The Secret Life of Bees
Thendara House | Where the Heart Is (Oprah's Book Club (Paperback))
Emily's Quest | The Joy Luck Club

## Evaluation
Metrik evaluasi yang akan digunakan dalam mengukur performa dari model pada proyek ini adalah Root Mean Squared Error (RMSE). Persamaannya dapat dilihat pada gambar di bawah ini.
![rmse](https://www.gstatic.com/education/formulas2/397133473/en/root_mean_square_deviation.svg)

Berikut adalah hasil evaluasi model menggunakan metrik RMSE.
![hasil](https://i.ibb.co/WVpcQ1C/download.png)

Nilai error pada data latih dan validasi model memiliki nilai yang relatif sama. Bahkan pada iterasi terakhir memiliki nilai error yang persis sama yaitu `0.2246`. Nilai tersebut cukup baik untuk pemodelan sistem rekomendasi. Hasil rekomendasi yang didapatkan pun sesuai sehingga dapat dikatakan model telah berjalan dengan baik.

## Referensi
[1] P. W. W. Andrew Hans Ritdrix, “Sistem rekomendasi buku menggunakan metode item-based collaborative filtering skripsi,” J. Masy. Inform., vol. 9, hal. 24–32, 2018.