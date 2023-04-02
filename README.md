# Pakistan E-Commerce Sales Performance and Customer RFM Behaviour

# Table of Contents

[Business Understanding](#Business-Understanding)
[Data Cleaning and Preparation](#Data-Cleaning-and-Preparation)

# Business Understanding
## Latar Belakang
Perusahaan memiliki data berisi informasi tentang transaksi ecommerce yang dilakukan oleh pelanggan di Pakistan. Data mencakup berbagai kolom seperti "Item ID", "Order Status", "Date of Order", "SKU", "Price", "Quantity", "Grand Total", "Category", "Payment Method" dan "Customer ID“. Perusahaan ingin menggunakan data tersebut untuk menyusun strategi bisnis berdasarkan data.

## Pernyataan Masalah
Perusahaan E-Commerce ingin mengetahui sales performance dan customer behaviour. Informasi ini akan membantu perusahaan untuk menentukan strategi dalam meningkatkan penjualan dan kepuasan pelanggan. 

Sebagai Data Analyst, saya bertugas untuk memahami customer behaviour dan rekomendasi apa yang dapat diberikan untuk meningkatkan penjualan dan kepuasan pelanggan.

# Data Cleaning and Preparation
## Drop Unused Columns
'df.drop(['Unnamed: 21', 'Unnamed: 22', 'Unnamed: 23',
         'Unnamed: 24', 'Unnamed: 25', 'increment_id',
         'sales_commission_code', 'Working Date',
         'BI Status', ' MV ', 'Year', 'M-Y', 'FY', 'Month'], axis=1, inplace=True)'

## Missing Value Handling
