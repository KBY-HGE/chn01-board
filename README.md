# CHN-01 phone boards

Two pages the guards and the site engineer open on their phones to reach the
Google Forms. **No Claude account, no app, no login.** Plain static HTML on
GitHub Pages.

```
guard.html            the guard's page — shift, 6 rounds, gate, issue
engineer.html         the engineer's page — generation, cleaning, grass, PM, issues
links.json            the ONE file that holds every form link and phone number
guard.webmanifest     so "Add to Home screen" behaves like an app
engineer.webmanifest
.nojekyll             stops GitHub trying to build this as a blog
```

Both pages read **the same `links.json`**. Edit that one file and every phone
gets the change on its next open. Nothing else ever needs touching.

---

## Put it online — about five minutes

**1. Make the repo**

github.com/new → name it `chn01-board` → **Public** → Create.
(GitHub Pages needs a public repo on the free plan. See the privacy note below.)

**2. Upload these files**

On the repo page: **Add file → Upload files** → drag in all six files above →
**Commit changes**.

**3. Turn Pages on**

**Settings → Pages** → Source **Deploy from a branch** → Branch `main`,
folder `/ (root)` → **Save**. Give it a minute.

Your two links will be:

```
https://kby-hge.github.io/chn01-board/guard.html
https://kby-hge.github.io/chn01-board/engineer.html
```

(Replace `kby-hge` if you create the repo under a different account.)

**4. Put the links in — do this BEFORE sharing**

Open `guard.html` on your own phone or laptop → **Setup** at the bottom →
copy the whole **Links** tab out of the register → paste it in the box →
**Read the paste**.

It matches each URL to its form by name, including the six pre-filled round
links. Check what it found, add the five phone numbers, then
**Make links.json** → **Copy links.json**.

**5. Save it back to the repo**

In the repo, click `links.json` → the pencil icon → select all, paste, →
**Commit changes**. Wait a minute and reload the page. Every button lights up.

**6. Hand it out**

WhatsApp `guard.html` to both guards and `engineer.html` to the engineer.
They open it in Chrome → browser menu → **Add to Home screen**. It then sits
with their other apps and opens straight to the buttons.

---

## Changing anything later

Edit `links.json` in the repo. That is the whole job — no re-sending files, no
re-adding to home screens. A new guard's phone number, a rebuilt form, a
changed round plate: one commit.

---

## Three things that will catch you out

**"Test on this phone" overrides `links.json` on that phone.**
It is there so you can check the buttons before committing. It saves to that
browser only and **wins over `links.json` until you clear it**. If a page looks
out of date and everyone else's is fine, press **Setup → Clear the test** and
reload. Don't use it on the guards' phones.

**Stay signed in to Google.** The photo questions need a signed-in Google
account. If a guard signs out, the photo box silently stops working.

**A public repo is public.** `links.json` will hold your form URLs and five
staff phone numbers, readable by anyone who finds the repo. The pages carry
`noindex` so search engines skip them, but that is not privacy. Two ways to
avoid it if you care: give the repo an unguessable name, or host the same
files on **Cloudflare Pages**, which serves from a private repo on its free
tier. The form links themselves are not really secrets — anyone with a link
can already submit, by design — but the phone numbers are.

---

## What the pages do when something is missing

They are built to never look broken. With no `links.json` at all, or a form
link not yet filled in, that button still renders with its real label and says
**"Link not added yet"**, greyed and not tappable. A missing phone number shows
**"not added"** instead of dialling nothing. So a half-finished setup is
obvious rather than silently dead.

Small line at the bottom of each page says where the links came from —
`links.json`, or `this phone only (test)`. Useful when one phone disagrees
with the others.

---

## Tested

Driven end to end in a real browser before shipping: empty state, a filled
`links.json`, both pages reading the same file, `tel:` links with spaces
stripped, all six round tiles, no sideways scroll at 400px, and the paste
parser against a full Links tab — including the case where
**"CHN-01 Close Issue" must not be mistaken for "CHN-01 Issue"**. 20 checks,
all passing.
