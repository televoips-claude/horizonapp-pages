# Updating the Virtual Switchboard in Horizon (v0.3.0)

This folder is the built app: the exact static files Horizon fetches. There is
nothing to build. `remoteEntry.js` is the file Horizon loads, the hashed `.js`
files are its chunks, the `.map` files are required (the platform rejects a
bundle without source maps), and `.nojekyll` stops GitHub Pages filtering
anything.

Your lab install already points at:

```
https://televoips-claude.github.io/horizonapp-pages/remoteEntry.js
```

Keep that URL. To update it:

1. **Replace the files in the `horizonapp-pages` repo.** Delete the old
   `*.js`, `*.js.map`, `*.LICENSE.txt`, `remoteEntry.js` and `index.html`, then
   upload everything in this folder (keep the file names exactly as they are,
   including `.nojekyll`). Commit to `main`.
2. **Wait for Pages.** The repo's **Actions** tab shows "pages build and
   deployment". When it is green (usually under a minute), open the URL above
   and confirm the file changed.
3. **Deploy the new version in Horizon.** Platform -> UI SDK management ->
   Registered Apps -> **TeleVoIPs Virtual Switchboard**: set **Version** to
   `0.3.0`, save, then press **Deploy**. Expand the row: `approved` or
   `flagged` both load; `rejected` lists what to fix.
4. **Open it.** Reload Horizon and go to **Apps -> Virtual Switchboard**.

Why the version must change: Horizon pins a hash of the bytes it verified. New
bytes under the old version fail that check in every browser and the app
silently disappears. Do steps 1 to 3 close together for the same reason.

Deleting the old chunks is tidy but not required for correctness:
`remoteEntry.js` only references the new file names.

## Can the repo be private?

The served files are always public: Horizon's verifier and every user's
browser fetch them without credentials. GitHub Pages from a private repo needs
a paid plan, and the published site stays public either way. Nothing secret is
in these files (no keys, no tokens, no customer data); they are the same
JavaScript every Horizon user's browser downloads.
