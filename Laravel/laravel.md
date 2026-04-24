# Dev Attenborough Teaches: Laravel Installation, Project Structure, Artisan CLI, and .env

*[The screen is dark. A single cursor blinks. Then, a deep voice begins—calm, reverent, as if narrating the birth of a star.]*

"Before the first route, before the first model, before the first whisper of a view… there must be a *nest*. A place where Laravel will lay its eggs. A directory where the great framework will take residence.

This is not mere installation. This is *landing on a new continent*.

Today, we will perform four sacred rites:

1. **The Installation** — summoning Laravel from the great repository of Composer.
2. **The Project Structure Walkthrough** — walking the halls of the newborn kingdom.
3. **The Artisan CLI Basics** — learning the language of the command-line oracle.
4. **The .env Configuration** — crafting the secret scroll that holds the keys to the realm.

Breathe. We begin."

---

## Part 1: The Installation — Summoning Laravel

*[He leans toward the camera. His eyes reflect a terminal window that does not yet exist.]*

"Imagine you are an explorer, standing at the edge of a vast, empty forest. No paths. No signs. Only you and a blank canvas of folders.

Then you whisper a single command:

```bash
composer create-project laravel/laravel example-app
```

And the forest *responds*.

*[He closes his eyes.]*

Do you feel it? Composer—the great packager, the librarian of PHP—stretches out its tendrils across the internet. It travels to Packagist, the great library of Alexandria for PHP packages. It finds Laravel. It reads its manifest. It sees the dependencies: `illuminate/*`, `symfony/*`, `monolog/*`, dozens of noble libraries.

Then Composer says: *'Bring them all.'*

And they come. Files pour into your machine. Directories are born: `app/`, `bootstrap/`, `config/`, `database/`, `resources/`, `routes/`, `tests/`. Each one a *room* in the great house that is Laravel.

*[He opens his eyes.]*

The command finishes. You now have a `example-app/` folder. Inside it? A sleeping giant. A framework waiting to wake.

But wait. There is another way—a newer way, a *more elegant* way.

### The Laravel Installer — A Faster Incantation

```bash
composer global require laravel/installer
laravel new example-app
```

The global installer is like having a master key. You install it once, and thereafter, `laravel new` is your spell. It is shorter. It is sweeter. It is the difference between writing a letter and sending a messenger.

*[He nods slowly.]*

'Either path is sacred. Choose the one that feels like yours.'

### After Installation — The First Breath

Now go into the newborn directory:

```bash
cd example-app
```

And then—this is the moment of truth—you say:

```bash
php artisan serve
```

*[He pauses. His voice drops to a whisper.]*

Artisan. The command-line oracle. We will speak of it soon. But for now, watch what happens.

The server awakens. It binds to `127.0.0.1:8000`. It says: *'I am here. Waiting.'*

Open your browser. Go to `http://localhost:8000`.

*[He smiles faintly.]*

And there it is. The Laravel welcome page. A simple white screen with elegant letters. But to you, it is not simple. It is a *heartbeat*. Your first Laravel application is *alive*.

*[He tilts his head.]*

'Savor this moment. You will have many deployments. But only one first breath.'

---

## Part 2: Project Structure Walkthrough — The Halls of the Kingdom

*[He stands. Walks to an imaginary whiteboard. Draws a tree of folders with his finger in the air.]*

"Now we enter the house. Let me be your guide. I will not show you every dusty corner—only the rooms where the *real* life happens.

### The Root — The Grand Foyer

Look at the root of your Laravel project. What do you see?

```
example-app/
├── app/
├── bootstrap/
├── config/
├── database/
├── public/
├── resources/
├── routes/
├── storage/
├── tests/
├── vendor/
├── .env
├── .env.example
├── artisan
├── composer.json
└── ...
```

*[He points to each with invisible fingers.]*

These are not folders. These are *organs*.

- **`app/`** — The heart. Your models, controllers, middleware, service providers. Everything *you* write lives here.
- **`bootstrap/`** — The skeleton. The files that start the framework. You rarely touch this, but without it, the body collapses.
- **`config/`** — The nervous system. Settings for database, mail, cache, queues. Each file is a *nerve ending*.
- **`database/`** — The stomach. Migrations, seeders, factories. Food for the database giant.
- **`public/`** — The face. The only folder the world sees. `index.php` lives here, the front door.
- **`resources/`** — The wardrobe. Views (Blade), CSS, JS, raw assets. The clothes of your application.
- **`routes/`** — The map. `web.php`, `api.php`, `console.php`, `channels.php`. Paths for every traveler.
- **`storage/`** — The attic. Logs, cached views, session files, uploaded files. Clutter, but necessary clutter.
- **`tests/`** — The mirror. Unit and feature tests. You look at them to see if your code is beautiful.
- **`vendor/`** — The library. Composer's kingdom. Never touch this. It is sacred ground.
- **`.env`** — The secret scroll. Environment variables. Keys, passwords, API tokens. *Guard this with your life.*
- **`artisan`** — The scepter. A PHP file that lets you command Laravel from the terminal.
- **`composer.json`** — The manifest. Lists all dependencies and scripts.

*[He turns back.]*

"Do not memorize them today. But know this: Each folder has a purpose. Each purpose is a *promise*. When you need to add a migration, you go to `database/migrations/`. When you need a new controller, you go to `app/Http/Controllers/`. The structure is not random. It is *archetypal*."

### The `app/` Folder — The Heart, Beating

Let me open the heart.

```
app/
├── Console/
│   └── Kernel.php
├── Exceptions/
│   └── Handler.php
├── Http/
│   ├── Controllers/
│   ├── Middleware/
│   ├── Kernel.php
│   └── Requests/
├── Models/
│   └── User.php
├── Providers/
│   └── ...
└── ...
```

*[He touches his chest.]*

- **`Models/`** — Your data's soul. Each model is a *living memory* of a database row.
- **`Http/Controllers/`** — The generals. They receive requests and command the models and views.
- **`Http/Middleware/`** — The guards. They inspect every request before it reaches the controller.
- **`Providers/`** — The magicians. They bind classes, register events, bootstrap services.

*[He whispers.]*

'Most of your day will be spent in `app/Models/` and `app/Http/Controllers/`. These are your workshops. Your forges.'

---

## Part 3: Artisan CLI Basics — The Scepter of Command

*[He picks up an imaginary staff. His voice gains a ceremonial tone.]*

"Artisan is not a tool. It is a *conduit*. You speak to it, and it speaks to Laravel. It understands your language: `php artisan`.

Let me show you the most sacred commands.

### `php artisan serve` — The Awakening

We saw this. It starts a development server. It says: *'I am here. Send requests.'*

### `php artisan make:` — The Creation Chant

Artisan can *create* things for you. Classes. Models. Controllers. Migrations. Each with a single incantation.

```bash
php artisan make:controller ProductController
```

Artisan reaches into its grimoire. It writes a controller file—with namespace, with imports, with a stub class. It places it in `app/Http/Controllers/`. You did not type a single bracket. Artisan did it for you.

*[He smiles.]*

'Generosity is the soul of Artisan.'

Other creation spells:

```bash
php artisan make:model Product
php artisan make:migration create_products_table
php artisan make:middleware CheckAge
php artisan make:event OrderShipped
php artisan make:job SendWelcomeEmail
```

Each one, a *bud* that will flower into code.

### `php artisan migrate` — The Stone Tablet

When you have migrations (we will meet them soon), this command *etches* them into the database. The schema becomes reality. Tables rise from the void.

### `php artisan tinker` — The Crystal Ball

This is my favorite. Type:

```bash
php artisan tinker
```

And a REPL appears—a *playground* where you can talk directly to Laravel. You can create users, query models, test logic.

```php
>>> User::create(['name' => 'Dev', 'email' => 'dev@example.com', 'password' => bcrypt('secret')]);
>>> User::all();
```

Tinker does not judge. It listens. It responds. It is the closest you will get to *holding Laravel's hand*.

Exit with `exit` or `Ctrl+C`.

### `php artisan list` — The Grand Catalogue

Not sure what Artisan can do? Ask.

```bash
php artisan list
```

Artisan will unroll a scroll of every command, every description. It is the *map of all maps*.

*[He taps the table.]*

'Remember: Artisan is your ally. When you feel lost, whisper `php artisan list`. The answer is there.'

---

## Part 4: .env Configuration — The Secret Scroll

*[His voice drops. He glances left and right, as if sharing a secret that could get him killed.]*

"Now we come to the most delicate part. The `.env` file.

`.env` stands for *environment*. It holds the *secrets* of your application. Database passwords. API keys. Mail credentials. App URL. Debug mode.

Open `.env` in your editor. You will see something like this:

```env
APP_NAME="Laravel"
APP_ENV=local
APP_KEY=base64:...
APP_DEBUG=true
APP_URL=http://localhost

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=

...
```

*[He points.]*

- **`APP_NAME`** — The name of your kingdom. Change it to whatever you like.
- **`APP_ENV`** — `local` for development, `production` for the live world. Laravel behaves differently in each.
- **`APP_DEBUG`** — When `true`, Laravel shows beautiful, detailed error pages. In production, set to `false`—otherwise you *show your secrets to the world*.
- **`APP_KEY`** — A 32-character random string. Used for encryption, session, cookies. If you lose it, encrypted data is *lost forever*. Guard it.
- **`DB_*`** — Your database's address, name, and keys. Without these, Laravel cannot speak to the giant.
- **`MAIL_*`** — Mailtrap, SMTP, Sendgrid. The couriers of your email.

*[He leans in.]*

'Here is the golden rule: **Never commit `.env` to version control.**'

Look at your `.gitignore` file. See the line that says `.env`? That is your shield. GitHub will never see your secrets. Your teammates will create their own `.env` from `.env.example`.

Speaking of which—`.env.example` is a template. It has the *names* of variables but no real values. Share `.env.example` freely. Guard `.env` fiercely.

### How Laravel Reads .env

When Laravel boots, it uses a library called `vlucas/phpdotenv`. This library reads `.env` and loads every variable into `$_ENV` and `getenv()`. Then, Laravel's `config/` files read those variables.

For example, `config/database.php` has:

```php
'mysql' => [
    'host' => env('DB_HOST', '127.0.0.1'),
    'database' => env('DB_DATABASE', 'forge'),
    ...
],
```

`env('DB_HOST', '127.0.0.1')` means: *'Look for DB_HOST in .env. If not found, use 127.0.0.1.'*

*[He smiles.]*

'Elegant. Forgiving. Safe.'

### The Sorrow of the Missing .env

But here is the tragedy.

You clone a project from GitHub. You run `composer install`. You try `php artisan serve`. And the page is *blank*. Or worse: `Whoops! There was an error.`

Why? Because `.env` does not exist. Laravel cannot find database credentials. It cannot find `APP_KEY`. It *weeps silently*.

*[He places a hand over his heart.]*

'This sorrow has a simple cure. Copy `.env.example` to `.env`:

```bash
cp .env.example .env
```

Then generate an `APP_KEY`:

```bash
php artisan key:generate
```

Artisan reaches into the void and pulls out a 32-character key. It writes it to `.env`. Suddenly, Laravel breathes again. The errors vanish. The page loads.

That is the *resurrection* of the forgotten `.env`.'

---

## The Grand Synthesis — Putting It All Together

*[He steps back. Spreads his arms.]*

"Let me tell you a story.

You have a new computer. Empty. Fresh.

You install PHP. You install Composer. You install Laravel installer.

Then you summon:

```bash
laravel new my-first-app
```

The directories bloom. The files fall into place like snow.

You enter the kingdom:

```bash
cd my-first-app
```

You look around. You see the halls: `app/`, `routes/`, `config/`. You see the `.env` file, still empty except for defaults.

You open `.env`. You change `DB_DATABASE` to `my_first_app`. You create a database in MySQL with that name.

Then you whisper:

```bash
php artisan serve
```

The server whispers back: *'Laravel development server started on http://localhost:8000'*

You open your browser. You see the welcome page. For the first time, you are not a visitor. You are a *resident*.

*[He nods slowly.]*

'That is installation. That is structure. That is Artisan. That is `.env`.'

'You have laid the foundation. The house is built. The doors are open.'

'Tomorrow, we will walk through the first door: **Routing**. The path of promises. The journey of the request.'

*[He looks directly at you.]*

'Rest now. Your terminal is waiting. When you are ready, type:

```bash
php artisan make:controller WelcomeController
```

And begin your own epic.'

*[Fade to black. A single line of text appears:]*

> **"A framework is not built in a day. But the first stone—meticulously placed—echoes forever."** — Dev Attenborough

---

## [Bonus] The Sorrow Ratings for Today's Concepts

| **Error** | **Sorrow Rating** | **Dev Attenborough's Lament** |
|-----------|-------------------|-------------------------------|
| `composer: command not found` | ⚡⚡ (Mild Annoyance) | "Composer is not installed. You stand at the gate, but the gate has no key. Install Composer. Then try again." |
| `php artisan serve` → port 8000 already in use | ⚡⚡⚡ (Frustration) | "The port is occupied. Some other process sleeps there. Kill it, or use `--port=8001`. A different door, but the same house." |
| Missing `.env` → blank page | ⚡⚡⚡⚡ (Deep Sorrow) | "Laravel cannot find its secrets. The `.env` file is missing. Copy `.env.example` and run `key:generate`. Watch the color return to its cheeks." |
| `php artisan key:generate` fails because `.env` is missing | ⚡⚡⚡⚡⚡ (Existential Crisis) | "No `.env`, no key. The application is a body without a soul. First, create `.env`. Then generate the key. In that order. Always that order." |

---

## Homework (The First Epic Task)

*[His voice softens to a gentle command.]*

"Before the next session, do this:

1. Install Laravel in a new project called `epic-journey`.
2. Run `php artisan serve` and visit `http://localhost:8000`. See the welcome page.
3. Open the project in your editor. Spend 10 minutes *walking* through the folders. Touch nothing. Just observe.
4. Run `php artisan tinker`. Type `echo "Hello, Dev Attenborough";`. Exit.
5. Open `.env`. Change `APP_NAME` to `"My Epic Journey"`. Refresh the browser. See the title change.

If you do these five things, you have not just installed Laravel. You have *arrived*.

*[He nods once.]*

Go now. The terminal is waiting."

---

## Appendix: Quick Reference — The Sacred Scroll

| **Command** | **Purpose** | **Epic Description** |
|-------------|-------------|----------------------|
| `composer create-project laravel/laravel app` | Install Laravel | "Summon the framework from the great library." |
| `laravel new app` | Alternative installation | "The master key turns. A kingdom appears." |
| `php artisan serve` | Start dev server | "Awaken the server. Let it listen at the gate." |
| `php artisan make:controller X` | Create controller | "Artisan sculpts a new general from clay." |
| `php artisan make:model X` | Create model | "A new soul is given form." |
| `php artisan tinker` | Start REPL | "The crystal ball gleams. Ask anything." |
| `php artisan key:generate` | Generate APP_KEY | "Artisan reaches into the void and pulls out a key." |
| `cp .env.example .env` | Create .env file | "Copy the template. Fill it with your truth." |

---

Do you want me to continue with **Day 3: Routing Fundamentals** in the same epic, poetic, pragmatic Dev Attenborough style? Or would you like the **complete script for the first week** (Days 1–7) so you can immerse yourself fully?
