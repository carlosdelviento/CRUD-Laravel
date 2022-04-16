<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400"></a></p>

<p align="center">
<a href="https://travis-ci.org/laravel/framework"><img src="https://travis-ci.org/laravel/framework.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>

## About Laravel

Laravel is a web application framework with expressive, elegant syntax. We believe development must be an enjoyable and creative experience to be truly fulfilling. Laravel takes the pain out of development by easing common tasks used in many web projects, such as:

- [Simple, fast routing engine](https://laravel.com/docs/routing).
- [Powerful dependency injection container](https://laravel.com/docs/container).
- Multiple back-ends for [session](https://laravel.com/docs/session) and [cache](https://laravel.com/docs/cache) storage.
- Expressive, intuitive [database ORM](https://laravel.com/docs/eloquent).
- Database agnostic [schema migrations](https://laravel.com/docs/migrations).
- [Robust background job processing](https://laravel.com/docs/queues).
- [Real-time event broadcasting](https://laravel.com/docs/broadcasting).

Laravel is accessible, powerful, and provides tools required for large, robust applications.

## Learning Laravel

Laravel has the most extensive and thorough [documentation](https://laravel.com/docs) and video tutorial library of all modern web application frameworks, making it a breeze to get started with the framework.

If you don't feel like reading, [Laracasts](https://laracasts.com) can help. Laracasts contains over 1500 video tutorials on a range of topics including Laravel, modern PHP, unit testing, and JavaScript. Boost your skills by digging into our comprehensive video library.

## Laravel Sponsors

We would like to extend our thanks to the following sponsors for funding Laravel development. If you are interested in becoming a sponsor, please visit the Laravel [Patreon page](https://patreon.com/taylorotwell).

### Premium Partners

- **[Vehikl](https://vehikl.com/)**
- **[Tighten Co.](https://tighten.co)**
- **[Kirschbaum Development Group](https://kirschbaumdevelopment.com)**
- **[64 Robots](https://64robots.com)**
- **[Cubet Techno Labs](https://cubettech.com)**
- **[Cyber-Duck](https://cyber-duck.co.uk)**
- **[Many](https://www.many.co.uk)**
- **[Webdock, Fast VPS Hosting](https://www.webdock.io/en)**
- **[DevSquad](https://devsquad.com)**
- **[Curotec](https://www.curotec.com/services/technologies/laravel/)**
- **[OP.GG](https://op.gg)**
- **[WebReinvent](https://webreinvent.com/?utm_source=laravel&utm_medium=github&utm_campaign=patreon-sponsors)**
- **[Lendio](https://lendio.com)**

## Contributing

Thank you for considering contributing to the Laravel framework! The contribution guide can be found in the [Laravel documentation](https://laravel.com/docs/contributions).

## Code of Conduct

In order to ensure that the Laravel community is welcoming to all, please review and abide by the [Code of Conduct](https://laravel.com/docs/contributions#code-of-conduct).

## Security Vulnerabilities

If you discover a security vulnerability within Laravel, please send an e-mail to Taylor Otwell via [taylor@laravel.com](mailto:taylor@laravel.com). All security vulnerabilities will be promptly addressed.

## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

## Setup and commands for CRUD

# Laravel 8 Sheets for Libreria CRUD's

# Database MySQL
Laravel

# 1-Create project with Composer
composer create-project laravel/laravel libreria

# 2-Create and delete migration tables
php artisan make:migration categorias
php artisan migrate

# 2a-Fix table lenght migration(OPCIONAL)
1-php artisan db:wipe
2-app\Providers\AppServiceProvider.php

use Illuminate\Support\Facades\Schema;
/**
 * Bootstrap any application services.
 *
 * @return void
 */
public function boot()
{
    Schema::defaultStringLength(191); 
}

# 3-Correr project server
php artisan serve

# 4-Composer Login Generator
1-composer require laravel/ui --dev
2-php artisan ui bootstrap
2-php artisan ui bootstrap --auth
3-npm install
4-npm run dev
5-php artisan serve

# 5-Composer CRUD Generator
1-composer require ibex/crud-generator --dev
2-php artisan vendor:publish --tag=crud
3-php artisan make:crud categorias
4-php artisan make:crud libros

# 6-CRUD Access
Paso 1
routes/web.php

Route::resource('libros', App\Http\Controllers\LibroController::class);
Route::resource('categorias', App\Http\Controllers\CategoriaController::class);

Paso 2
resources\views\layouts\app.blade.php
Agregar las dos listas
 <!-- Left Side Of Navbar -->
                    <ul class="navbar-nav me-auto">

                        <li class="nav-item">
                            <a class="nav-link" href="{{ route('libros.index') }}">{{ __('Libros') }}</a>
                        </li>

                        <li class="nav-item">
                            <a class="nav-link" href="{{ route('categorias.index') }}">{{ __('Categorias') }}</a>
                        </li>
                    
                    </ul>

# 6-a Fix Login Bootstrap Styles and Js (OPCIONAL)
1-npm i vue-loader
2-npm run dev

# 7-Select de relacion del CRUD con tablas relacionadas
MODIFICACION CONTROLADOR
app\Http\Controllers\LibroController.php

public function create()
    {
        $libro = new Libro();
        $categorias= Categoria::pluck('nombre','id');
        return view('libro.create', compact('libro','categorias'));
    }

public function edit($id)
    {
        $libro = Libro::find($id);
        $categorias= Categoria::pluck('nombre','id');
        return view('libro.edit', compact('libro','categorias'));
    }
MODIFICACION VISTA
resources\views\libro\form.blade.php

(REMOVE CATEGORIA DUPLICADA)

<div class="form-group">
            {{ Form::label('categoria_id') }}
            {{ Form::select('categoria_id', $categorias, $libro->categoria_id, ['class' => 'form-control' . ($errors->has('categoria_id') ? ' is-invalid' : ''), 'placeholder' => 'Categoria Id']) }}
            {!! $errors->first('categoria_id', '<div class="invalid-feedback">:message</div>') !!}
 </div>

resources\views\libro\index.blade.php

CAMBIAR

<th>categoria_id</th>

POR
<th>Categoría</th>

{{ $libro->categoria_id }}
POR
{{ $libro->categoria->nombre }}

8-Ajustes finales
SECURITY ACCESS
routes\web.php

Route::resource('libros', App\Http\Controllers\LibroController::class)->middleware('auth');
Route::resource('categorias', App\Http\Controllers\CategoriaController::class)->middleware('auth');

resources\views\layouts\app.blade.php

<!-- Left Side Of Navbar -->
 @if (Auth::check())
<ul class="navbar-nav me-auto">

                        <li class="nav-item">
                            <a class="nav-link" href="{{ route('libros.index') }}">{{ __('Libros') }}</a>
                        </li>

                        <li class="nav-item">
                            <a class="nav-link" href="{{ route('categorias.index') }}">{{ __('Categorias') }}</a>
                        </li>
</ul>
@endif
