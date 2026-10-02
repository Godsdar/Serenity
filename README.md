# Serenity

A mental-health blog built as a Laravel 12 + Inertia.js monolith. React pages are
rendered through Inertia, authentication comes from Laravel Breeze, and the app
runs on SQLite by default so it starts with no database setup.

## What it does

- **Authentication** — register, log in, email verification, password reset and
  password confirmation (Laravel Breeze).
- **Profile** — update account details and password, delete the account.
- **Blog pages** — welcome, post and user pages rendered as Inertia/React
  components.
- **Dashboard** — authenticated area behind the `auth` + `verified` middleware.
- Responsive layout with Tailwind CSS and React Bootstrap components.

## Tech stack

| Layer | Technology |
|---|---|
| Backend | PHP 8.2+, Laravel 12, Inertia.js, Ziggy |
| Frontend | React 18, Inertia React adapter, Tailwind CSS, React Bootstrap |
| Build | Vite 7 |
| Database | SQLite (default), MySQL/PostgreSQL supported |
| Auth | Laravel Breeze |
| Tests | PHPUnit 11 |

## Run it (3 commands)

```bash
composer install && npm install
cp .env.example .env && php artisan key:generate && php artisan migrate
composer run dev          # serves the app on http://localhost:8000
```

`composer run dev` starts the PHP server, queue worker, log tailer and the Vite
dev server together. To run them separately: `php artisan serve` and `npm run dev`.

## Testing

```bash
php artisan test
```

The suite covers authentication (login, registration, email verification,
password reset/update/confirmation) and profile updates.

## Project structure

```
app/
  Http/Controllers/       # controllers (auth, profile, user)
  Http/Requests/          # form request validation
  Models/                 # Eloquent models
resources/
  js/Pages/               # Inertia + React pages (Welcome, Dashboard, Post, User, Auth, Profile)
  js/Components/          # shared React components
  js/Layouts/             # layout components
  js/app.jsx              # Inertia entry point
routes/
  web.php                 # web routes
  auth.php                # Breeze auth routes
tests/                    # PHPUnit feature + unit tests
```

## What was tricky

- **Wiring React pages into a Laravel monolith** — Inertia resolves pages by
  glob (`./Pages/**/*.jsx`); making unknown page names throw a clear error
  instead of failing silently was important for debugging.
- **Keeping auth tests green** — the Breeze feature tests assert guest/verified
  redirects, so any change to middleware or routes has to keep those contracts.

## Development notes (AI-assisted)

Parts of the UI and the Breeze scaffolding were generated with AI agents and then
reviewed:

- The backend uses the standard Laravel/Breeze architecture; tests are the safety
  net for the auth and profile flows.
- The Inertia page resolver was hardened to surface unknown pages explicitly
  instead of rendering blank.
- CI (`.github/workflows/ci.yml`) runs `php artisan test` and `npm run build` on
  every push and pull request.

## License

MIT — see [LICENSE](LICENSE).
