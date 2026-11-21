# dolanjack.com

Source for my personal site at [dolanjack.com](https://dolanjack.com).

It is one static page: hand-written HTML and CSS, two self-hosted fonts, and no JavaScript or build step.

## Layout

| Path | What it is |
| --- | --- |
| `site/` | Everything that gets served: `index.html`, `style.css`, fonts, and the resume PDF |
| `resume/` | LaTeX source for the public resume PDF (the phone number is left off) |
| `nginx.conf` | Server config: security headers, caching, and the `www` redirect |
| `Dockerfile` | Unprivileged nginx image that serves `site/` on port 8080 |

## Run it locally

Open `site/index.html` in a browser, or run the real container:

```sh
docker build -t dolanjack-site .
docker run --rm -p 8080:8080 dolanjack-site
```

Then visit `http://localhost:8080`.

## Update the resume

Edit `resume/Resume-JackDolan.tex`, then rebuild the PDF and copy it into the site:

```sh
cd resume
tectonic Resume-JackDolan.tex
cp Resume-JackDolan.pdf ../site/files/
```

## Deploy

The site runs as an application on my Dokploy server. Dokploy builds the Dockerfile, and a push to `main` deploys it. Cloudflare terminates TLS and sends traffic through a Cloudflare Tunnel to Traefik, which routes it to this container.

## Email address and Cloudflare

Cloudflare's email obfuscation rewrites email addresses and depends on a script it injects. The site's Content Security Policy blocks that script, so the address would show as "[email protected]". The `<!--email_off-->` comments in `index.html` tell Cloudflare to leave those parts alone. Keep them around any email address you add.

## Fonts

Source Serif 4 and IBM Plex Sans, both under the SIL Open Font License. The license texts are in `site/fonts/`.
