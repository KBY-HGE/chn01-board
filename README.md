# CHN-01 phone boards

Two pages the guards and the site engineer open on their phones to reach the
Google Forms. **No Claude account, no app, no login.** Plain static HTML.

```
guard.html            the guard's page — shift, 6 rounds, gate, issue
engineer.html         the engineer's page — generation, cleaning, grass, PM, issues
links.json            the ONE file holding every form link and phone number
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

On the repo page: **Add file → Upload files** → drag in all seven files →
**Commit changes**.

**3. Turn Pages on**

Open the **repository's** Settings — the tab in the repo's own row, far right,
not your profile settings. Then in the left sidebar under **Code and
automation** → **Pages** → Source **Deploy from a branch** → Branch `main`,
folder `/ (root)` → **Save**. Give it a minute.

Shortcut if you cannot find it: `github.com/<you>/chn01-board/settings/pages`

Your two links will be:

```
https://<you>.github.io/chn01-board/guard.html
https://<you>.github.io/chn01-board/engineer.html
```

**4. Put the links in — do this BEFORE sharing**

Open `guard.html` → **Setup** at the bottom → copy the whole **Links** tab out
of the register → paste it in the box → **Read the paste**.

It matches each URL to its form by name, including the six pre-filled round
links. Check what it found, add the five phone numbers, then
**Make links.json** → **Copy links.json**.

**5. Save it back to the repo**

In the repo, click **`links.json`** → the **pencil icon** → select everything
that is there (Ctrl/Cmd+A) → paste → **Commit changes**. Wait a minute and
reload the page.

**Replace the whole file. Do not paste underneath what is already there** — the
file stops being valid JSON and every button goes dead with no error message.

**6. Hand it out**

WhatsApp `guard.html` to both guards and `engineer.html` to the engineer.
They open it in Chrome → browser menu → **Add to Home screen**. It then sits
with their other apps and opens straight to the buttons.

---

## What `links.json` looks like

It ships with every key already listed and empty, so you can see exactly what
goes where. Put the link **inside the quotes**:

```json
{
  "links": {
    "shift":  "https://docs.google.com/forms/d/e/1FAIpQL.../viewform",
    "round":  "https://docs.google.com/forms/d/e/1FAIpQL.../viewform",
    "gate":   "",
    "gen":    "",
    "clean":  "",
    "grass":  "",
    "pm":     "",
    "close":  "",
    "issue":  ""
  },
  "rounds": {
    "1": "https://docs.google.com/forms/d/e/1FAIpQL.../viewform?entry.123=1",
    "2": "", "3": "", "4": "", "5": "", "6": ""
  },
  "phones": {
    "eng":    "+91 98765 43210",
    "csto":   "",
    "md":     "",
    "hescom": "1912",
    "amb":    "108"
  }
}
```

**Leave the ones you do not have yet as `""`.** Do not delete the line, and do
not delete the brackets — every key must stay. Anything left as `""` shows
greyed out on the phone with *"Link not added yet"*, which is exactly what you
want while you are part-way through.

| Key | Which form |
|---|---|
| `shift` | Shift In Out |
| `round` | Patrol Round (the plain one) |
| `gate` | Gate Log |
| `gen` | Daily Generation |
| `clean` | Module Cleaning |
| `grass` | Grass Cutting |
| `pm` | Preventive Maintenance |
| `close` | Close Issue |
| `issue` | Issue |
| `rounds` `1`–`6` | the six pre-filled round links |
| `eng` `csto` `md` `hescom` `amb` | engineer · you · MD · HESCOM fault · ambulance |

The `_note` line at the top of the shipped file is only a reminder for whoever
opens it next. The pages ignore it — keep it or delete it, either works.

---

## A custom domain (optional)

Use a **subdomain**, so your apex and the existing `os.` record are untouched.

At GoDaddy, DNS for `hgeinfra.com` → **Add record**: type **CNAME**, Name
**`solar`** (the label only, not the full host), Value **`<you>.github.io`**
(no repository name on the end), TTL 1 hour.

Then repo → Settings → Pages → **Custom domain** → `solar.hgeinfra.com` →
**Save**. Once the check passes, tick **Enforce HTTPS** — that box can take up
to 24 hours to appear while the certificate is issued.

Saving a custom domain makes GitHub add a file called `CNAME` to the repo root.
**If you ever re-upload the files and that file disappears, the custom domain
stops working** — just re-enter it in Settings → Pages.

---

## Changing anything later

Edit `links.json` in the repo. That is the whole job — no re-sending files, no
re-adding to home screens. A new phone number, a rebuilt form, a changed round
plate: one commit.

---

## Three things that will catch you out

**"Test on this phone" overrides `links.json` on that phone.**
It is there so you can check the buttons before committing. It saves to that
browser only and **wins over `links.json` until you clear it**. If one page
looks out of date while everyone else's is fine, press **Setup → Clear the
test** and reload. Never use it on a guard's phone.

**Stay signed in to Google.** The photo questions need a signed-in Google
account. If a guard signs out, the photo box silently stops working.

**A public repo is public.** `links.json` will hold your form URLs and five
staff phone numbers, readable by anyone who finds the repo. The pages carry
`noindex` so search engines skip them, but that is not privacy. Two ways round
it: give the repo an unguessable name, or host the same files on **Cloudflare
Pages**, which serves from a private repo on its free tier. The form links
themselves are not really secrets — anyone with a link can already submit, by
design — but the phone numbers are.

---

## How to tell it is working

The small grey line at the bottom of each page says where the links came from:

* **`Links from: links.json`** — the file loaded and parsed. Correct.
* **`Links from: nothing yet`** — no file, or the JSON is malformed. Check you
  replaced the whole file rather than adding to it.
* **`Links from: this phone only (test)`** — that phone is on a local test.
  Clear it.

The pages are built never to look broken: a form with no link yet still shows
its real label, greyed, saying *"Link not added yet"*, and a missing phone
number shows *"not added"* instead of dialling nothing. A half-finished setup
is obvious rather than silently dead.

---

## Tested

Driven end to end in a real browser before shipping: the empty state, a filled
`links.json`, both pages reading the same file, `tel:` links with spaces
stripped, all six round tiles, no sideways scroll at 400px, and the paste
parser against a full Links tab — including the case where **"CHN-01 Close
Issue" must not be mistaken for "CHN-01 Issue"**. 20 checks, all passing.
