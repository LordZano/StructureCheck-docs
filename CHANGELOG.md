# Changelog

## 1.0.0

- Compares the descendants of one reference `Model` or `Folder` with one or more target `Model` or `Folder` instances, recursively, by exact `Name` and `ClassName`. Sibling order and the names and classes of the roots are ignored.
- Read-only: a scan never creates, changes, moves or deletes an instance. Only the Studio selection changes, when you click a result row.
- The reference is a frozen snapshot, taken with **Set Reference From Selection**; a live indicator shows whether the reference instance is still in the place.
- Four kinds of differences: Missing, Unexpected, Type and Count, with a counter for each and one row per difference.
- Clicking a result row selects the matching instances in Explorer.
- Category filters: the four counters hide or show the rows of their kind; the counters keep the totals.
- **Copy Report**: a plain-text report of the whole scan, in a read-only text box with a **Select all** button, to copy by hand. It is limited to 199,000 bytes; when it is cut, its last line says how many differences were omitted and how many targets are not listed.
- Dark and Light themes follow Studio's theme.
- Known limits are listed in section 8 of the [user guide](USER_GUIDE.md): names and classes only, nothing kept between sessions, only `Model` and `Folder`, Count results are summaries, performance on very large results, no toolbar icon, English only.
