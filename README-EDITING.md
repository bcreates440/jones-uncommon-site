# Jones Uncommon Website — How to Edit It

There are two ways to change this website. Most of the time you want the first.

| | Who it is for | How |
|---|---|---|
| **The website editor** | Anyone | Go to **jonesuncommon.com/admin/**, sign in with GitHub, change the words, press Publish. |
| **The files** | Someone comfortable with a text editor | Edit the files in this folder and push to GitHub. |

Either way the website rebuilds itself and is live about a minute later. There is
nothing to upload.

---

## Using the website editor

Open **/admin/** and sign in. You will see:

- **Pages** — Home, Work, Services, Contact
- **Site-wide details** — contact details, the team, and the navigation menu

### Editing a page

A page is a stack of **sections**. Open a page and you will see them listed:
"Hero banner", "Card grid", "Writing", and so on.

- Click a section to open it and change the words.
- **Drag the handle** on the left to move a section up or down.
- Use the **bin icon** to delete a section.
- **Add Sections** at the bottom adds a new one — pick the kind you want.

Press **Publish** when you are done. That is it.

### The shortcuts

Inside any piece of writing you can type these, and they fill themselves in
from **Site-wide details**:

| Type this | You get |
|---|---|
| `[[org]]` | Jones Uncommon |
| `[[phone]]` | the phone number |
| `[[email]]` | the email address |
| `[[address]]` | the address / service-area line |

### Photographs

Use the **Photograph** box in any section that has one. When you add a picture,
fill in **Photo description** — that is what a blind visitor hears and what
Google reads.

**Resize a photo to about 1000px on its longest edge before you upload it.**

---

## The folders

```
_content/     the 4 pages - the words
_data/        facts used across the whole site
_includes/    header, footer, and the section designs
_layouts/     the shape every page shares
admin/        the website editor
css/          all the styling
images/       every photograph on the site
_site/        the built website (created automatically - never edit this)
```

---

## Editing the files directly

### Preview it on your own computer

```
bundle exec jekyll serve
```

Then open **http://localhost:4000**. It rebuilds as you save.

### Before you push: run the check

```
ruby check.rb
```

It prints `ALL CHECKS PASSED` or tells you exactly what is wrong.

### The one rule when editing files by hand

**The website editor rewrites a page from the field list in
`admin/config.yml`.** If you add a new setting to a page by hand and do not
add it to `admin/config.yml`, the next editor save **silently deletes it**.
`ruby check.rb` catches exactly this. Run it.

---

## Common jobs

### Fill in your own voice

Three spots are placeholder text right now, all in `_content/index.html`:
the About paragraph, and the testimonial callout in the "Recent work"
section. Open the page in the editor and replace them — search for the word
"PLACEHOLDER" if editing the files directly.

### Change the phone number or email

**Site-wide details → Contact details & links.** The phone lives in three
fields that must all change together: the readable one, the dialable one
(`+1…`), and the button link (`tel:+1…`). Same for email and `mailto:`.
Everything else — the footer, the contact page, the call button — follows
automatically.

There is no contact form on this site, on purpose: the phone number and
email are published directly so people reach a person, not a queue.

### Add a new page

1. **Pages → New Page** in the editor.
2. Set the **File name** and **Web address**.
3. Fill in the tab title and search description, then build it from sections.
4. Add it to **Site-wide details → Navigation menu**.
5. Add it to `sitemap.xml`.

---

## Changing how the site looks

Everything is set at the top of `css/styles.css`, in the `:root` block —
currently the palette inherited from the site template, not a considered
Jones Uncommon brand choice. Change `--navy` / `--red` / `--gold` and every
button, heading accent and link changes with it, sitewide.
