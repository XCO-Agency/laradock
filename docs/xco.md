# XCO fork

This repository is [XCO Agency](https://github.com/XCO-Agency)'s fork of [laradock/laradock](https://github.com/laradock/laradock). The README below the XCO banner, and the Hugo sources in `DOCUMENTATION/`, are the upstream Laradock text from the snapshot this fork was cut from. They describe Laradock in general. Current XCO apps are run from other repositories.

Checked 2026-11-22.

## Status

The fork is dormant.

- GitHub created the public fork on 2023-10-27. The only branch is `master` (there is no `staging`). The last push is `3d68b6f` on 2026-08-24 (`pushed_at` 2026-08-24T12:08:02Z).
- XCO's portfolio map in `XCO-Agency/ai-config` (`brain/projects/README.md`, tier list current through 2026-11-22) places `laradock` in the Dormant tier: parked, and not a place for product work. The same file lists it in `platform/hermes/repos.txt` only so fleet watch loops still see the repository.
- Coolify production has no application whose name or git repository is this fork. A name search for `laradock` and for `dev-lab` returned nothing. The applications that are running are product repos (Shopify apps, and Laravel apps such as Corex, Sappo, Shopeye, and Claium) under the Shopify Apps and Laravel Apps projects.
- Coolify staging returned no resources for a `docker` search, so this compose file is not deployed there either.
- No repository in the org is named `dev-lab` (code search for that string returned nothing). The local Docker repository is [`XCO-Agency/shopify-docker-lab`](https://github.com/XCO-Agency/shopify-docker-lab), created 2025-07-15 and pushed on 2026-11-22. Its `dev/` directory is a Shopify CLI container (`docker-compose up -d` in that folder, then `./shopify-dev shell`). Its `production/laravel-frankenphp-docker-image` directory builds `xcoagency/laravel-frankenphp-octane`. [`Laravel-SAAS-Starter`](https://github.com/XCO-Agency/Laravel-SAAS-Starter) starts from that image (`FROM xcoagency/laravel-frankenphp-octane:latest`) and documents a host install (`composer install`, `npm install`, `php artisan serve` / `composer run dev`) with PHP 8.4. Production deploys go through Coolify.
- That same portfolio map also lists `shopify-docker-lab` as Dormant. The image name in the Laravel starter Dockerfile and the 2026-11-22 push say the Docker images are still referenced. This note does not reclassify that repository.

## Why XCO keeps the fork

The fork is a frozen copy of upstream Laradock, kept in the XCO org with one local CI change. Service definitions, Dockerfiles, and `.env.example` are the upstream files. Nothing in them was written for an XCO app.

The only commit after the fork point is `3d68b6f` (2026-08-24, Anass Ez-zouaine), message `ci: make workflows manual-only (workflow_dispatch)`. It drops the `push`, `pull_request`, and weekly `schedule` triggers from `.github/workflows/main-ci.yml`, and the `push` trigger from `.github/workflows/build-deploy-docs.yml`. Both workflows run only from a manual `workflow_dispatch`. That keeps upstream's CI and docs-deploy jobs from running on this fork. It does not change a container.

## What changed versus upstream

| Item | Value |
| --- | --- |
| Fork point | `b958fc096c1c43fd5ed73eeef727fe636bd1b789` (`b958fc0`), upstream merge of PR #3421 on 2023-10-19, "Fix build arguments for JDK" |
| Files different from that commit | `.github/workflows/main-ci.yml`, `.github/workflows/build-deploy-docs.yml` |
| Upstream `master` since the fork point | 391 commits ahead, 0 behind, tip `0fd82a5` on 2026-11-22 (upstream `pushed_at` 2026-11-10). Compare: https://github.com/laradock/laradock/compare/b958fc0...master |
| Compose services in this snapshot | 101, unchanged from the fork point |
| `.env.example` keys | 715 lines of the form `KEY=value`, unchanged from the fork point |
| Upstream README commit still in this tree | `955d66d`, 2023-10-15 00:56 +0800 (2023-10-14 16:56 UTC), "Update README.md (#3463)" |

There is no `docs/` directory in the upstream tree. Upstream documentation lives in `DOCUMENTATION/` and at https://laradock.io. This file is the XCO addition.

## Services and keys

This repository does not record an XCO service list or an XCO `.env`. There is no committed `.env`, no compose override, and no XCO comment in the service directories. `.env.example` is the upstream template. Defaults in that file include `PHP_VERSION=7.4`, `APP_CODE_PATH_HOST=../`, and `MYSQL_VERSION=latest`. The Laravel starter's documented requirement is PHP 8.4, so those defaults are the 2023 Laradock template, not the stack XCO apps use now.

The commands below are the upstream demo from `DOCUMENTATION/content/introduction/index.md` and `DOCUMENTATION/content/getting-started/index.md`. They are how this snapshot starts if someone still checks it out. They are not an XCO profile.

```bash
cp .env.example .env
docker-compose up -d nginx mysql phpmyadmin redis workspace
docker-compose exec workspace bash
```

`nginx` depends on `php-fpm`, so naming `nginx` starts PHP-FPM as well. The documented demo is PHP, NGINX, MySQL, Redis, and Composer inside `workspace`, served at `http://localhost`. A Laravel project's own `.env` would then use `DB_HOST=mysql` and `REDIS_HOST=redis`, as the upstream introduction says. Every other service stays off until it is named in `docker-compose up`. The full set is the 101 services in `docker-compose.yml` (workspace, php-fpm, nginx, apache2, mysql, mariadb, postgres, redis, mongo, elasticsearch, and the rest of the upstream catalogue).

The 715 keys in `.env.example` are upstream install switches (`WORKSPACE_INSTALL_*`, `PHP_*`, per-service versions and ports). This fork does not document them individually. Read `.env.example` next to `docker-compose.yml`, or the upstream docs at https://laradock.io/documentation/.

## What this note could not verify

- Whether any developer machine still has this checkout running. No host `.env` is in git, Coolify has no laradock application, and the only org code hits for the string `laradock` are the fleet inventory and the dormant-tier line in `ai-config`.
- Why the fork was created on 2023-10-27, beyond what the git history shows: a full upstream history ending at `b958fc0`, then silence until the 2026 CI commit. No issue, PR, or commit message on this repo states a product reason.
- The portfolio map calls `shopify-docker-lab` Dormant while `Laravel-SAAS-Starter` still names its FrankenPHP image. Both facts are recorded above; the map was not edited from this repo.
- The "391 commits" figure is the GitHub compare of `b958fc0` to `laradock/laradock` `master` on 2026-11-22. A later upstream push changes that count.
