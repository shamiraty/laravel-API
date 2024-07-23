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
```php
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

### step 9: Define the schema in the migration file (database/migrations/xxxx_xx_xx_xxxxxx_create_users_table.php):

``` php
public function up()
{
    Schema::create('users', function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->string('email')->unique();
        $table->timestamp('email_verified_at')->nullable();
        $table->string('password');
        $table->string('phone_number');
        $table->string('resident');
        $table->string('national_id')->unique();
        $table->rememberToken();
        $table->timestamps();
    });
}

```

### step 10: Run the migration:
```bash
php artisan migrate
```
### step 10:  Create a User Model and Controller
_Create a User model:_
```bash
php artisan make:model User
```

### step 11: Create a controller:
```bash
php artisan make:controller UserController --resource
```

### step 12: Define the User model (app/Models/User.php):
```php
namespace App\Models;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;
class User extends Authenticatable
{
    use HasFactory, Notifiable;

    protected $fillable = [
        'name', 'email', 'password', 'phone_number', 'resident', 'national_id',
    ];

    protected $hidden = [
        'password', 'remember_token',
    ];
}
```

### step 13: Set Up API Routes
_Define API routes in routes/api.php:_
```php

use App\Http\Controllers\UserController;
Route::apiResource('users', UserController::class);
```

### step 14: Implement Controller Methods
_Update UserController (app/Http/Controllers/UserController.php):_

```php
namespace App\Http\Controllers;
use App\Models\User;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Hash;
class UserController extends Controller
{
    public function index()
    {
        return User::all();
    }

    public function store(Request $request)
    {
        $validatedData = $request->validate([
            'name' => 'required|string|max:255',
            'email' => 'required|string|email|max:255|unique:users',
            'password' => 'required|string|min:8',
            'phone_number' => 'required|string|max:20',
            'resident' => 'required|string|max:255',
            'national_id' => 'required|string|max:255|unique:users',
        ]);

        $user = User::create([
            'name' => $validatedData['name'],
            'email' => $validatedData['email'],
            'password' => Hash::make($validatedData['password']),
            'phone_number' => $validatedData['phone_number'],
            'resident' => $validatedData['resident'],
            'national_id' => $validatedData['national_id'],
        ]);

        return response()->json($user, 201);
    }

    public function show($id)
    {
        $user = User::findOrFail($id);
        return response()->json($user);
    }

    public function update(Request $request, $id)
    {
        $user = User::findOrFail($id);

        $validatedData = $request->validate([
            'name' => 'sometimes|string|max:255',
            'email' => 'sometimes|string|email|max:255|unique:users,email,' . $user->id,
            'password' => 'sometimes|string|min:8',
            'phone_number' => 'sometimes|string|max:20',
            'resident' => 'sometimes|string|max:255',
            'national_id' => 'sometimes|string|max:255|unique:users,national_id,' . $user->id,
        ]);

        if (isset($validatedData['password'])) {
            $validatedData['password'] = Hash::make($validatedData['password']);
        }

        $user->update($validatedData);

        return response()->json($user);
    }

    public function destroy($id)
    {
        $user = User::findOrFail($id);
        $user->delete();

        return response()->json(null, 204);
    }
}
```

### Step 15: Testing the API
_Run the Laravel development server:_
```bash
php artisan serve
```



 

 



