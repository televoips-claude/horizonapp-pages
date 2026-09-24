# Host these files on GitHub Pages

This folder IS the built app - the exact static files Horizon needs to fetch.
There is nothing to build. You just serve these files. `remoteEntry.js` is the
file Horizon loads; the hashed `.js` are its chunks; the `.map` files are
required (the platform rejects a bundle with no source maps); `.nojekyll` stops
GitHub from filtering anything.

## Put them in your repo

Easiest, keeps your source repo intact - use a `docs/` folder:

1. In `televoips-claude/horizonapp`, create a folder named **`docs`** and upload
   **everything in this folder** into it (so you have `docs/remoteEntry.js`,
   `docs/main.*.js`, the `.map` files, `.nojekyll`, etc.). Keep the file names
   exactly as they are.
2. Settings -> Pages -> Build and deployment:
   - Source: **Deploy from a branch**
   - Branch: **main**, folder: **/docs** -> **Save**
3. Wait ~1 minute. Pages publishes.

(If you would rather not keep source and build together, make a fresh repo, put
these files at its **root**, and select folder **/(root)** instead. Same result.)

## Your remote entry URL

```
https://televoips-claude.github.io/horizonapp/remoteEntry.js
```

Check it is live and sends CORS before registering it in Horizon:

```bash
curl -sI https://televoips-claude.github.io/horizonapp/remoteEntry.js \
  | grep -iE 'http/|content-type|access-control-allow-origin'
# want: 200, application/javascript, access-control-allow-origin: *
```

Then register that URL in Horizon (Platform -> UI SDK management -> Add App):
- Webpack module: `televoipsSwitchboard`  (exact - do not change)
- Remote entry URL: the URL above
- Version: `0.1.0`
- Enabled: yes

Save, then press **Deploy** on the row. Open a fresh Horizon session ->
**Apps -> Virtual Switchboard**.

## Updating later

These are compiled bytes, so a change to the app means a new build. Ask me for a
fresh `horizonapp-pages` zip, replace the files here with the new ones, bump the
**Version** in Horizon (e.g. `0.1.1`), and press **Deploy** again. The version
must change or Horizon keeps serving the old bytes.

## Can the repo be private?

Yes, with a caveat that is not GitHub's fault but a fact of how this works:

- **The served files are always public.** Horizon verifies the bundle by
  fetching it server-side, and every user's browser fetches it too, both without
  logging in. So `remoteEntry.js` has to be reachable without auth wherever it
  lives. (This is true of any web-served JavaScript.)
- **The repo (your source) can be private:**
  - On **GitHub Pro / Team / Enterprise**, Pages will publish from a **private
    repo** - the repo stays private, the site is still served publicly. This is
    what you want.
  - On **GitHub Free**, Pages needs a **public repo**. Two ways around it: make
    the repo public, or host these files on your own AWS instead (below) and
    keep the repo private anywhere.
  - Do NOT use GitHub Enterprise "private Pages" visibility - that access-gates
    the site, which would stop Horizon and browsers from fetching it.

## Alternative: host on TeleVoIPs AWS (fully private repo, on your own domain)

Since you run AWS for Engage, the tidiest long-term home is your own S3 +
CloudFront on a `televoips.com` subdomain. Then the source repo can be private
anywhere and the files sit on infrastructure you own. It is a few more steps
(bucket, CloudFront distribution, add the origin to Horizon's approved-CDN-origins
list, which you can edit as a reseller). Ask me and I will write the exact setup.
