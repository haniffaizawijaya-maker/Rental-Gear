🛠️ Jurnal & Catatan Praktikum: Laravel Bug HunterMateri: Bug Hunting & Troubleshooting Framework LaravelKelas: TRPL (Teknologi Rekayasa Perangkat Lunak)Tanggal Praktikum: Senin, 9 September 2026Topik Utama: Mengidentifikasi, Menganalisis, dan Memperbaiki Error pada Projek Laravel (Rental Gear)📐 Alur Penanganan Bug (Workflow Overview)flowchart TD
    Start([Mulai Projek Laravel]) --> Step1{Folder vendor Ada?}
    Step1 -- Tidak --> Fix1[Jalankan: composer install & composer update]
    Step1 -- Ya --> Step2{.env & APP_KEY Ada?}
    Fix1 --> Step2

    Step2 -- Tidak --> Fix2[Salin .env.example -> .env & php artisan key:generate]
    Step2 -- Ya --> Step3{Koneksi DB & Migration OK?}
    Fix2 --> Step3

    Step3 -- Gagal Auth/DB --> Fix3[Atur DB_PASSWORD & DB_DATABASE di .env -> php artisan migrate]
    Step3 -- Berhasil --> Step4{Route & View Error?}
    Fix3 --> Step4

    Step4 -- Route Not Defined --> Fix4[Perbaiki nama route di Blade views]
    Step4 -- Syntax/Parse Error --> Fix5[Perbaiki sintaks Blade contoh: tag kurung kurawal ganda]
    Step4 -- Aman --> Step5{Data Dummy & Asset Gambar Ready?}

    Fix4 --> Step5
    Fix5 --> Step5

    Step5 -- Data Kosong / Gambar Missing --> Fix6[Jalankan Seeder & Sesuaikan Nama File Gambar di public/images]
    Step5 -- OK --> Step7[Update Config App Name -> php artisan config:clear]
    Fix6 --> Step7
    Step7 --> End([Projek Berjalan Lancar / App Ready])
🔍 Log Troubleshooting & Penyelesaian Error📌 Bug 1: Dependensi Vendor Tidak Ditemukan (vendor/ Missing)Gejala: Aplikasi Laravel tidak bisa dijalankan via php artisan serve atau menampilkan error dependensi.Penyebab: Folder vendor tidak diikutsertakan (di-ignore oleh .gitignore). Tanpa vendor, Laravel kehilangan pustaka dan kelas dasarnya.Solusi:Buka terminal di VS Code.Jalankan perintah install dependensi:composer install
Perbarui dependensi jika diperlukan:composer update
📌 Bug 2: Konfigurasi .env & Key Aplikasi Belum AdaGejala: Error No application encryption key has been specified.Penjelasan Teori:APP_KEY pada file .env berfungsi sebagai kunci kriptografi utama untuk mengamankan data sensitif (seperti session, cookie, dan enkripsi data).APP_KEY wajib ada karena internet tidak aman secara default, sehingga Laravel mewajibkan sistem pertahanan berlapis.Solusi:Duplikat file .env.example menjadi .env.Generate kunci enkripsi baru dengan perintah:php artisan key:generate
📌 Bug 3: Gagal Koneksi Database (Access Denied)Gejala Error:SQLSTATE[HY000] [1045] Access denied for user 'root'@'localhost' (using password: NO)
(Connection: mysql, Host: 127.0.0.1, Port: 3306, Database: laravel, SQL: select * from `sessions` ...)
Penyebab: Konfigurasi database di file .env masih ter-comment atau baris DB_PASSWORD belum diisi sesuai kredensial MySQL lokal.Solusi:Buka file .env, uncomment (Ctrl + /) dan sesuaikan parameter berikut:DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=root
Jalankan migrasi tabel database:php artisan migrate
Jalankan server lokal:php artisan serve
📌 Bug 4: Route [borrowings.index] not defined.Gejala: Aplikasi crash saat memuat halaman tertentu dengan pesan route tidak ditemukan.Penyebab: Terdapat kesalahan penulisan (typo) nama route pada berkas Blade view (resources/views/borrowings/layouts/app.blade.php).Solusi:Periksa Exception Trace pada browser untuk menemukan lokasi baris error.Buka file app.blade.php di VS Code.Sesuaikan pemanggilan fungsi route('borrowings.index') agar persis dengan yang terdefinisi di routes/web.php.📌 Bug 5: Data Alat Kosong & Gambar Broken (Empty Equipment List)Gejala: Halaman aplikasi berhasil tampil, tetapi daftar peralatan (seperti backpack, powerbank) kosong atau gambar tidak muncul.Penjelasan Teori Konsep Data Laravel:database/factories/: Menyediakan data palsu (dummy data) secara otomatis untuk testing.database/seeders/: Memasukkan data awal/spesifik (seperti akun admin, peran, atau katalog awal) ke database.tests/: Direktori pengujian otomatis (automated testing) menggunakan Pest atau PHPUnit.Solusi Perbaikan:Jalankan seeder database untuk mengisi data awal:php artisan db:seed
Fix Gambar Broken (Power Bank):Periksa file seeder di database/seeders/ -> terdaftar nama gambar power-bank-missing.jpg.Cek folder public/images/ -> ganti nama file gambar yang ada agar sesuai persis dengan referensi nama di seeder (power-bank-missing.jpg).📌 Bug 6: ParseError Sintaks Blade (unexpected token "{")Gejala Error:ParseError
resources\views\borrowings\create.blade.php:16
syntax error, unexpected token "{"
Penyebab: Adalah kelebihan sintaks kurung kurawal {{ atau typo penulisan nama variabel/route (contoh: borrowing tertulis borrowings).Solusi:Lihat Exception Trace pada pojok kanan atas tampilan Laravel Ignition error.Buka resources/views/borrowings/create.blade.php pada baris ke-16.Hapus kelebihan karakter { / {{ dan perbaiki typo plural/singular kata (borrowing vs borrowings).📌 Task Tambahan: Mengubah Nama Aplikasi (APP_NAME) & Configuration CacheInstruksi Mentor: Mengubah nama judul landing page dari "Laravel" menjadi "Rental Gear".Langkah-langkah:Buka file .env.Ubah variabel di bagian paling atas:APP_NAME="Rental Gear"
Jika nama belum berubah di tampilan, bersihkan cache konfigurasi Laravel:php artisan config:clear
💡 Rangkuman Perintah Terminal (Cheatsheet)Perintah TerminalFungsi / Tujuancomposer installMengunduh seluruh pustaka/dependensi ke folder vendorcomposer updateMemperbarui paket-paket dependensi ke versi terbaruphp artisan key:generateMembuat APP_KEY baru di file .envphp artisan migrateMenjalankan migrasi struktur tabel ke databasephp artisan db:seedMengisi database dengan data awal (seeding)php artisan serveMenjalankan local development server Laravelphp artisan config:clearMembersihkan cache konfigurasi saat ada perubahan di .env
