# İlk ayarlar
.env den veritabanı ayarlarını yaptıktan sonra 
    ``` php artisan migrate ```
    demeden önce 
    - AppServiceProvider a 
     ```use Illuminate\Support\Facades\Schema; ``` ve boot() içine
     ```Schema::defaultStringLength(191);``` ekliyoruz.
     
  ```php artisan make:seeder UsersTableSeeder```
  - UsersTableSeeder a bir kullanıcı ekliyoruz.
  ```use Illuminate\Support\Facades\Hash;``` ı sayfaya ekliyoruz

        $user = [
            'name' => 'Just Eser',
            'email' => 'justeser@gmail.com',
            'password' => Hash::make('password')
        ];
        User::create($user);

  
    - DatabaseTableSeeder içindeki 
    ```$this->call(UsersTableSeeder::class);```önündeki yorum satırlarını kaldırdıktan sonra 
    ```php artisan migrate``` diyoruz. Sonrada 
    ```php artisan db:seed``` ile kullanıcıyı veritabanına ekliyoruz. ve ilk ayarları bitiriyoruz.
    

# Passport ile api authorization işlemleri

    - İlk olarak passport u laravel e ekliyoruz.
    ```composer require laravel/passport```
    - Config -> app.php içinde provider içine 
    ```Laravel\Passport\PassportServiceProvider::class,``` 
    ı ekliyoruz.Ve
    ```php artisan migrate``` 
    ile passport tablolarını ekliyoruz.
    ```php artisan passport:install``` 
    ile passport değerlerini yüklüyoruz.

    - User modeline geliyoruz 
    ``` use Laravel\Passport\HasApiTokens; ```ekliyoruz.
    ve class ın içine 
    ``ùse HasApiTokens, Notifiable;```ekliyoruz.

    - AuthServiceProvider a 
    ```use Laravel\Passport\Passport``` u ve 
    boot() metodu içine 
    ``` Passport::routes(); ``` u ekliyoruz...

    - Config -> auth.php dosyasındaki aşağıdaki kısımdaki driver => passport olarak değiştiriyoruz. 
```
        'api' => [
            'driver' => 'passport',
            'provider' => 'users',
        ],
```

    Artık API mizi yazmaya başlayabiliriz...