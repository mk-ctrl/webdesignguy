# Set up Laravel in few steps

Laravel is a free, open-source PHP web framework used for web application development. It follows the Model-View-Controller (MVC) architectural pattern and is known for its elegant syntax and tools for tasks such as routing, authentication, and caching. In this blog post, we will go through the steps of setting up a Laravel application on your local machine.

### Step 1: Install PHP and Composer

Before installing Laravel, you must have PHP and Composer installed on your machine. If you do not have them installed, you can download them from the official website. Follow these detailed instructions to install PHP.

[Install PHP](https://www.php.net/manual/en/install.php)

After the PHP installation is complete you can now process to install composer.

Composer is a dependency manager for PHP. It allows developers to manage the libraries and packages their project depends on, and it automatically handles installing, updating, and resolving dependencies between them. It is widely used in the PHP community and is considered the de facto standard for managing dependencies in PHP projects. It helps to easily manage and update libraries and dependencies in a project.

[Install composer](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-macos)

### Step 2: Install Laravel

Once you have PHP and Composer installed, you can use the Composer command-line tool to install Laravel. Open your command prompt and run the following command:

```php
composer global require laravel/installer
```

This command will install the Laravel installer, which you can use to create new Laravel projects.

### Step 3: Create a new Laravel project

To create a new Laravel project, you can use the following command:

```php
laravel new projectname
```

### Step 4: Configure the database

Laravel comes with an ORM (Object-Relational Mapping) called Eloquent, which allows you to interact with your database using an object-oriented syntax. To configure your database, you'll need to update the `.env` file located in the root of your project. This file contains the configuration settings for your application, including your database connection settings.

### Step 5: Run the migrations

Migrations are like version control for your database. They allow you to modify your database schema in a structured and organized way. To create your database tables, you can use the following command:

```php
php artisan migrate
```

This command will run all of the migrations in the `database/migrations` directory, creating the necessary tables in your database.

### Step 6: Start the server

To start the development server, you can use the following command:

```php
php artisan serve
```

This command will start a development server at [`http://localhost:8000`](http://localhost:8000). You can visit this URL in your browser to see your application.

In conclusion, setting up a Laravel application is a straightforward process that can be completed in a few simple steps. The framework provides a lot of tools and features out of the box, making it an excellent choice for web application development. With Laravel, you can focus on building your application's functionality and leave the tedious tasks to the framework.