# Jones Uncommon Website — How to Edit It

There are three ways to change this website. Almost always you want the first.

| | Who it is for | How |
|---|---|---|
| **The website editor** | Anyone | Go to **jonesuncommon.com/edit/**, sign in with GitHub, click the words or pictures you want to change, press **Publish**. |
| **The advanced editor** | Anyone, for the odd setting the first one doesn't show | **jonesuncommon.com/admin/** - a form for every setting. Same sign-in. |
| **The files** | Someone comfortable with a text editor | Edit the files in this folder and push to GitHub. |

Whichever you use, the website rebuilds itself and is live about a minute later.
There is nothing to upload.

---

## Using the website editor

Open **/edit/** and sign in. You'll see your real page.

### Changing words and pictures

- **Click any words** and type. Press **Enter** when you're done, or **Esc** to
  cancel.
- **Click any picture** and choose **Replace picture**. The editor makes it the
  right size for you, so a photo straight off your phone is fine.
- After a new picture, fill in (or check) its **description** - that's what a
  blind visitor hears and what Google reads.
- Anything you can't click - a link's destination, a colour choice - is in the
  **settings panel** on the right, which opens when you click the section.

### Sections

A page is a stack of sections. Point at one and a small toolbar appears:
**Move up**, **Move down**, **Duplicate**, **Remove**, **Settings**, and
**+ Add section below** (which shows every kind of section to pick from).

### Pages, menu and contact details

- **Page** (top left) switches pages. **+ Add a page...** at the bottom of that
  list makes a new one and can put it in the menu for you.
- **Page settings** is the page's title and description for Google, with a
  preview of how it will look there. **Delete this page** is at the bottom.
- **Site-wide...** is everything that appears on every page: contact details,
  the menu, and so on.

### Publishing

Nothing goes live until you press **Publish**. Until then your changes are kept
on your computer - close the tab and they'll be offered back to you next time.
**Undo** takes back any step.

If something's missing (an empty heading, a picture without a description),
Publish tells you what and takes you to it.

Changed something by mistake after publishing? **History** shows every earlier
version of the page. Look at one, press **Restore this version**, then Publish.

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

In the website editor, pictures are resized for you. **In the advanced editor
they aren't**: resize a photo to about 1000px on its longest edge before you
upload it there. A straight-off-the-phone photo is roughly ten times bigger
than the page needs and will make the site slow.

Either way, give every picture a description that says what's in it. Be
specific and skip generic filler.

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
