<h1 align="center">Relishing Roots</h1>

<p align="center">
  <strong>Recipes with memory, flavor, and meaning.</strong><br>
  A recipe journal for family recipes, cultural roots, and the stories behind the food.
</p>

<p align="center">
  <img alt="Laravel 12+" src="https://img.shields.io/badge/Laravel-12%2B-ff2d20?logo=laravel&amp;logoColor=white">
  <img alt="MongoDB" src="https://img.shields.io/badge/MongoDB-flexible%20storage-47a248?logo=mongodb&amp;logoColor=white">
  <img alt="React + Vite" src="https://img.shields.io/badge/Frontend-React%20%2B%20Vite-646cff?logo=vite&amp;logoColor=white">
  <img alt="Status: concept" src="https://img.shields.io/badge/status-concept-orange">
</p>

<p align="center">
  <a href="#quick-start">Quick start</a> · <a href="#features">Features</a> · <a href="#mongodb-integration">MongoDB</a> · <a href="#data-model">Data model</a> · <a href="#roadmap">Roadmap</a> · <a href="#brand">Brand</a>
</p>

Relishing Roots is a web app concept for people who cook from memory. Save the recipes your family actually makes, attach the photos and the stories that go with them, and find them again by ingredient, mood or occasion. A Laravel backend provides the API, a separate frontend provides the experience, and MongoDB stores recipes, notes, tags and media metadata without forcing them into rigid tables.

> **Status:** early blueprint. The Laravel backend is scaffolded; the frontend and recipe features are planned (see the [roadmap](#roadmap)).

## Features

- **Recipes:** save ingredients, steps, prep and cook times, and a photo.
- **Organization:** tag by cuisine, season or occasion, and filter by breakfast, lunch, dinner, dessert or seasonal dishes.
- **Stories and notes:** keep personal notes and family memories alongside each recipe.
- **Search:** find recipes by ingredient, keyword, mood or meal type.
- **Favorites and history:** bookmark recipes and track what you have cooked.

## Quick start

**Requirements:** PHP and Composer, a running MongoDB instance (local or Atlas), and Node.js for the frontend.

```bash
git clone https://github.com/yourusername/relishing-roots.git
cd relishing-roots/backend

composer install
cp .env.example .env
php artisan key:generate
php artisan serve
```

The API is served at `http://127.0.0.1:8000`. Until the landing page is built you will see Laravel's default welcome page. Configure MongoDB next, as described below.

## Project structure

```text
RelishingRoots/
├── backend/      # Laravel app: API and application logic
├── frontend/     # Web UI
├── README.md
└── .gitignore
```

## MongoDB integration

MongoDB suits this app because a recipe is naturally a document: arrays of ingredients and steps, free-form tags, notes and image metadata all live together in one record.

**1. Install the Laravel MongoDB package**

```bash
cd backend
composer require mongodb/laravel-mongodb
```

**2. Configure `.env`**

```env
DB_CONNECTION=mongodb
MONGO_DSN=mongodb://127.0.0.1:27017
DB_DATABASE=relishing_roots
```

For MongoDB Atlas, set `MONGO_DSN` to your Atlas connection string.

**3. Register the connection in `config/database.php`**

```php
'mongodb' => [
    'driver'   => 'mongodb',
    'dsn'      => env('MONGO_DSN', 'mongodb://127.0.0.1:27017'),
    'database' => env('DB_DATABASE', 'relishing_roots'),
],
```

**4. Define a model**

```php
<?php

namespace App\Models;

use MongoDB\Laravel\Eloquent\Model;

class Recipe extends Model
{
    protected $connection = 'mongodb';
    protected $collection = 'recipes';

    protected $fillable = [
        'title',
        'description',
        'ingredients',
        'instructions',
        'tags',
        'prep_time',
        'cook_time',
        'image_url',
        'story',
        'user_id',
    ];
}
```

**Planned collections**

| Collection | Purpose |
| --- | --- |
| `recipes` | Recipe records |
| `ingredients` | Ingredient reference data |
| `users` | Accounts and profiles |
| `meal_plans` | Planned meals |
| `bookmarks` | Favorite recipes |
| `notes` | Personal notes and memories |

## Data model

A recipe document:

```json
{
  "title": "Mango Sticky Rice",
  "description": "A sweet, comforting dessert from my family kitchen.",
  "ingredients": ["rice", "mango", "coconut milk", "sugar"],
  "instructions": ["Cook rice", "Prepare coconut sauce", "Serve with mango"],
  "tags": ["dessert", "sweet", "family-favorite"],
  "prep_time": 20,
  "cook_time": 30,
  "image_url": "https://example.com/mango-rice.jpg",
  "story": "This recipe was taught by my grandmother during summer evenings.",
  "created_at": "2026-08-17T00:00:00Z"
}
```

`prep_time` and `cook_time` are in minutes.

## Frontend

The `frontend/` folder holds the UI. The recommended stack is **Vite + React** with **Tailwind CSS** and reusable cards for recipes and meal plans. A Vue app, a plain HTML/CSS/JS prototype, or Blade templates inside Laravel would also work.

```text
frontend/
├── src/
├── public/
├── package.json
├── vite.config.js
└── tailwind.config.js
```

## MVP scope

| Page | Purpose |
| --- | --- |
| Home feed | Browse recent recipes |
| Add recipe | Create a recipe with ingredients, steps, photo and story |
| Recipe detail | Read a recipe with its notes and story |
| Favorites | Saved recipes |
| My Journal | Profile and personal collection |
| Search and filters | Find by ingredient, keyword, meal type or tag |

## Roadmap

- [x] Scaffold the Laravel backend
- [ ] Replace the default welcome page with a Relishing Roots landing page
- [ ] Finalize the logo and color system
- [ ] Build the recipe dashboard and recipe form UI
- [ ] Create API routes for recipes and users
- [ ] Add MongoDB models and repository logic
- [ ] Connect the frontend to the API
- [ ] Add authentication (Laravel Sanctum or JWT) and profile pages

## Brand

**Tagline:** "Recipes with memory, flavor, and meaning."

**Mood:** cozy, handcrafted and heritage-inspired, rooted in cooking traditions.

| Element | Direction |
| --- | --- |
| Palette | Terracotta, cream, olive, warm brown, deep aubergine |
| Emblem | A round plate silhouette with a leaf and a spoon or fork in line art |
| Wordmark | "Relishing Roots" in a serif or script-inspired display face |
| Typography | A friendly, slightly handwritten display font with a clean sans-serif for body text |
| Finish | Subtle warm gradients that suggest homemade meals |

## Tech stack

- Laravel 12 or later (backend and API)
- MongoDB, local or Atlas
- React with Vite, or Blade (frontend)
- Tailwind CSS (styling)
- Laravel Sanctum or JWT (optional authentication)

## Contributing

Contributions are welcome. Fork the repository, create a feature branch, and open a pull request describing your change.

## License

Add a `LICENSE` file and name the license here.
