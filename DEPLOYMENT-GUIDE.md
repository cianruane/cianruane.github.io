# Moving cianruane.com from Squarespace to GitHub Pages

A step-by-step guide. Allow about 45 minutes of hands-on time, plus up to a day of waiting for DNS. Nothing here is irreversible until the very last step, and your Squarespace site keeps working throughout.

## What you end up with

Your site is served for free by GitHub Pages at www.cianruane.com, with HTTPS. The domain name itself stays registered at Squarespace (roughly €20/year, the only ongoing cost), while the Squarespace *website* subscription (the expensive part) gets cancelled.

## What's in the folder

Everything the site needs is already included: the two HTML pages, the stylesheet, your headshot, your CV (`cv/CianRuane_CV.pdf`) and the five self-hosted PDFs in `papers/`. If you ever rename a PDF, update the matching `href` in the HTML.

## Step 1 — Create a GitHub account (5 min)

1. Go to https://github.com/signup and create a free account. Choose the username **cianruane** if it's available — it makes the next step tidier, but any username works.
2. Verify your email address.

## Step 2 — Create the repository (5 min)

1. Once signed in, go to https://github.com/new.
2. Repository name: **`<your-username>.github.io`** — for example `cianruane.github.io`. This exact pattern is what tells GitHub to treat it as a website.
3. Leave it **Public** (required for free Pages hosting). Don't tick any of the "initialise" boxes.
4. Click **Create repository**.

## Step 3 — Upload the site files (10 min)

1. On the new empty repository page, click the link **"uploading an existing file"**.
2. In Finder, open the unzipped site folder. Select everything inside it (`index.html`, `research.html`, `style.css`, `CNAME`, `README.md`, and the `images`, `cv` and `papers` folders) and drag them onto the GitHub upload area. Dragging the folders keeps the structure intact.
3. Wait for the progress bar to finish, then click **Commit changes** at the bottom.

You should now see the files listed in the repository, with `images/`, `cv/` and `papers/` as folders.

## Step 4 — Turn on GitHub Pages (2 min)

1. In the repository, click **Settings** (top tab) → **Pages** (left sidebar).
2. Under "Build and deployment", Source should be **Deploy from a branch**; Branch should be **main** and folder **/ (root)**. Click **Save** if you changed anything.
3. Wait a minute and refresh. A box appears saying "Your site is live at https://cianruane.github.io/".

Open that address. The new site should appear, PDFs and all. If something looks off, this is the moment to fix it — nothing about your real domain has changed yet.

## Step 5 — Tell GitHub about your domain (2 min)

Still in Settings → Pages, under **Custom domain**, type `www.cianruane.com` and click **Save**. GitHub will show a "DNS check in progress" message, which is expected: the next step fixes it.

(The `CNAME` file you uploaded does the same thing; entering it here as well just makes sure GitHub has registered it.)

## Step 6 — Point the domain at GitHub, in Squarespace (10 min)

1. Log in to Squarespace and go to **Domains** (from the home dashboard, or Settings → Domains).
2. Click **cianruane.com** → **DNS** (sometimes labelled "DNS settings" or "Advanced settings").
3. You'll see a list of records. Squarespace adds several of its own for the website; you want to **delete the ones that point at Squarespace** and add GitHub's. Specifically:

   Delete any existing **A** records for host `@` (they'll have IP addresses like `198.185.159.x` or `198.49.23.x`), and the **CNAME** record for host `www` that points at `ext-cust.squarespace.com` or similar.

   Then add these **four A records**, all with host `@`:

   | Type | Host | Value |
   |---|---|---|
   | A | @ | 185.199.108.153 |
   | A | @ | 185.199.109.153 |
   | A | @ | 185.199.110.153 |
   | A | @ | 185.199.111.153 |

   And this **CNAME record**:

   | Type | Host | Value |
   |---|---|---|
   | CNAME | www | `<your-username>.github.io` |

   (e.g. `cianruane.github.io` — no `https://`, no trailing slash.)

4. Leave any records that aren't about the website alone — in particular any **MX** or **TXT** records if you use the domain for email. (You use Gmail directly, so there probably aren't any, but don't delete them if there are.)
5. Save.

Squarespace may warn that "your domain will no longer point to your Squarespace site". That's exactly what you want.

## Step 7 — Wait, then switch on HTTPS (30 min to 24 h)

DNS changes take anywhere from minutes to a day to spread. Check progress by visiting https://www.cianruane.com — once it shows the new site, go back to GitHub → Settings → Pages. The DNS check should now show a green tick. Tick the **Enforce HTTPS** box. (If it's greyed out, GitHub is still issuing the certificate; check back in an hour.)

Test both `cianruane.com` and `www.cianruane.com` — both should land on the new site with a padlock.

## Step 8 — Cancel the Squarespace website subscription (5 min)

Only once the new site has been live on your domain for a day or so:

1. Squarespace → Settings → **Billing**.
2. Under the **website** subscription, choose **Cancel**. **Do not cancel the domain** — that's a separate line item and you want to keep it. If Squarespace offers to "delete the site", it's fine to leave it in trial/expired state for a while as a backup.
3. Make sure domain **auto-renew** stays on so cianruane.com doesn't lapse.

That's it. From now on the only bill is the domain renewal.

## Updating the site afterwards

- **Add or update a paper:** open the repository on GitHub, click `research.html`, click the pencil icon, edit the HTML (copy an existing `<article class="paper">` block as a template), and click **Commit changes**. It's live in about a minute.
- **New PDF:** open the `papers` folder in the repository, click **Add file → Upload files**, drop the PDF, commit. Then link to it as `papers/YourFile.pdf`.
- **New CV:** upload the new file to `cv/` with the same name `CianRuane_CV.pdf` and it replaces the old one; no link changes needed.
- **Bio or photo:** edit `index.html`, or upload a new `images/cian.jpg`.

You can also ask Claude to make any of these edits for you and hand back the updated file.

## If something goes wrong

- *Site shows a 404 at cianruane.github.io:* the repository name must be exactly `<username>.github.io` and `index.html` must be at the top level, not inside a subfolder.
- *DNS check keeps failing after a day:* double-check the four A records have host `@` (not `www`), and that the old Squarespace A records were removed.
- *Site works at www but not at the bare domain (or vice versa):* the four A records handle the bare domain; the CNAME handles www. Both are needed.
- *Want to go back:* Squarespace keeps your old site as long as the subscription is active, so simply restoring the original DNS records puts it back.
