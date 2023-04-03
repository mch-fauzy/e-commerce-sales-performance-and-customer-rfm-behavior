# Pakistan E-Commerce Sales Performance and Customer RFM Behavior
| Report (PDF) | Dashboard |
|:---:|:---:|
| [Analysis](https://drive.google.com/file/d/1PX9So7JlmAs--c4fCEt6BYwxG5BZbUBk/view?usp=sharing) | [Tableau Story](https://public.tableau.com/views/PakistanE-CommerceSalesPerformanceandCustomerRFMBehaviour/DashboardStory?:language=en-US&:display_count=n&:origin=viz_share_link)


# Table of Contents

[Business Understanding](#Business-Understanding)<br>
[Data Cleaning and Preparation](#Data-Cleaning-and-Preparation)<br>
[Exploratory Data Analysis](#Exploratory-Data-Analysis)<br>
[Conclusion and Recommendation](#Conclusion-and-Recommendation)<br>

# Business Understanding
## Latar Belakang
Perusahaan memiliki data berisi informasi tentang transaksi ecommerce yang dilakukan oleh pelanggan di Pakistan. Data mencakup berbagai kolom seperti "Item ID", "Order Status", "Date of Order", "SKU", "Price", "Quantity", "Grand Total", "Category", "Payment Method" dan "Customer ID“. Perusahaan ingin menggunakan data tersebut untuk menyusun strategi bisnis berdasarkan data.

## Pernyataan Masalah
Perusahaan E-Commerce ingin mengetahui sales performance dan customer behavior. Informasi ini akan membantu perusahaan untuk menentukan strategi dalam meningkatkan penjualan dan kepuasan pelanggan. 

Sebagai Data Analyst, saya bertugas untuk memahami customer behavior dan rekomendasi apa yang dapat diberikan untuk meningkatkan penjualan dan kepuasan pelanggan.

# Data Cleaning and Preparation
Pada tahap ini dilakukan pemahaman dan pembersihan data untuk mempersiapkan data sebelum dilakukan analisa

## Drop Unused Columns
Kolom-kolom berikut akan di drop karena tidak digunakan dalam analisa:<br>
`'Unnamed: 21', 'Unnamed: 22', 'Unnamed: 23', 'Unnamed: 24', 'Unnamed: 25', 'increment_id',
 'sales_commission_code', 'Working Date', 'BI Status', ' MV ', 'Year', 'M-Y', 'FY', 'Month'`

Karena item_id unik pada setiap baris sedangkan sku tidak, maka item_id adalah ID transaksi/order bukan ID barang. 

Berikut adalah kolom yang akan digunakan dalam proses analisis:

* item_id: ID unik transaksi/order
* status: status barang yang dibeli 
* created_at: tanggal order
* sku: stock keeping unit, ID unik barang
* price: harga barang
* qty_ordered: jumlah barang yang dibeli
* grand_total: total harga barang yang dibeli 
* category_name_1: kategori barang
* discount_amount: diskon barang
* payment_method: cara pembayaran
* Customer Since: waktu pertama pelanggan menggunakan platform
* Customer ID: ID pelanggan

## Missing Value Handling
1. Drop Rows with All NaN Values<br>
`df.dropna(axis=0, how='all', inplace=True)`<br>
2. Drop Rows if Customer ID is NaN<br>
`df.dropna(subset=['Customer ID'], axis=0, inplace=True)`<br>
3. Most Frequent Imputation for NaN under 5%<br>
Karena NaN pada categorical data dibawah 5%, maka NaN akan di imputasi dengan nilai yang sering muncul.<br>
`df['sku'].fillna(df['sku'].mode()[0], inplace=True)`<br>
`df['status'].fillna(df['status'].mode()[0], inplace=True)`<br>
`df['category_name_1'].fillna(df['category_name_1'].mode()[0], inplace=True)`<br>

## Data Types Conversion
`df['created_at'] = pd.to_datetime(df['created_at'])`<br>
`df['Customer Since'] = pd.to_datetime(df['Customer Since'])`<br>
`df['item_id'] = df['item_id'].astype('int').astype('str')`<br>
`df['Customer ID'] = df['Customer ID'].astype('int').astype('str')`<br>

## Data Consistency and Anomalies
1. Grouping Common Labels
 Beberapa label di kolom status memiliki kesamaan arti:

* RECEIVED dan CLOSED menjadi COMPLETE
* ORDER_REFUNDED dan REFUND menjadi CANCELED
* PENDING_PAYPAL, PAYMENT_REVIEW, PENDING, HOLDED, EXCHANGE, PAID, dan COD menjadi PROCESSING
* Label "PROCESSING" dan "FRAUD" sangatlah kecil (0.7 dan 0.001%), jadi bisa di drop jika tidak diperlukan dalam analisis 
* EASYPAY_MA dan EASYPAY_VOUCHER menjadi EASYPAY

2. Drop Anomalies Data
Terdapat anomali data pada kolom "status" dan "category_name_1" dengan label "\\N"
* Karena persentasenya sangat kecil (1%), maka baris dengan label "\\N" akan di drop

3. Replace Anomalies Data
Terdapat anomali data pada kolom "grand_total" dan "discount_amount", yaitu nilainya ada yang negatif (harga dan diskon seharusnya tidak negatif)
* Maka akan di filter hanya include grand total dan diskon yang lebih dari 0

## Add New Features (Feature Improvements)
1. Order Quantity Binning <br>
`df['qty_bins'] = pd.cut(`<br>
    `df.qty_ordered,`<br>
    `bins=[0, 1, 2, 3, 4, 5, 10, 20, 100, 1000],`<br>
    `include_lowest=True,`<br>
    `labels=["1", "2", "3", "4", "5", "6-10", "11-20", "21-100", "101-1000"]`<br>
`)`<br>

2. Monthly Period <br>
`df["order_month"] = df['created_at'].dt.to_period("M")` <br>

3. Quarter Period <br>
`df["order_quarter"] = df['created_at'].dt.to_period("Q")` <br>

4. Day of Week <br>
`dow_mapping = {`<br>
    `0: "Monday",`<br>
    `1: "Tuesday",`<br>
    `2: "Wednesday",`<br>
    `3: "Thursday",`<br>
    `4: "Friday",`<br>
    `5: "Saturday",`<br>
    `6: "Sunday",`<br>
`}`<br>

`df["day_of_week"] = df['created_at'].dt.dayofweek`<br>
`df["day_of_week"] = df["day_of_week"].map(dow_mapping)` <br>

5. Is Complete Order? <br>
`df["is_complete"] = df['status'].apply(lambda x: 1 if x in ["COMPLETE"] else 0)` <br>

# Exploratory Data Analysis
Untuk saat ini anda bisa mengakses hasil analisa pada link berikut:<br>
[Analysis](https://drive.google.com/file/d/1PX9So7JlmAs--c4fCEt6BYwxG5BZbUBk/view?usp=sharing)

# Conclusion and Recommendation
## Conclusion
Dari analisis yang telah dilakukan, dapat disimpulkan:
* Customer lebih sering membeli produk dengan harga di bawah 939.
* Lebih dari 80% order memiliki 1 quantity per order dengan rata-rata persentase complete order cenderung diatas 55% pada rentang order quantity 1-3 dan 6-10. Customer lebih memilih untuk melakukan pembelian yang lebih kecil dengan produk yang lebih sedikit per order.
* Analisis menunjukkan bahwa Men's Fashion dan Mobiles & Tablets adalah kategori tertinggi yang diorder customers, terhitung sekitar 15-20% dari total transaksi. Namun, mereka menunjukkan tingkat complete order rate yang berbeda. Rata-rata complete order rate untuk Mobiles & Tablets (42%) adalah sekitar 15% lebih rendah daripada Men's Fashion (57%).
* Rata-rata grand total terus meningkat sejak 2017 Q3. Bahkan, meningkat 238% dari Q3 2017 ke Q3 2018 dengan total orders tertinggi terjadi pada Q4. Namun, selama periode waktu yang sama, complete order rate mengalami penurunan dari waktu ke waktu tanpa pola yang jelas. 
* Total orders terbesar terjadi pada bulan November dengan order lebih dari 60k. Total orders kedua terbesar terjadi pada bulan Mei dengan order mencapai 30k.
* Rata-rata grand_total stabil dalam skala harian. Namun, total order tertinggi terjadi pada hari Jumat. Selain itu, tampaknya complete order rate tetap stabil dan sebagian besar di atas 50%.
* Customer dengan canceled order memiliki median rata-rata bulanan price 3x lebih tinggi daripada customer dengan complete order. Hal ini mungkin menunjukkan bahwa harga yang tinggi merupakan faktor yang menyebabkan customer mengurungkan niat untuk membeli sehingga terjadi pembatalan order.
* Analisis menunjukkan bahwa order yang dibatalkan memiliki median rata-rata grand total 3x lebih tinggi daripada order yang berhasil sedangkan  diskon secara statistik tidak berdampak signifikan pada complete order rate. Ada kemungkinan bahwa customer membatalkan atau refund order karena produk yang sebenarnya berbeda dari yang diiklankan
* Retention rate analysis menunjukkan bahwa tingkat retention customers dalam 1 tahun secara keseluruhan tergolong rendah. Hanya rata-rata 5.5% customer melakukan repeat order dalam 90 hari sejak pembelian terakhir mereka. Berdasarkan [Harvard Business School](https://hbr.org/2014/10/the-value-of-keeping-the-right-customers)  mendapatkan customers baru lebih mahal 5-25 kali daripada mempertahankan customers yang ada
* 50% customers masih aktif dalam melakukan transaksi yaitu di segmen Loyal, Potential, New, dan Champions
* Segmen prioritas untuk high recency (R 3-4) adalah Champions, Loyal, Potential, dan New
* Segmen prioritas untuk low recency (R 1-2) adalah At Risk, About to Sleep, dan Can't Lose Them

## Recommendation
1. Perusahaan disarankan fokus untuk menawarkan lebih banyak produk dalam kisaran harga dibawah 939 seperti "Men's Fashion", "Home & Living", dan "Health & Sports" untuk memenuhi permintaan customer dan meningkatkan kinerja penjualan.
    * Perlu analisa lebih lanjut untuk produk dengan harga diatas 939, karena beberapa produk memiliki rentang harga yang lebar dan adanya brand yang mengeluarkan produk low-budget.
2. Minimalisir pemberian diskon karena kurang berdampak pada complete order rate
3. Perusahaan direkomendasikan berfokus pada promosi penawaran bundel yang lebih kecil, order quantity 1-3  atau menerapkan loyalty program untuk customer yang melakukan order dengan quantity 6-10 untuk memaksimalkan kesempatan complete order demi meningkatkan penjualan.
    * Jika perusahaan ingin lebih berhati-hati dalam menerapkan strategi promosi, lebih baik berfokus pada order quantity 1-5 dan 11-20 karena secara distribusi memiliki median persentase complete order lebih stabil diatas 55% dan rentang yang lebih sempit.
4. Perusahaan dapat fokus mempersiapkan volume order yang tinggi di bulan November dan Mei serta mempertimbangkan untuk meningkatkan stock pada bulan tersebut untuk memenuhi permintaan customer
5. Perusahaan bisa mempertimbangkan untuk menyediakan program cicilan atau memastikan bahwa produk aktual yang dikirim ke customer sesuai dengan iklan untuk meminimalkan jumlah order yang dibatalkan.
6. Contoh strategi yang dapat digunakan antara lain rekomendasi produk berdasarkan pembelian sebelumnya, loyalty program, cross-selling, upselling, re-engagement emails, follow-up phone calls atau surveys untuk memahami mengapa mereka belum melakukan pembelian akhir-akhir ini. 
    * Prioritaskan pada segmen Champion, Loyal, Potential, dan New untuk meminimalisir risiko churn
    * Prioritaskan pada segmen At Risk, About to Sleep, dan Can't Lose Them untuk reactivation customers

### Potential Opportunity
* Soghaat memiliki complete order rate tertinggi tetapi total ordernya rendah. Soghaat adalah produk makanan manis, complete order rate tinggi menunjukkan bahwa customer puas dengan produk dan layanannya. Namun, penting untuk berfokus pada peningkatan total order untuk kategori ini.
* Pada bulan Juni rata-rata grand total cenderung lebih tinggi dengan rentang mencapai 1.5-1.7 kali rata-rata grand total bulan-bulan lainnya. Perusahaan dapat memanfaatkan tren rata-rata grand total yang lebih tinggi di bulan Juni dengan memberikan penawaran dan promosi untuk menarik lebih banyak customer.
* Customers dengan frekuensi order tinggi (F 3-4), cenderung memiliki monetary value tinggi (M 3-4) meskipun sudah jarang aktif. Perusahaan bisa melakukan pendekatan pada customer yang jarang aktif (R 1-2) tetapi memiliki FM tinggi untuk dapat meningkatkan penjualan
