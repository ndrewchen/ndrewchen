# Deploying ndrewchen.com

The site is five static files: `index.html`, `pfp.webp`, `favicon.svg`, `preview.png` and `robots.txt`.
GitHub Pages serves them at https://ndrewchen.com from the `main` branch of this repo.

## Making a change

Edit, commit, and push to `main`. The **Deploy site** workflow publishes the new version in about a minute.
Only the five files above are published; everything else in the repo stays private to the repo.

## Undoing a change

- Revert the commit (`git revert <commit>`) and push. The previous version is live again in about a minute.
- Or, in the repo's **Actions** tab, open an earlier **Deploy site** run and choose **Re-run all jobs**.

## Recovering

There is no database and no visitor data. Everything is in this repo, on GitHub and in your local clone.
A full restore is: push `main` (or re-run the latest **Deploy site** run).

## Monitoring

The **Uptime check** workflow loads the site once a day and checks that the page and photo come back.
If it fails, GitHub emails you. GitHub pauses scheduled workflows in public repos after 60 days with no
commits; if you get a notice about that, re-enable it from the Actions tab.

## Cost

$0 for hosting: GitHub Pages is free for public repos, and no billing account is attached, so there is
nothing to overspend. The only cost is the domain renewal at Squarespace Domains (next due 2027-09-06).

## DNS (managed in Squarespace Domains)

| Host | Type | Value |
|---|---|---|
| `@` | ALIAS | `ndrewchen.github.io` |
| `www` | CNAME | `ndrewchen.github.io` |

The ALIAS record makes Squarespace hand out GitHub Pages' current IPv4 and IPv6 addresses, so it stays
correct if GitHub ever changes them. These two records replaced the Squarespace parking records. The
`_domainconnect` record is Squarespace's one-click setup helper; leave it.

If you ever move DNS to a provider without ALIAS support, use these instead of the ALIAS record:
A records `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
(optionally AAAA `2606:50c0:8000::153` through `2606:50c0:8003::153`).

## Who can see what

- The site is public and open to search engines. `robots.txt` asks AI training crawlers to stay out;
  well-behaved ones honor it, others may not.
- The repo is public, because GitHub Pages on a free account requires it. It contains only the site and
  these deploy files: no secrets, keys, or personal data beyond what the site itself shows.
- This repo is named after your username, so GitHub treats it as your profile repo. Adding a `README.md`
  would show it on github.com/ndrewchen; this file is named `DEPLOY.md` to avoid that.
- Outside services the page loads from: Google Fonts (the name's typeface) and cdnjs (the WebGL library
  for the tree). Both see visitors' IP addresses.

## Change log

Gaps closed while preparing the deploy:

- Made `index.html` a complete standalone page (doctype, language, viewport, charset, base margins),
  instead of relying on the claude.ai viewer to supply them.
- Added search and link-preview tags with real `https://ndrewchen.com/` URLs, a canonical link, and
  light/dark browser theme colors. Removed the staging file `head-tags.html`.
- Added `robots.txt`: search engines allowed, AI training crawlers asked to stay out.
- Added the **Deploy site** workflow: publishes only the five site files, one deploy at a time.
- Added the **Uptime check** workflow: daily check, emails on failure.
- Added `.gitignore` for macOS `.DS_Store` files. Exploration pages stay out of the repo via
  `.git/info/exclude` on this machine.

## Tradeoffs

This setup optimizes for **low effort and zero cost**: no new accounts, no keys to manage, and every
change deploys by pushing to `main`.

- For a **private repo**, move hosting to Firebase Hosting or Cloudflare Pages (free), or upgrade to
  GitHub Pro (paid). Drawback: a new account or a deploy key to manage.
- For **no outside requests**, copy the font and the WebGL library into this repo. Drawback: about
  100 KB more to download and occasional manual updates.
- For **stronger domain protection**, verify `ndrewchen.com` in GitHub's Pages settings. This stops
  anyone else from attaching the domain to their own GitHub site.
