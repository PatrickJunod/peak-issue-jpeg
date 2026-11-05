# Peak Tools: picture tag breaks on `.jpeg` assets with Glide 4

Minimal reproduction for `studio1902/statamic-peak-tools`.

## Versions

- statamic/cms 6.40.0
- studio1902/statamic-peak-tools 9.2.3
- league/glide 4.2.0
- intervention/image 4.4.1
- laravel/framework 13.x

## Setup

    composer install
    cp .env.example .env
    php artisan key:generate

Serve the site (Herd, Valet or `php artisan serve`).

## Reproduce

- `/` renders `public/assets/test.jpg` with the Peak picture partial → works
- `/jpeg` renders the **same image** saved as `public/assets/test.jpeg` → `Invalid format provided: jpeg`

The only difference between both pages is the file extension.

## Notes

- `config/statamic/assets.php` has `'cache' => true` (like the Peak starter kit), so images are generated while the page renders and the error shows on the page itself.
- The test template is `resources/views/picture_test.antlers.html`.
- Cause: `src/Tags/Picture.php` passes `$asset->extension()` (`jpeg`) as the Glide format for the fallback `<img>`. Glide only accepts `jpg`. Glide 3 silently fell back to `jpg`, Glide 4 throws.
