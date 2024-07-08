# Konfigurasi auth dengan breeze jika login multi user tetapi beda tabel
1. Install Laravel 11 `composer create project laravel/laravel breeze-multiple-users `
2. Install Breeeze `composer require laravel/breeze --dev`
3. Buat model admin dan super admin beserta
migratoins nya `php artisan make:model SuperAdmin -m` dan `php artisan make:model Admin -m`
4. Setelah itu copy paste semua isi atribut yang di file migrations user ke tabel admin dan super admin
5. lalu masuk ke folder model, copy paste semua yang di model user ke model super admin dan admin, jangan lupa ganti nama classnya sesuai dengan nama filenya
6. Buat konfigurasi guards untuk model super admin dan admin dengan cara
   1. masuk ke folder config lalu ke file auth.php tambahkan kode berikut dibawah guards yang web, karena yang web itu bawaan dari tabel users milik laravel
   ```
   super_admin' => [
        'driver' => 'session',
        'provider' => 'super_admins',
    ],
    'admin' => [
        'driver' => 'session',
        'provider' => 'admins',
    ],
    ```
    2. tambahkan kode dibawah di bagian provider (masih di file auth)
   ```
   'super_admins' => [
        'driver' => 'eloquent',
        'model' => App\Models\SuperAdmin::class,
    ],
    'admins' => [
        'driver' => 'eloquent',
        'model' => App\Models\Admin::class,
    ],
    ```

    3. terakhir pada bagian resetting passwords
   ```
    'super_admins' => [
        'provider' => 'super_admins',
        'table' => env('AUTH_PASSWORD_RESET_TOKEN_TABLE', 'password_reset_tokens'),
        'expire' => 60,
        'throttle' => 60,
    ],
    'admins' => [
        'provider' => 'admins',
        'table' => env('AUTH_PASSWORD_RESET_TOKEN_TABLE', 'password_reset_tokens'),
        'expire' => 60,
        'throttle' => 60,
    ],
    ```

7. Setelah itu implementasikan auntentikasi untuk admin dan super admin, tiru dibagian auth buat user bawaan breezenya tinggal modifikasi seusai kebutuhan
8. llau buat middleware dan selesai
