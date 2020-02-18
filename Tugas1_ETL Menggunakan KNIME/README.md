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

### Data Preparation, Modeling, dan Evaluation
## Splitting Data
![SS split1]()
* Berikut adalah gambaran besar jalannya proses splitting data.
* Pada kasus ini, penulis akan menbagi data menjadi 2 tabel, yaitu tabel berisi data terkait kasus bunuh diri, dan tabel berisi data terkait tingkat kemakmuran negara.
* Pertama, baca dataset asli berupa CSV dengan node **CSV Reader** dengan memilih file CSV-nya melalui konfigurasi node tersebut
* Untuk proses split, digunakan node **Column Splitter**
![SS split2]()
* Di konfigurasi **Column Splitter**, pilah kolom mana saja yang mau dipisah.
* Siapkan node **DB Connector** dan **DB Writer**
* **DB Connector** bertugas membuat koneksi ke Database
![SS split3]()
* Set Database Type ke MySQL, karena penulis menggunakan MySQL dari XAMPP
* Masukkan URL Database dengan localhost, port, dan nama databasenya
* Jangan lupa untuk menjalankan XAMPP-nya dulu
* **DB Writer** bertugas untuk menulis hasil split ke dalam database
* Hubungkan koneksi **DB Connector** (kotak merah) ke **DB Writer**, dan output **Column Splitter** (panah) ke input **DB Writer**
* Beri nama tabel yang dinginkan di konfigurasi **DB Writer**
* Setelah dijalankan, tabel akan terbentuk di dalam Database (lihat melalui PhpMyAdmin)
![SS split4]()
* Lalu, buat node **Excel Writer** untuk menyimpan sebagian data satunya
* Pada konfigurasinya, cukup pilih letak file Excel akan terbuat
![SS split5]()

## Append Data



### Deployment
