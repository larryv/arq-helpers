# arq-helpers #

These scripts streamline (or, arguably, complicate) using [Arq][1] on
macOS. (The plural is aspirational, as there's currently only one. Maybe
there will be more later!) Some of them might be made to work on Windows
via [Cygwin][2] or an act of [Aslan][3], but I have neither the means nor
the inclination nor a [wardrobe][4].

To install, place them in an arbitrary location readable by any user who
wants to use them. I use `/usr/local/bin` like a schmuck, but feel free
to get creative.

For the skittish: The contents of this repository are published from the
United States of America under [CC0 1.0 Universal][5].

  [1]: https://www.arqbackup.com
  [2]: https://cygwin.com
  [3]: https://en.wikipedia.org/wiki/Aslan "'Aslan' on the English Wikipedia"
  [4]: https://www.worldcat.org/oclc/7207376 "'The lion, the witch and the wardrobe : a story for children (Book, 1950)' on WorldCat"
  [5]: https://creativecommons.org/publicdomain/zero/1.0


## arq-preflight-mirror ##

`arq-preflight-mirror` mirrors one or more files or directory trees to
a single destination directory. This is useful for backing up isolated
items in directories that should not otherwise be backed up: Instead of
[excluding][6] everything else and continually checking for new items to
deselect, the desired items can be mirrored to a separate directory that
Arq fully backs up. (It would be preferable to move the items and link
them back to the original location, but that is sometimes infeasible.)

    arq-preflight-mirror [-0hv] sources_file destination_dir

The files and directories listed in `sources_file` are mirrored to
`destination_dir` under their full paths (e.g., `/foo/bar/baz` is
mirrored to `/backup/dir/foo/bar/baz`). Directories are handled
recursively. Items in `destination_dir` are deleted if they no longer
exist under their corresponding source path.

Files are hard-linked to save disk space, so the sources must be on the
same device as `destination_dir`. Directory modes are preserved but ACLs
are not, to prevent "deny delete" ACLs from interfering with deletion.
(Such ACLs are not stripped from files because that would remove them
from the originals as well. Consequent problems must be resolved
manually.)

Paths in `sources_file` should be absolute. They must be NUL-delimited
if the `-0` option is given; otherwise, they must be LF-delimited.

The `-v` option causes the script to print extra status messages to
standard output. The `-h` option causes it to print a usage message to
standard output and exit.

The current implementation uses [GNU `cp(1)`][7], which is not included
with macOS and must be obtained from your friendly neighborhood software
distributor. Once installed, its path can be specified in the `CP`
environment variable.

  [6]: https://www.arqbackup.com/docs/arqbackup/pages/excludes.html "Arq Help: Excluding Items Within a Folder"
  [7]: https://www.gnu.org/software/coreutils "GNU Coreutils"
