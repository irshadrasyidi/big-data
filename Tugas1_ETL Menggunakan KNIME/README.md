# Dokumentasi Tugas 1 ETL Menggunakan KNIME
## Dataset Suicide Rates 1985 - 2016

### Business Understanding
* Dataset berikut dapat dianalisis secara mendalam untuk mengetahui negara/benua mana yang memiliki kasus bunuh diri tinggi dan rendah, serta fluktuasi jumlah kasus dari tahun ke tahun.
* Selain dari letak geografis, analisis demografis bisa juga dilakukan untuk mengatahui gender mana yang lebih berpotensi bunuh diri di suatu negara, serta umur dari korban bunuh diri.
* Lebih dalam lagi, analisis kasus bunuh diri dapat dihubungkan dengan pengaruh tingkat ekonomi dan kemakmuran dari suatu negara pada tahun tertentu, karena dataset telah menyediakan kolom data GDP dan HDI.
* Dari hasil analisis dari variabel-variabel di atas, analis dapat memperkirakan strategi yang cocok untuk membantu pencegahan dan mengurangi terjadinya kasus bunuh diri, dari aspek ekonomi atau kemakmuran di berbagai negara, serta target sosialisasi yang tepat berdasarkan hasil pengolahan data demografis negaranya.

Berikut adalah contoh dari pemrosesan data yang lalu divisualisasikan dengan baik sehingga mudah dimengerti oleh orang awam.

![SS BU 1](https://github.com/irshadrasyidi/big-data/blob/master/Tugas1_ETL%20Menggunakan%20KNIME/images/bu1.png)
![SS BU 2](https://github.com/irshadrasyidi/big-data/blob/master/Tugas1_ETL%20Menggunakan%20KNIME/images/bu2.png)
![SS BU 3](https://github.com/irshadrasyidi/big-data/blob/master/Tugas1_ETL%20Menggunakan%20KNIME/images/bu3.png)

Sumber : (https://www.kaggle.com/tavoosi/suicide-data-full-interactive-dashboard)

### Data Understanding
* country : Negara korban bunuh diri
* year : Tahun terjadinya kasus bunuh diri
* sex : Gender korban bunuh diri
* age : Range umur korban bunuh diri
* suicides_no : Jumlah kasus bunuh diri yang terjadi pada periode dan kategori tersebut
* population : Jumlah populasi penduduk di negara tersebut
* suicides/100k pop : Jumlah kasus bunuh diri per 100000 populasi
* country-year : Sandingan negara dan tahun kasus bunuh diri terjadi
* HDI for year : Human Development Index negara pada tahun tersebut
* gdp_for_year : Gross Domestic Product negara pada tahun tersebut
* gdp_per_capita : GDP per Kapita negara pada tahun tersebut
* generation : Generasi korban bunuh diri

**CATATAN PENTING**
* Setiap row dalam dataset asli **tidak** merepresentasikan satu kasus bunuh diri, tetapi jumlah kasus bunuh diri pada parameter kategori tersebut.
* Contoh :
> Albania,1995,male,15-24 years,11,241200,4.56,Albania1995,0.619,"2,424,499,009",835,Generation X
  * Row di atas berarti di negara Albania pada tahun 1995 ada 11 orang laki-laki dari generasi X berumur 15-24 tahun melakukan bunuh diri.
  * Di tahun itu, Albania memiliki populasi 241200 orang, sehingga *Suicide Rate* atau kasus bunuh diri/100000 orang senilai 4.56
  * Di tahun itu juga, Albania memiliki nilai HDI 0.619, GDP 2,424,499,009, dan GDP per capita-nya 835
  * HDI adalah Index Pembangunan Manusia (IPM), adalah pengukuran perbandingan dari harapan hidup, melek huruf, pendidikan dan standar hidup di suatu negara. (https://id.wikipedia.org/wiki/Indeks_Pembangunan_Manusia)
  * GDP dan GDP per kapita menunjukan tingkat perekonomian suatu negara. (https://www.investopedia.com/terms/g/gdp.asp)
  * HDI dan GDP dapat merepresentasikan tingkat kemakmuran suatu negara pada tahun tertentu, masuk kategori negara maju atau negara berkembang

### Data Preparation, Modeling, Evaluation, dan Deployment
**Split Data**
![SS split1](https://github.com/irshadrasyidi/big-data/blob/master/Tugas1_ETL%20Menggunakan%20KNIME/images/split1.png)
* Berikut adalah gambaran besar jalannya proses split data.
* Pada kasus ini, penulis akan membagi data menjadi 2 tabel, yaitu tabel berisi data korban male, dan tabel berisi data korban female.
* Pertama, baca dataset asli berupa CSV dengan node **CSV Reader** dengan memilih file CSV-nya melalui konfigurasi node tersebut
* Untuk proses split, digunakan node **Row Splitter**
![SS split2](https://github.com/irshadrasyidi/big-data/blob/master/Tugas1_ETL%20Menggunakan%20KNIME/images/split2.png)
* Di konfigurasi **Row Splitter**, isi *column to test* dengan opsi **sex**, lalu untuk *mathing criteria*-nya diisi **male**
* Siapkan node **DB Connector** dan **DB Writer**
* **DB Connector** bertugas membuat koneksi ke Database
![SS split3](https://github.com/irshadrasyidi/big-data/blob/master/Tugas1_ETL%20Menggunakan%20KNIME/images/split3.png)
* Set Database Type ke MySQL, karena penulis menggunakan MySQL dari XAMPP
* Masukkan URL Database dengan localhost, port, dan nama databasenya
* Jangan lupa untuk menjalankan XAMPP-nya dulu
* **DB Writer** bertugas untuk menulis hasil split ke dalam database
* Hubungkan koneksi **DB Connector** (kotak merah) ke **DB Writer**, dan output **Row Splitter** (panah) ke input **DB Writer**
* Beri nama tabel yang dinginkan di konfigurasi **DB Writer**
* Setelah dijalankan, tabel akan terbentuk di dalam Database (lihat melalui PhpMyAdmin)
![SS split4](https://github.com/irshadrasyidi/big-data/blob/master/Tugas1_ETL%20Menggunakan%20KNIME/images/split4.png)
* Lalu, buat node **Excel Writer** untuk menyimpan sebagian data satunya
* Pada konfigurasinya, cukup pilih letak file Excel akan terbuat
![SS split5](https://github.com/irshadrasyidi/big-data/blob/master/Tugas1_ETL%20Menggunakan%20KNIME/images/split5.png)
> Hasil split data akan menghasilkan 13910 baris, baik di Excel atau di database, sedangkan jumlah baris di data asli adalah 27820
> Hal ini terjadi karena memang perbandingan jumlah kategori male dan female di dataset asli dari Kaggle seimbang, yaitu 50 : 50
![SS 5050]()

**Append Data**
![SS append1]()
* Berikut adalah gambaran besar alur proses Append Data
* Hasil append akan disimpan ke database dan file JSON
* Untuk membaca dari database (data male), dimulai dengan node **DB Connector** dengan konfigurasi sama dengan proses split, karena hanya untuk membuka koneksi
* Selanjutnya, hubungkan **DB Connector** dengan node **DB Table Selector** yang akan memilih tabel mana dari database yang akan diproses, dengan memilih *schema* dan *table* mana yang akan dipilih
![SS append2]()
* Lalu, hubungkan node tersebut dengan **DB Reader** yang akan membaca isi dari tabel tersebut
* Untuk membaca dari Excel (data female), digunakan node **Excel Reader**, dengan cukup memilih file mana yang akan dibaca
* Siapkan node **Concatenate** yang akan menggabungkan output dari **DB Reader** dan **Excel Reader** menjadi satu tabel besar kembali
![SS append3]()
* Input atas dari **Concatenate** akan diletakkan di baris bagian atas tabel, dan input bawahnya akan diletakkan di bagian bawah tabel
* Selanjutnya simpan hasil proses append tadi ke database dan JSON
* Seperti pada proses split, untuk database, gunakan node **DB Writer** dengan input hasil **Concatenate** dan koneksi dari **DB Connector**
![SS append4]()
* Untuk penyimpana ke JSON, hubungkan **Concatenate** ke node **Table to JSON**, untuk mentransformasi bentuk tabel menjadi bentuk JSON
![SS append5]()
* Setelah bentuk JSON siap, maka masukkan outputnya ke node **JSON Writer** untuk disimpan ke sebuah file JSON, konfigurasinya memilih lokasi simpan dan nama file JSON-nya
![SS append6]()
