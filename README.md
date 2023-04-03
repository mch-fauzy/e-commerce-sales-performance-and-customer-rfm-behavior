# Pakistan E-Commerce Sales Performance and Customer RFM Behavior
| Report (PDF) | Dashboard |
|:---:|:---:|
| [Analysis](https://drive.google.com/file/d/1PX9So7JlmAs--c4fCEt6BYwxG5BZbUBk/view?usp=sharing) | [Tableau Story](https://public.tableau.com/views/PakistanE-CommerceSalesPerformanceandCustomerRFMBehaviour/DashboardStory?:language=en-US&:display_count=n&:origin=viz_share_link)


# Table of Contents

[Business Understanding](#Business-Understanding)<br>
[Data Cleaning and Preparation](#Data-Cleaning-and-Preparation)<br>

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
