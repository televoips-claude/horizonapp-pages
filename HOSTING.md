# Virtual Switchboard preview build (v0.2.1)

This is a preview of 0.3.0 for live testing. It includes the finished parts:

- **Real-Time**: contacts grouped by site and department (search, Site,
  Department, sort), contact cards with presence, headset and live lines,
  call queues with waiting and answered callers, agents, pinning, expand,
  staffing, and auto attendants.
- **Operate**: queue list with staffing bars, waiting callers longest first,
  and the agent roster.
- **Operator rail**: My Calls (hang up, park, decline, mute, record, keypad),
  Call Parks, and the Dial Pad.
- **Panels**: staff a queue, one agent's queues, expanded queue.
- **Header**: Live indicator, your status switcher (when you are an agent in
  the domain), the four live totals, and a domain picker for resellers.

Analyze, the Wallboard, and the per-user Voicemail / Call handling / Call
history panel show a "next build" note in this preview. Version 0.3.0 adds
them.

Buttons that change things act on the real phone system: membership and
Taking calls switches, Max calls, status changes, dialing, hanging up,
parking. Answer, hold, transfer and queue pickup show disabled with the
reason, because they are PATCH requests Horizon apps cannot send yet.

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
   `0.2.1`, save, then press **Deploy**. Expand the row: `approved` or
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
