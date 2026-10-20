# casks.kavaz.org

Landing page for [casks.kavaz.org](https://casks.kavaz.org). It lists the Mac apps in the [tarikkavaz/homebrew-tap](https://github.com/tarikkavaz/homebrew-tap) Homebrew tap, with an install command for each and a short guide to installing Homebrew.

The same host also serves the app zips that the tap's casks download, for example `https://casks.kavaz.org/cue/Cue-0.1.1.zip`.

## Contents

- `index.html`: the whole page. Plain HTML, CSS and a small script for the copy buttons. No build step, no dependencies.
- `icons/`: one 128px PNG per app, taken from the app's `AppIcon.icns`.

## Server

The page is served by nginx on `tarik.kavaz.org` (user `deployer`). The web root is `/home/deployer/casks`, which also holds the app folders with the release zips (`correctify/`, `cue/`, `posta/`, `virtualremote/`).

The nginx site is `/etc/nginx/sites-available/casks.kavaz.org`. Its `location /` uses `try_files $uri $uri/ =404;` so that `/` serves `index.html`. Directory listing is off.

## Deploy

Copy the page and icons into the web root. Do not use `--delete`, because the zip folders live in the same directory.

```sh
rsync -av -e "ssh -T -o RemoteCommand=none" index.html icons tarik.kavaz.org:/home/deployer/casks/
```

The `-T -o RemoteCommand=none` options are needed because the `tarik.kavaz.org` SSH host entry sets a `RemoteCommand`.

## Adding an app

1. Add the cask to the tap repository.
2. Copy the app icon into `icons/` as a PNG named after the cask.
3. Add a card to `index.html` with the description, macOS requirement, install command and update command.
4. Deploy with the command above.

## Updating an icon

Convert the icon from the installed app:

```sh
sips -s format png -z 128 128 /Applications/<App>.app/Contents/Resources/AppIcon.icns --out icons/<cask>.png
```
