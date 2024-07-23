# laravel-API

## Steps to Set Up a Laravel Project

### Step 1: Download Composer
_Download Composer, which allows you to use `php artisan` and other Laravel commands, from [this link](https://getcomposer.org/download/)._

### Step 2: Install Laravel Installer
_Open your command prompt and run the following command:_
```sh
composer global require laravel/installer
```
### Step 3: Navigate to Your Desired Directory
_Open the command prompt, navigate to any directory (e.g., Desktop), and run:_
```sh
cd /
```
### Step 4: Create a New Laravel Project
_In the command prompt, create a new Laravel project by running:_
```sh
laravel new my-laravel-api
```

### Step 5: Then, navigate into your new project directory:
```sh
cd my-laravel-api
```
### Step 6: run laravel project:
```sh
php artisan serve
```
_in command prompt copy given IP address and paste on browser _

### step 7: Set up your .env file: Configure your database settings in the .env file.
_Create the database: Create a MySQL database named laravel_api (or any name you prefer)_
```sh
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel_api
DB_USERNAME=root
DB_PASSWORD=
```
### step 8: Set Up Laravel Models and Migrations
```bash
php artisan make:migration create_users_table --create=users
```





