# StructureCheck

A Roblox Studio plugin that checks whether `Model` and `Folder` hierarchies match a reference. Pick one model as the reference, pick the models you want to check, and StructureCheck lists what is missing, what is unexpected, and which instances have the wrong class or the wrong count.

**Version:** 1.0.0

StructureCheck is a diagnostic tool. It is read-only: a scan never changes your place.

## What it does

- Compares the descendants of the reference with each target by exact `Name` and `ClassName`, recursively. Sibling order and the name and class of the roots themselves are ignored.
- Reports four kinds of differences: **Missing**, **Unexpected**, **Type** (same name, different class) and **Count** (same name and class, different number of copies).
- Shows four counters and one clickable row per difference. Clicking a row selects the matching instance in Explorer.
- Follows the Dark and Light Studio themes, and shows whether the reference is still in your place.

It does not compare properties, attributes, tags, or script source.

## Use

Select one Model or Folder and click **Set Reference From Selection**. Select the Models or Folders to check and click **Scan Selection**. Installation, the full walkthrough, how to read the results, and the known limits are in the [user guide](USER_GUIDE.md).

## Getting help

Use the page of StructureCheck on the Roblox Creator Store. Tell us the version shown at the top of the plugin window and what you did before the problem appeared. Messages beginning with `[StructureCheck]` in Studio's Output window can help.

## More

- [User guide](USER_GUIDE.md)
- [Changelog](CHANGELOG.md)

## License

Copyright © 2026 ZanoCorp. All rights reserved. Use of StructureCheck is governed by the end user license in [LICENSE](LICENSE).
