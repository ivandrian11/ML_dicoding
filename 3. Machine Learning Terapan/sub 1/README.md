# Laporan Proyek Machine Learning - Ivan Andrianto

## Domain Proyek

Domain proyek yang dipilih dalam proyek Machine Learning ini adalah mengenai keuangan dengan judul proyek "**Prediksi Biaya Asuransi Kesehatan**".

Tidak ada satupun yang benar-benar kebal dari suatu penyakit. Sebab, tidak ada yang bisa menjamin kesehatan seseorang, baik yang sudah tua ataupun yang masih muda, pasti tidak akan lepas dari yang namanya ancaman penyakit. Untuk itu, asuransi kesehatan hadir sebagai penyokong dalam mempersiapkan hal-hal yang tidak diinginkan pada masa mendatang. Asuransi kesehatan hadir sebagai penanggung biaya medis dan perawatan kesehatan lainnya. Sebagian konsumen menganggap nominal yang dibayarkan terkait asuransi kesehatan terlampau cukup besar. Namun, sebagian konsumen tersebut tidak memikirkan manfaat yang bisa didapatkan ketika mendaftar asuransi kesehatan. Hal tersebut memang tidak mengherankan, mengingat cara menjual produk asuransi sedikit berbeda dengan produk lainnya. Produk asuransi menjual risiko, di mana hal tersebut umumnya belum dialami oleh konsumen. Tentu saja, banyak konsumen yang masih ragu dan lebih memilih mengeluarkan uang mereka untuk kebutuhan yang lain.

Biaya asuransi kesehatan dari masing-masing individu tentunya berbeda-beda, tergantung dari berbagai faktor dari individu tersebut, salah satunya adalah riwayat penyakit, misal apakah individu tersebut mengalami obesitas atau merupakan perokok aktif. Machine learning merupakan sebuah teknik yang mempelajari kumpulan data dan menghasilkan aturan untuk dapat mengenali atau memprediksi data baru yang belum dipelajari. Adapun suatu metode yang umum digunakan dalam pengoperasian Machine Learning adalah regresi. Regresi merupakan suatu metode analisis statistik yang digunakan agar dapat melihat pengaruh antara dua variabel atau lebih, yang mengembalikan nilai kontinyu pada prosesnya. Dalam proyek ini akan dibuat sebuah model Machine Learning menggunakan regresi untuk memprediksi biaya asuransi kesehatan berdasar berbagai faktor dari individu. Dengan adanya model Machine Learning ini diharapkan dapat membantu masyrakat untuk mengetahui perkiraan nominal biaya yang perlu dikeluarkan mereka ketika ingin mendaftar biaya asuransi kesehatan.

## Business Understanding

### Problem Statements

Berdasarkan latar belakang yang sudah dipaparkan sebelumnya, berikut rincian masalah yang dapat diselesaikan dalam proyek ini:
- Apa saja faktor yang paling memengaruhi biaya asuransi kesehatan dari masing-masing individu?
- Bagaimana cara membuat model Machine Learning dengan mengimplementasikan metode regresi untuk memprediksikan biaya asuransi kesehatan?

### Goals

Tujuan dari pembuatan proyek ini adalah sebagai berikut:
- Mendapatkan pemahaman mengenai faktor yang paling memengaruhi biaya asuransi kesehatan.
- Membuat beberapa model Machine Learning yang dapat mengimplementasikan kasus regresi dalam memprediksikan biaya asuransi kesehatan.

### Solution statements
Berikut merupakan tahapan penting yang dilakukan dalam proyek ini:
- Menghilangkan data yang kembar.
- Mengatasi outlier dengan IQR Method.
- Melakukan encoding dengan Ordinal Encoder dan One Hot Encoding.
- Membagi data train dan test dengan porsi 90% untuk training dan 10% untuk testing.
- Melakukan standardisasi data terhadap data numerik murni.
- Melakukan modeling dengan algoritma K-Nearest Neighbor, Random Forest, Linear Regression, dan Support Vector Machine. Di mana untuk KNN sendiri dilakukan eksplorasi hyperparameter tuning pada nilai tetangganya. Model Random RF pada nilai kedalamannya, LR secara default, dan SVM dieksplorasi menggunakan teknik Grid Search.
- Dilakukan evaluasi model menggunakan Mean Squared Error (MSE).

## Data Understanding
Dataset yang digunakan pada proyek ini dapat di-download melalui [Kaggle](https://www.kaggle.com/mirichoi0218/insurance). Dataset berisikan 7 kolom yang memiliki 1338 sampel data. Detail dari masing-masing kolom dapat diuraikan sebagai berikut:
Atribut  | Keterangan
------------- | -------------
age | usia dari individu.
sex | jenis kelamin dari individu.
bmi | body mass index, memberikan pemahaman mengenai tubuh, apakah berat badan dari individu relatif tinggi atau rendah relatif terhadap tinggi badan.
children | jumlah anak yang ditanggung.
smoker | apakah individu merokok atau tidak.
region | daerah perumahan dari individu.
charges | biaya asuransi kesehatan.

Teknik EDA yang dilakukan:
Gambar  | Keterangan
------------- | -------------
![box plot](https://i.ibb.co/j3r0dF6/download.png) | Dilakukan visualisasi dengan box plot untuk melihat apakah terdapat outlier pada data untuk ditangani. Pada gambar terlihat masih terdapat outlier dan ditangani menggunakan IQR Method.
![bar plot](https://i.ibb.co/hsm2z4K/download.png) | Dilakukan visualisasi dengan diagram batang untuk melihat representasi data pada kolom kategorikal.
![histo](https://i.ibb.co/CJXBtX9/download.png) | Dilakukan visualisasi dengan histogram untuk melihat representasi kemunculan data pada kolom numerikal.
![corr](https://i.ibb.co/r2JMz9r/download.png) | Dilakukan visualisasi dengan heatmap untuk melihat korelasi antar kolom numerikal untuk melihat hubungan variabel manakah yang paling dominan. Disini berfokus pada variabel dependen yaitu `charges`, variabel `age` memiliki nilai korelasi yang tertinggi walau tidak sangat kuat hubungannya.

## Data Preparation
Pada tahap ini dilakukan preprocessing untuk menghasilkan data yang sudah siap pakai untuk dimodelkan, tahapan yang dilalui diuraikan sebagai berikut:
- Pertama dilakukan pengecekan data, mulai dari apakah terdapat data yang kosong, apakah terdapat tipe data yang tidak sesuai dengan kolom, apakah terdapat data yang kembar Pada dataset tidak memiliki data yang kosong, tipe datanya sudah sesuai, tetapi terdapat 1 (satu) data yang kembar dan akan dihapus.
- Kedua, dilakukan pengecekan outlier dengan memvisualisasikan data dengan box plot. Karena masih terdapat outlier maka ditangani menggunakan IQR Method. IQR merupakan interquartile yang dapat diformulasikan `Q3 - Q1`. Untuk mendeteksi pencilan langkah pertama menghitung `IQR * 1,5`. Kemudian tentukan batas bawah dan batas atas dengan cara, `batas bawah = Q1 - 1.5 * IQR` dan `batas atas = Q3 + 1.5 * IQR`. Maka data yang bukan outlier adalah data yang berapa pada rentang batas bawah hingga batas atas.
- Ketiga, dilakukan transformasi terhadap kolom kategorikal (biasa disebut **encoding**) menggunakan Ordinal Encoder pada kolom `sex` dan `smoker` karena kedua kolom tersebut hanya memiliki 2 (dua) kemungkinan nilai, serta One Hot Encoding pada kolom `region` yang memiliki 4 (empat) kemungkinan nilai.
- Keempat, dilakukan pembagian data menjadi data latih dan data uji di mana 90% digunakan sebagai data latih, dan 10% sisanya sebagai data uji.
- Kelima, dilakukan transformasi terhadap kolom numerikal dengan proses standardisasi untuk menyamakan skala data sehingga algoritma Machine Learning dapat memiliki performa lebih baik dan konvergen lebih cepat ketika dimodelkan.

## Modeling
Setelah melakukan tahapan preprocessing maka data telah siap digunakan untuk pemodelan. Pada proyek ini dilakukan pembuatan model menggunakan 4 (empat) algoritma yang berbeda yaitu K-Nearest Neighbor, Random Forest, Linear Regression, dan Support Vector Machine. Rincian pemodelan yang dilakukan sebagai berikut:
- Untuk algoritma KNN, digunakan pengujian dengan eksplorasi hyperparameter tuning yaitu `n_neighbors` dengan nilai tetangga dalam rentang 1-50. Model KNN terbaik dalam pengujian tersebut yaitu yang memiliki nilai error terkecil (menggunakan MSE) akan disimpan untuk perbandingan pada tahap evaluasi.
- Untuk algoritma RF, digunakan pengujian dengan eksplorasi hyperparameter tuning yaitu `max_depth` dengan nilai tetangga dalam rentang 1-20. Model RF terbaik dalam pengujian tersebut yaitu yang memiliki nilai error terkecil (menggunakan MSE) akan disimpan untuk perbandingan pada tahap evaluasi.
- Untuk algoritma LR, digunakan hyperparameter tuning secara default-nya.
- Untuk algoritma SVM, digunakan eksplorasi hyperparameter tuning yaitu `C` dan `gamma` menggunakan teknik Grid Search.

## Evaluation
Pada proyek ini, model dibuat untuk memprediksi biaya asuransi kesehatan, metrik evaluasi yang digunakan adalah MSE. MSE atau Mean Squared Error akan menghitung jumlah selisih kuadrat rata-rata nilai sebenarnya dengan nilai prediksi. Persamaan MSE dapat dilihat pada gambar di bawah ini.
![mse](https://d17ivq9b7rppb3.cloudfront.net/original/academy/2021071619431112f1106e20559e77c855cea11d1b1479.jpeg)

Berikut adalah hasil evaluasi model menggunakan metrik MSE.
![evaluasi](https://i.ibb.co/pRZ3YrF/download.png)

Dapat terlihat pada gambar di atas bahwa jumlah error dari keempat model hampir terlihat sama tetapi algoritma RF yang memiliki nilai error terkecil. Sehingga model RF dapat digunakan sebagai pertimbangan untuk memprediksi biaya asuransi kesehatan.