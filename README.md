# filament:assets publishes executable files

Reproduction for a Filament bug: since v5.8.3, `filament:assets` publishes every asset with mode `755` instead of `644`.

This repository is the [5.x issue reproduction template](https://github.com/filamentphp/issue-reproduction-template/tree/5.x) with two changes:

- `composer.lock` pins Filament v5.9.0.
- The assets published by v5.9.0 are committed in `public/` with mode `644`, as Filament published them before v5.8.3.

## Steps

```bash
git clone https://github.com/jhm-ciberman/filament-assets-executable-mode.git
cd filament-assets-executable-mode
umask 0022
composer install
git diff --summary
```

## Result

`composer install` runs `filament:upgrade`, which republishes the assets. Every committed asset shows a mode change, with no content change:

```
 mode change 100644 => 100755 public/css/filament/filament/app.css
 mode change 100644 => 100755 public/js/filament/filament/app.js
 ...
```

## Expected

No changes. The published assets keep mode `644`.
