# Deploy Astro using FTP

This action builds and deploys your [Astro](https://github.com/withastro/astro) project via FTP. 
This is a soft fork of [radenpioneer/astro-ftp-deploy](https://github.com/radenpioneer/astro-ftp-deploy) focused on lightning fast builds and updated dependencies. The action for example uses bun instead of node.js.

## Usage

### Inputs

#### Required

- `server` - The address of your FTP server, e.g `ftp.myftpserver.com`.
- `username` - The username of your FTP server.
- `password` - The password of your FTP server.


#### Optional

- `directory` - The path to upload to, on your FTP server, relative to your FTP root. Must end with trailing slash, e.g `www/`. Defaults to `public_html/`.
- `protocol` - The protocol to use on your FTP server. Accepts `ftp`, `ftps`, and `ftps-legacy`. Defaults to `ftp`.
- `port` - The port of your FTP server. Defaults to `21`.
- `path` - The root location of your Astro project inside the repository.
- `dry-run` - The FTP deploy will only log potential changes, but will not actually transfer any files. Defaults to `false`.

### Example workflow:

#### Build and Deploy to GitHub Pages

Create a file at `.github/workflows/deploy.yml` with the following content.

```yml
name: Deploy to GitHub Pages

on:
  # Trigger the workflow every time you push to the `master` branch
  push:
    branches: [ master ]
  # Allows you to run this workflow manually from the Actions tab on GitHub.
  workflow_dispatch:
  
# Allow this job to clone the repo and create a deployment
permissions:
  contents: read

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout your repository using git
        uses: actions/checkout@v3
      - name: Install, build, and upload your site output
        uses: radenpioneer/astro-ftp-deploy@v0.1.0
        with:
          server: ${{ secrets.FTP_SERVER }}
          username: ${{ secrets.FTP_USERNAME }}
          password: ${{ secrets.FTP_PASSWORD }}
          # directory: public_html/ # The path to upload to, on your FTP server, relative to your FTP root. Must end with trailing slash. (optional)
          # protocol: ftp # The protocol to use on your FTP server. (optional)
          # port: "21" # The port of your FTP server. (optional)
          # path: . # The root location of your Astro project inside the repository. (optional)
          # dry-run: false # If the ftp deploy should only log potential changes instead of actually making them. (optional)

```

### SFTP Support

SFTP is unsupported at current time.

## Attribution

- This is a soft fork of [radenpioneer/astro-ftp-deploy](https://github.com/radenpioneer/astro-ftp-deploy). Props to the author for the initial idea!
- This action uses [`SamKirkland/FTP-Deploy-Action`](https://github.com/SamKirkland/FTP-Deploy-Action) under the hood. 
