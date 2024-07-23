# laravel-API

# Laravel API: Web Services Using REST
___
## Introduction
This tutorial covers the creation of a RESTful API using Laravel, JavaScript, MySQL, AJAX, HTML, Bootstrap, PHP, and CSS.

## Problem Statement
Many developers struggle with integrating multiple technologies to create a cohesive web service.

## Problem Justification
A clear, step-by-step guide helps bridge the knowledge gap, ensuring efficient development processes.

## Existing Problem
Current tutorials often lack comprehensive integration of frontend and backend technologies, leading to fragmented learning.

## Importance of this Tutorial
This tutorial provides a holistic approach, combining modern web technologies to build robust web services.

## Main Objective
To develop a fully functional RESTful API using Laravel and integrate it with a frontend using AJAX and Bootstrap.

## Specific Objectives
1. Set up a Laravel project.
2. Create CRUD API endpoints.
3. Implement AJAX for asynchronous operations.
4. Design a responsive frontend with Bootstrap.
5. Ensure secure API access.

## Literature Review 

1. **"RESTful Web APIs" by Leonard Richardson and Mike Amundsen**: This book discusses the principles of REST and how to apply them in web API design. It provides a strong foundation for understanding REST, which is essential for this project as it involves creating a RESTful API using Laravel.

2. **"Laravel: Up and Running" by Matt Stauffer**: This book covers Laravel from setup to advanced features. It aligns with our project's objective of developing a fully functional RESTful API using Laravel, providing insights and best practices for efficient development.

3. **"JavaScript: The Good Parts" by Douglas Crockford**: This book offers insights into the essential features of JavaScript. Our project utilizes JavaScript, specifically with AJAX, for asynchronous data operations, making this book a valuable resource for understanding JavaScript's core concepts.

4. **"Learning PHP, MySQL & JavaScript" by Robin Nixon**: This book covers the fundamentals of PHP, MySQL, and JavaScript for building dynamic websites. Our project involves using PHP for backend development with Laravel, MySQL for database management, and JavaScript for frontend operations, directly relating to the content of this book.

5. **"Bootstrap 4 Quick Start" by Jacob Lett**: This practical guide to using Bootstrap for responsive web design is relevant to our project's use of Bootstrap to design a responsive frontend. It provides best practices and techniques that are applied in our project to enhance the user interface.

6. **Internet Source**: [REST API Tutorial](https://restapitutorial.com): This online resource explains REST API concepts and best practices, which are fundamental to our project's RESTful API development.

7. **Internet Source**: [Laravel Documentation](https://laravel.com/docs): The official Laravel documentation offers detailed instructions and examples, which are utilized in our project for setting up and configuring Laravel.

8. **Internet Source**: [MDN Web Docs - AJAX](https://developer.mozilla.org/en-US/docs/Web/Guide/AJAX): This guide on using AJAX for asynchronous web applications is directly related to our project's use of AJAX for data operations, providing technical details and examples.

9. **Internet Source**: [Bootstrap Documentation](https://getbootstrap.com/docs): The official Bootstrap documentation is referenced in our project for implementing responsive design elements, ensuring adherence to best practices.

10. **Internet Source**: [MySQL Documentation](https://dev.mysql.com/doc/): The official MySQL documentation is used in our project for database management and query optimization, providing essential guidelines and references.

By reviewing these sources, we ensure that our project aligns with industry standards and leverages established best practices for developing a RESTful API with Laravel, integrating it with a frontend using AJAX and Bootstrap, and managing data with MySQL.
> Studies show the importance of RESTful APIs in modern web development (Author, Year). Refer to [REST API Tutorial](https://restapitutorial.com) for further reading.

## Methodology
1. Set up the development environment.
2. Build the backend with Laravel and MySQL.
3. Create frontend pages using HTML, CSS, and Bootstrap.
4. Implement AJAX for data operations.
5. Test and deploy the application.

## Future and Conclusion
Future enhancements could include advanced security measures and scaling. This tutorial provides a solid foundation for integrating multiple web technologies effectively.



## Steps to Set Up a Laravel Project

## PART A: LARAVEL API DEVELOPMENT

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
## PART B:  API  TESTING

## Using Postman

### Install Postman
Download and install Postman from [Postman Download](https://www.postman.com/downloads/).

### Create a New Request
1. Open Postman and click on "New" to create a new request.
2. Select "Request" and give it a name, e.g., "Get All Users".

### Set Up the Request

#### GET Request
1. Set the request type to `GET`.
2. Enter the URL: `http://localhost:8000/api/users`.
3. Click on "Send" to execute the request.

#### POST Request
1. Set the request type to `POST`.
2. Enter the URL: `http://localhost:8000/api/users`.
3. Go to the "Body" tab and select "raw".
4. Set the format to JSON.
5. Enter the JSON data:

    ```json
    {
        "name": "John Doe",
        "email": "johndoe@example.com"
    }
    ```

## PART C: CLIENT SIDE DEVELOPMENT

In this section, we use front-end languages to create an application that connects to the API. We will utilize HTML, CSS, JavaScript, AJAX, and Bootstrap. Note that we do not use PHP here, as we are connecting to the API as a client, not a server.

### client side Technologies Used:
- **HTML:** For structuring the web page.
- **CSS:** For styling the web page.
- **JavaScript:** For adding interactivity.
- **AJAX:** For asynchronous communication with the API.
- **Bootstrap:** For responsive design and pre-built UI components.
- **J Query:** A javascript library
- **Sweet Alert:** for Material Design popup

### HTTP Requests:
We have created a page to handle various HTTP requests using AJAX for asynchronous communication. These requests include:

- **GET:** Retrieve data from the server.
- **POST:** Send new data to the server.
- **PUT:** Update existing data on the server.
- **PATCH:** Partially update existing data on the server.
- **DELETE:** Remove data from the server.

_download HTML files furthermore_

 
