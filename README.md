# The AO Lab website

Hugo site for photography notes and resources related to The AO Lab YouTube channel.

## Local preview

Install Hugo Extended, then run:

```sh
hugo server
```

Open the local address shown by Hugo.

## GitHub Pages

1. Replace `baseURL` in `config.toml` with the final GitHub Pages URL.
2. Push the repository to GitHub, using the `main` branch.
3. In repository settings, set Pages to `GitHub Actions` as the source.
4. The workflow in `.github/workflows/hugo.yml` will build and deploy the site.

## Good future companion posts

- ART installation on Linux and common fixes
- Installing Darktable correctly on Linux
- Training a RawNIND denoise model for Darktable
- Geotagging photos with Darktable
- Focus stacking on Linux
- Compressing RAW photo archives
