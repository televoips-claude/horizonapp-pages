# Virtual Switchboard for a second reseller on the same platform (v0.3.1)

This folder is the built app for a **second install** of the TeleVoIPs Virtual
Switchboard on a NetSapiens platform that already runs the first one. It is the
same app, built under its own name, because the platform requires every app's
webpack module name to be unique across the whole platform:

| | First install | This install |
|---|---|---|
| Webpack module | `televoipsSwitchboard` | `televoipsSwitchboard2` |
| App id (derived by the platform) | `televoips-switchboard` | `televoips-switchboard2` |
| Page | Apps -> Virtual Switchboard (`/apps/switchboard`) | Apps -> Virtual Switchboard (`/apps/switchboard2`) |

The route id, the page path, webpack's chunk-loading name and the browser
storage prefix all differ too, so the two can run side by side (a platform
admin who can see both gets two menu entries, not a clash).

## 1. Host the files on GitHub Pages

1. Create a new **public** GitHub repository (for example `switchboard-pages`).
   Pages from a private repo needs a paid plan, and the files are public either
   way: Horizon's verifier and every browser fetch them without credentials.
   Nothing secret is in them.
2. Upload **everything in this folder**, keeping the names exactly as they are,
   including `.nojekyll` (it stops GitHub Pages hiding files). Commit to `main`.
3. **Settings -> Pages**: Source "Deploy from a branch", branch `main`, folder
   `/ (root)`, Save. Wait for the "pages build and deployment" run in the
   **Actions** tab to go green (usually under a minute).
4. Open `https://<github-user>.github.io/<repo>/remoteEntry.js` in a browser. If
   it shows JavaScript, that URL is your **remote entry URL**.

`*.github.io` is already an approved origin on this platform, so no operator
change is needed.

## 2. Register and deploy in Horizon

Signed in as the second reseller's administrator:

1. **Platform -> UI SDK management -> Registered Apps -> Add App**:
   - **Name**: `TeleVoIPs Virtual Switchboard`
   - **Webpack module**: `televoipsSwitchboard2` (exactly; it is baked into the
     bundle and cannot be changed after registration)
   - **Remote entry URL**: the URL from step 1.4
   - **Version**: `0.3.1`
   - **Enabled**: yes
2. Save, then press **Deploy** on the app's row. Deploy fetches, verifies and
   pins the bundle; registration alone shows nothing.
3. Expand the row: `approved` or `flagged` both load; `rejected` lists what to fix.
4. Reload Horizon and open **Apps -> Virtual Switchboard**.

## Updates

Each new release comes as a new zip. Replace the files in the repo, wait for
Pages, then in Registered Apps set **Version** to the new number, save and press
**Deploy**. The version must change on every release: Horizon pins a hash of the
bytes it verified, so new bytes under an old version fail that check and the app
silently disappears. Do the upload and the Deploy close together.
