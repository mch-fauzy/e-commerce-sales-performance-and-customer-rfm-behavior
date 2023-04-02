# Pakistan E-Commerce Sales Performance and Customer RFM Behaviour

# Table of Contents

[Business Understanding](#Business-Understanding)<br>
[Data Cleaning and Preparation](#Data-Cleaning-and-Preparation)<br>

# Business Understanding
## Latar Belakang
Perusahaan memiliki data berisi informasi tentang transaksi ecommerce yang dilakukan oleh pelanggan di Pakistan. Data mencakup berbagai kolom seperti "Item ID", "Order Status", "Date of Order", "SKU", "Price", "Quantity", "Grand Total", "Category", "Payment Method" dan "Customer ID“. Perusahaan ingin menggunakan data tersebut untuk menyusun strategi bisnis berdasarkan data.

## Pernyataan Masalah
Perusahaan E-Commerce ingin mengetahui sales performance dan customer behaviour. Informasi ini akan membantu perusahaan untuk menentukan strategi dalam meningkatkan penjualan dan kepuasan pelanggan. 

Sebagai Data Analyst, saya bertugas untuk memahami customer behaviour dan rekomendasi apa yang dapat diberikan untuk meningkatkan penjualan dan kepuasan pelanggan.

# Data Cleaning and Preparation
Pada tahap ini dilakukan pemahaman dan pembersihan data untuk mempersiapkan data sebelum dilakukan analisa

## Drop Unused Columns
Kolom-kolom berikut akan di drop karena tidak digunakan dalam analisa:
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
