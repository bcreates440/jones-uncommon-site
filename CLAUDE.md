# Jones Uncommon site — primer

Read `RUNBOOK.md` first — it has the full picture (status, URLs, accounts,
file map, what will bite you, pending tasks).

Before every push: run `ruby check.rb`. It must say `ALL CHECKS PASSED`.
It exists because the website editor (Decap CMS) rewrites a whole page from
`admin/config.yml`'s field list every time it saves — add a field to a
template by hand without adding it there too, and the next editor save
silently deletes it. `check.rb` catches this before it ships.

The private ops repo for this business is at
`C:\Users\Blake Jones\jones-uncommon\ops`. After completing any change
here, log it: append a dated entry to
`C:\Users\Blake Jones\jones-uncommon\ops\clients\jones-uncommon\request-log.md`
(format is at the top of that file), and update
`C:\Users\Blake Jones\jones-uncommon\ops\master-index.md` if status changed.
