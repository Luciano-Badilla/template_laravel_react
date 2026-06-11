# Template Laravel con React

Template web construido con Laravel 10, Inertia.js, React, Vite y Tailwind CSS.

## Requisitos

* PHP 8.1 o superior
* Composer
* Node.js y npm
* MySQL

## Instalar Composer

### Windows

Descargar y ejecutar el instalador desde:

https://getcomposer.org/Composer-Setup.exe

Durante la instalación, seleccionar el ejecutable de PHP correspondiente. Si se utiliza Laragon, normalmente se encuentra dentro de:

```text
C:\laragon\bin\php\
```

Comprobar la instalación:

```bash
composer --version
```

### Linux y macOS

Seguir las instrucciones oficiales:

https://getcomposer.org/download/

Comprobar la instalación:

```bash
composer --version
```

## Instalar Node.js y npm

npm se instala junto con Node.js. Descargar la versión LTS desde:

https://nodejs.org/

Comprobar la instalación:

```bash
node --version
npm --version
```

## Instalación del proyecto

1. Clonar el repositorio y entrar a la carpeta:

```bash
git clone <URL_DEL_REPOSITORIO>
cd <NOMBRE_DE_LA_CARPETA>
```

2. Instalar las dependencias de PHP:

```bash
composer install
```

3. Instalar las dependencias de JavaScript:

```bash
npm install
```

4. Crear el archivo de configuración del entorno:

Linux, macOS o Git Bash:

```bash
cp .env.example .env
```

Windows PowerShell:

```powershell
Copy-Item .env.example .env
```

5. Generar la clave de la aplicación:

```bash
php artisan key:generate
```

6. Crear una base de datos MySQL y configurar estas variables en `.env`:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=
```

7. Ejecutar las migraciones:

```bash
php artisan migrate
```

## Ejecutar en desarrollo

Abrir dos terminales dentro de la carpeta del proyecto.

Terminal 1, servidor Laravel:

```bash
php artisan serve
```

Terminal 2, servidor de Vite:

```bash
npm run dev
```

La aplicación estará disponible normalmente en:

```text
http://127.0.0.1:8000
```

## Compilar para producción

```bash
npm run build
```

Antes de publicar, configurar el archivo `.env` de producción y ejecutar:

```bash
php artisan migrate --force
php artisan optimize
```

## Posible error: rutas duplicadas en Inertia

En algunos entornos puede ocurrir que las rutas generadas por Inertia aparezcan duplicadas, por ejemplo:

```text
http://localhost/proyecto/public/proyecto/public/login
```

Esto suele suceder cuando la aplicación se ejecuta dentro de un subdirectorio (por ejemplo con Laragon, XAMPP o Apache configurado con una carpeta virtual).

Si esto sucede, revisar el archivo:

```text
/vendor/inertiajs/inertia-laravel/src/Response.php
```

Buscar el siguiente bloque:

```php
$page = [
    'component' => $this->component,
    'props' => $props,
    'url' => $request->getBaseUrl().$request->getRequestUri(),
    'version' => $this->version,
];
```

Y verificar que quede exactamente así:

```php
$page = [
    'component' => $this->component,
    'props' => $props,
    'url' => $request->getRequestUri(),
    'version' => $this->version,
];
```

> **Importante:** este cambio se realiza dentro de la carpeta `vendor`, por lo que puede perderse al ejecutar nuevamente `composer install` o `composer update`. Si el problema reaparece, volver a aplicar la modificación.

## Comandos útiles

Limpiar cachés de Laravel:

```bash
php artisan optimize:clear
```

Ejecutar las pruebas:

```bash
php artisan test
```

Recrear todas las tablas de la base de datos:

```bash
php artisan migrate:fresh
```

> `migrate:fresh` elimina todas las tablas existentes. Utilizarlo solamente en entornos de desarrollo.
