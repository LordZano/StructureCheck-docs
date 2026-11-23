# StructureCheck User Guide

StructureCheck is a Roblox Studio plugin for people who build many structures that must have the same shape: pets, NPCs, weapons, vehicles, tools, level modules. You choose one `Model` or `Folder` as the **reference**, choose the models to check, and StructureCheck tells you what differs.

This guide describes StructureCheck 1.0.0. The version number is shown at the top right of the plugin window.

## 1. What it compares

StructureCheck compares the **descendants** of the reference with the descendants of each **target**, recursively, by two things only:

- the exact `Name` of each instance;
- its exact `ClassName`.

It ignores:

- the order of siblings;
- the name and the class of the two root instances themselves (the reference and each target are treated as containers);
- everything else: properties, attributes, tags, script source and asset contents are never read.

Only a `Model` or a `Folder` can be a reference or a target.

The plugin is read-only. A scan never creates, changes, moves or deletes an instance. The only thing the plugin changes in Studio is the **selection**, when you click a row in the results.

## 2. Installation

> **Not yet verified.** The steps below have not been tested with the final build. If something differs on your computer, tell the person who gave you the build.

You need the plugin file from the person who gave you this build, named like `StructureCheck-<version>.rbxm`. No development tool (Rojo, VS Code, Git) is required. Only Roblox Studio is needed.

1. Save the `.rbxm` file somewhere you can find it, for example your Downloads folder.
2. Open Roblox Studio and open any place.
3. In the **Plugins** tab, click **Plugins Folder**. A Windows folder opens: it is Studio's local plugins folder. (On Windows it is usually `%LOCALAPPDATA%\Roblox\Plugins`.)
4. Copy the `.rbxm` file into that folder. Do not put it inside another folder.
5. Close Roblox Studio completely, then start it again and open a place.
6. In the **Plugins** tab, click **StructureCheck** to open or close the window. The window docks on the right the first time.

To update to a new build, close Studio, replace the old `.rbxm` in the same folder with the new one (delete the old file first so that only one copy remains), and restart Studio. To uninstall, close Studio and delete the file.

The **StructureCheck** button in the Plugins tab currently has **no icon**: it looks like an empty button. This is a known limitation of this build, and the button works normally.

If the button does not appear, check that the file is directly in the plugins folder, that only one copy is there, restart Studio, and look in the **Output** window for messages beginning with `[StructureCheck]`.

Use of StructureCheck is governed by the end user license in the [LICENSE](LICENSE) file.

## 3. Quick start

1. In Explorer, select **one** `Model` or `Folder`: your reference.
2. In the StructureCheck window, click **Set Reference From Selection**. The reference line shows a green dot with its name and path, and the status line says the reference was captured.
3. In Explorer, select the `Model`s or `Folder`s you want to check. You can select several.
4. Click **Scan Selection**.
5. Read the results (see [section 4](#4-reading-the-results)) and click a row to jump to the instance in Explorer (see [section 5](#5-going-to-an-instance)).

If you modify the reference after step 2, set it again (see [section 6](#6-keeping-the-reference-up-to-date)).

## 4. Reading the results

### The four kinds of differences

| Badge | Counter | Meaning |
|---|---|---|
| `MISSING` | Missing | The reference has an instance that the target does not have. |
| `UNEXPECTED` | Unexpected | The target has an instance that the reference does not have. |
| `TYPE` | Type | Both have an instance with the same name, but with a different class (for example `Part` in the reference and `Model` in the target). |
| `COUNT` | Count | Both have instances with the same name and the same class, but not the same number of copies (for example 3 in the reference and 2 in the target). |

### Counters

Four counters sit under **SCAN RESULTS**: **Missing**, **Unexpected**, **Type** and **Count**. Each shows how many results of that kind were found, added up over all the targets you scanned. A counter at zero stays visible, in a neutral color. A `COUNT` result counts as one, however many copies are involved. On a narrow window the counters wrap into two rows of two.

### Filters

The four counters are also **buttons**. Click a counter to hide the rows of that kind in every target; click it again to show them. A thin bar under a counter, starting at the left edge of its name and with a small gap to the bar of the next counter, means its kind is shown. All kinds are shown at first, and again after every new scan or new reference. Before a scan, or while the panel shows only a message, there is no bar: there is nothing to hide.

- The numbers on the counters, and the summary line of each target, always show the **totals**, whatever is hidden.
- Clicking a row that is still shown selects the same instances as before.
- If you hide every kind, the list says so; if a target has nothing left to show, it says so for that target.
- Hiding does not scan again and does not change your place. The filter is not kept when Studio is closed.
- There is no text search, no sorting and no filter by target.

### Rows

Each target has a header with its full path, then one row per difference:

```text
MISSING     Stats/Health     Folder
UNEXPECTED  Stats/Extra      Part
TYPE        Config/Value     Part → Model
COUNT       Skill            Part: expected 3, found 2
```

- The **badge** shows the kind of difference, in the color of its category: red for Missing, blue for Unexpected, amber for Type and violet for Count.
- The **path** is relative to the root, for example `Stats/Health`, and ends with the instance's name.
- The class information is shown in italics: the class for Missing and Unexpected, `expected → found` for Type, and the class with both counts for Count.
- A row shows at most **two lines**. If a path is long, the end of the row is cut with `…` (the class information comes last, so it goes first). Click the row to select the instance in Explorer and read its full name there (not yet confirmed visually in Studio).

If a target has no difference, its section shows a green line: **Structure matches the reference.**

### Status line

The line under the **Set Reference** button reports what just happened. Its color tells you how it went:

| Color | Meaning | Examples |
|---|---|---|
| Gray | Information | the instruction shown when the window opens |
| Green | It worked | `Reference captured…`, `Scanned 2 target(s).`, `Selected 1 instance(s) in Explorer.` |
| Amber | It worked, with something to know | selected items were ignored or skipped; a clicked instance was renamed or deleted; results are out of date |
| Red | It could not be done | nothing selected, no reference set, the reference was deleted, the scan failed |

Finding differences is not an error: a scan that reports differences is a green `Scanned N target(s).`

More detail about a problem, such as which selected items were ignored, appears in the results area.

### Copying the report

After a scan, a **Copy Report** button appears at the right of the SCAN RESULTS title. It is not there before a scan, or when the panel shows only a message.

1. Click **Copy Report**. The results list is replaced by a text box that holds the whole report as plain text.
2. Click **Select all**, then press **Ctrl+C** to copy, or select the text with the mouse and copy it. Paste it where you want (a chat, a ticket, a text file). StructureCheck itself does not write any file and does not touch the clipboard: you copy by hand. On the author's machine, copying a report of nearly 199,000 characters worked in one go.
3. Click **Back to results** to return to the list.

What the report contains:

- the plugin version, the scan time (UTC), and the reference (name, class and path);
- a line saying that the report covers **all categories**: it is always complete, whatever filter is active in the panel (the filter only changes what the list shows);
- for each target: its path, the number of differences of each kind, and one line per difference, in the same order as the list: the kind (`MISSING`, `UNEXPECTED`, `TYPE`, `COUNT`), the full path (it ends with the instance's name), the class information and the message.

```text
[MISSING] Stats/Health | Folder | Missing Folder instance named Health.
```

- A name that contains a line break, a tab or another control character is written escaped (`\n`, `\t`, `\x01`, and `\\` for a backslash), so each difference stays on one line.
- The report is built when you click the button, from the results on screen. A new scan, a new reference or a message replaces the results and closes the report.

**The report contains the full paths of your instances** (and the name of the reference). Read it before pasting it somewhere public or sending it to someone outside your team.

## 5. Going to an instance

Click a row to select the matching instance in Explorer:

- **Missing:** the instance in the **reference** that the target lacks.
- **Unexpected:** the extra instance in the **target**.
- **Type:** both instances, at once.
- **Count:** the surplus copies, on the side that has more.

Clicking only changes the Studio **selection**. It never scans again and never changes an instance.

Because the selection changed, **Scan Selection** would no longer scan your original targets. Select them again before scanning again. A hint at the top of the results reminds you.

If something changed since the scan:

- an instance that was **renamed** is still selected, with a note in the status line;
- an instance that was **deleted** is replaced by its nearest remaining parent, with a note;
- if the instance and its parents are all gone, the status line says **Not found**.

After a new scan or a new reference, the old rows are replaced and can no longer be clicked.

## 6. Keeping the reference up to date

When you click **Set Reference From Selection**, StructureCheck takes a **snapshot** of the reference's structure. The snapshot is frozen: if you add, remove or rename things inside the reference afterwards, StructureCheck does not notice. **Set the reference again** after modifying it. A new capture is the only way to replace the snapshot.

The reference line in the window follows the reference instance itself, in real time:

- `● Name (Class)` with a **green** dot, and its path on the line below: the reference is in your place. If you rename it, move it, or move one of its parents, the name and path update. A long path wraps on two lines and is cut with `…` only after that (not yet confirmed visually in Studio).
- `● Name (Class)  not in the place` with a **red** dot: the reference was removed from the place or destroyed. A scan is refused while it is gone.

This only reports where the reference instance is. It never updates the snapshot.

## 7. Themes

The window follows Studio's Dark and Light themes and changes immediately when you switch themes, including the results already displayed.

## 8. Known limits

- **Names and classes only.** Properties, attributes, tags and scripts are not compared.
- **The reference is a snapshot** (see [section 6](#6-keeping-the-reference-up-to-date)). It is not saved between Studio sessions: set it again after reopening a place or reloading the plugin.
- **Nothing is kept between sessions.** Like the reference, the results and the filter are not kept after Studio is closed. There is no history of scans, no CSV and no file export; the only way to take the results out is the text report you copy by hand (see [Copying the report](#copying-the-report)). The only filter is by kind of difference (see [Filters](#filters)).
- **Only `Model` and `Folder`.** Selecting several Models or Folders when setting the reference is refused; unsupported items in the selection are ignored with a warning. Workspace and WorldModel are ignored as unsupported roots (they are Models for Roblox).
- **Count results are summaries.** When the same name and class appear a different number of times, the surplus copies are reported as one `COUNT` result, without describing their contents. The contents of instances reported as Missing, Unexpected or Type are not compared.
- **Many copies of the same name and class.** Copies are paired so that the number of differences is as small as possible. When more than 32 copies remain unmatched on one side after the identical ones are paired, they are paired in a fixed order instead: the result is always the same, but it may show more differences than the minimum.
- **Renaming a parent of the reference** does not update the path shown in the reference line right away: it is updated at the next change to the reference, or at the next scan.
- **Undoing a deletion** of the reference in Studio brought back the same instance in our test, and the dot is meant to turn green again with it. If the dot stays red, set the reference again.
- **Performance.** The scan runs in one step: Studio does not respond while it runs. These times were measured in Roblox Studio on one Windows machine, on synthetic hierarchies of depth 5:
  - 5,000 instances, 0 to 2,000 differences: capture and comparison about 0.07 to 0.18 s; showing the 2,004 rows about 0.3 s.
  - 20,000 instances, 0 to 100 differences: about 0.4 s.
  - 20,000 instances, 5,000 differences: about 1.8 s in total, including creating the 5,004 rows.
  - 20,000 instances, 20,000 differences: about 5.7 s in total. Studio's memory use probably goes above 1 GB (an indicative measurement), and resizing the panel is jerky.

  These values come from a synthetic benchmark (the `bench/` harness of the project, run in Roblox Studio on one machine on 2026-11-18; the raw runs were not archived). They do not replace a test on a real project, and they can differ with the machine and with the structure (large numbers of copies of the same name and class were not measured). With more than a few thousand differences, compare a smaller subset instead: select a more targeted Model or Folder.
- **Filters.** Hiding or showing a category rebuilds the list; with 3,000 rows it took under about a second on the author's machine (not timed); with larger results it may freeze briefly.
- **Report size.** Studio does not accept a text of 200,000 characters or more in a text box, so the report is limited to 199,000 bytes. If the full report is larger, it keeps whole targets and whole difference lines, in order, up to that limit, and its last line says how many differences were omitted and, when some targets are not listed at all (even identical ones), how many: `N differences omitted, M targets not listed: the report is limited to 199,000 bytes.` (`1 difference`, `1 target` in the singular; the part about targets is left out when every target is listed). The totals and the number of targets at the top are always complete. To get the rest, compare a more targeted Model or Folder.
- **No toolbar icon.** The Plugins tab button shows no icon in this build (see [section 2](#2-installation)).
- **English only,** and tested on Windows.

## 9. Privacy

StructureCheck works entirely inside Studio. It makes no network request, sends no data, and stores nothing between sessions. The text report is only displayed: nothing leaves Studio unless you copy it yourself, and it contains the full paths of your instances.

## 10. Getting help

Use the page of StructureCheck on the Roblox Creator Store. Please give:

- the version number shown at the top right of the window;
- what you selected and what you clicked;
- the text of the status line, and any message beginning with `[StructureCheck]` in the **Output** window.
