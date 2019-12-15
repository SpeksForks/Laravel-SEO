# Laravel SEO

[![Latest Stable Version](https://img.shields.io/packagist/v/romanzipp/Laravel-SEO.svg?style=flat-square)](https://packagist.org/packages/romanzipp/laravel-seo)
[![Total Downloads](https://img.shields.io/packagist/dt/romanzipp/Laravel-SEO.svg?style=flat-square)](https://packagist.org/packages/romanzipp/laravel-seo)
[![License](https://img.shields.io/packagist/l/romanzipp/Laravel-SEO.svg?style=flat-square)](https://packagist.org/packages/romanzipp/laravel-seo)
[![Code Quality](https://img.shields.io/scrutinizer/g/romanzipp/Laravel-SEO.svg?style=flat-square)](https://scrutinizer-ci.com/g/romanzipp/Laravel-SEO/?branch=master)
[![Build Status](https://img.shields.io/travis/romanzipp/Laravel-SEO.svg?style=flat-square)](https://travis-ci.org/romanzipp/Laravel-SEO)

A SEO package made for maximum customization and flexibility.

## Contents

- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
  - [Laravel-Mix Integration](#laravel-mix-integration)
  - [Schema.org Integration](#schemaorg-integration)
- [Cheat Sheet](#cheat-sheet)
- [Documentation](#documentation)
- [Testing](#testing)

## Installation

```
composer require romanzipp/laravel-seo
```

**If you use Laravel 5.5+ you are already done, otherwise continue.**

Add Service Provider to your `app.php` configuration file:

```php
romanzipp\Seo\Providers\SeoServiceProvider::class,
```

## Configuration

Copy configuration to config folder:

```
$ php artisan vendor:publish --provider="romanzipp\Seo\Providers\SeoServiceProvider"
```

## Usage

### Instantiation

```php
use romanzipp\Seo\Facades\Seo;
use romanzipp\Seo\Services\SeoService;

class IndexController
{
    public function index(Request $request, SeoService $seo)
    {
        $seo = seo();

        $seo = app(SeoService::class);

        $seo = Seo::make();
    }
}
```

### Examples

Take a look at an [example app](https://github.com/romanzipp/Laravel-SEO/blob/master/docs/EXAMPLE-APP.md) for more detailed usage.

#### Title

```php
seo()->title('romanzipp');
```

```php
use romanzipp\Seo\Structs\Title;

seo()->add(
    Title::make()->body('romanzipp')
);
```

... both compile to ...

```html
<title>romanzipp</title>
```

#### Charset

```php
use romanzipp\Seo\Structs\Meta\Charset;

seo()->add(
    Charset::make()->charset('utf-8')
);
```

... compiles to ...

```html
<meta charset="utf-8" />
```

#### Twitter

```php
seo()->twitter('card', 'summary');
```

```php
use romanzipp\Seo\Structs\Meta\Twitter;

seo()->add(
    Twitter::make()->name('card')->content('summary')
);
```

... both compile to ...

```html
<meta name="twitter:card" content="summary" />
```

#### Open Graph

```php
seo()->og('site_name', 'romanzipp');
```

```php
use romanzipp\Seo\Structs\Meta\OpenGraph;

seo()->add(
    OpenGraph::make()->name('site_name')->content('romanzipp')
);
```

... both compile to ...

```html
<meta name="og:site_name" content="romanzipp" />
```

For more information see the [Structs Documentation](https://github.com/romanzipp/Laravel-SEO/blob/master/docs/STRUCTS.md).

### Render

```blade
{{ seo()->render() }}
```

## Laravel-Mix Integration

You can include your `mix-manifest.json` file generated by [Laravel-Mix](https://laravel-mix.com) to automatically add preload/prefetch link elements to your document head.

#### Basic example

```php
seo()
    ->mix()
    ->load();
```

#### Extended usage

By default, all assets found in your mix file are prefteched. You can read more about preloading and prefetching [in this article by css-tricks.com](https://css-tricks.com/prefetching-preloading-prebrowsing/).

```php
seo()
    ->mix()
    ->rel('preload')
    ->load(public_path('custom-manifest.json'));
```

Take a look at the [Laravel-Mix intregration docs](https://github.com/romanzipp/Laravel-SEO/blob/master/docs/LARAVEL-MIX.md) for further usage.

## Schema.org Integration

This package features a basic integration for [Spaties Schema.org](https://github.com/spatie/schema-org) package to generate ld+json scripts.
Added Schema types render with the packages structs.

```php
use Spatie\SchemaOrg\Schema;

seo()->addSchema(
    Schema::localBusiness()->name('Spatie')
);
```

```php
use Spatie\SchemaOrg\Schema;

seo()->setSchemes([
    Schema::localBusiness()->name('Spatie'),
    Schema::airline()->name('Spatie'),
]);
```

Take a look at the [Schema.org package Docs](https://github.com/spatie/schema-org#usage).

## Cheat Sheet

| Code | Rendered HTML |
|--|--|
| `seo()->title('romanzipp')` | `<title>romanzipp</title>` |
| `seo()->meta('author', 'romanzipp')` | `<meta name="author" content="romanzipp" />` |
| `seo()->twitter('card', 'summary')` | `<meta name="twitter:card" content="summary" />` |
| `seo()->og('site_name', 'romanzipp')` | `<meta name="og:site_name" content="romanzipp" />` |
| `seo()->add(Charset::make()->charset('utf-8'))` | `<meta charset="utf-8" />` |

## Documentation

[Documentation](https://github.com/romanzipp/Laravel-SEO/blob/master/docs/INDEX.md)

## Testing

```
./vendor/bin/phpunit
```
