<!--
    README.markdown
    ---------------

    SPDX-License-Identifier: CC0-1.0

    Written in 2019, 2023 by Lawrence Velazquez <vq@larryv.me>.

    To the extent possible under law, the author has dedicated all
    copyright and related and neighboring rights to this software to the
    public domain worldwide.  This software is distributed without any
    warranty.

    You should have received a copy of the CC0 Public Domain Dedication
    along with this software.  If not, see
    <https://creativecommons.org/publicdomain/zero/1.0/>.
-->


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
Arq backs up in full. (It would be preferable to move the items and link
them back to the original location, but that is sometimes infeasible.)

    arq-preflight-mirror [-0hv] source_list destination_dir

The files and directories listed in `source_list` are mirrored to
`destination_dir` under their full paths (e.g., `/foo/bar/baz` is
mirrored to `/backup/dir/foo/bar/baz`). Files are hard-linked to save
disk space, so the sources must be on the same device as
`destination_dir`. Directories are processed recursively.

Items in `destination_dir` are deleted if they no longer exist under
their corresponding source path. To prevent "deny delete" ACLs from
interfering with this, directory ACLs are not preserved under
`destination_dir`. (This measure cannot be applied to files because they
are hard-linked, so problems stemming from file ACLs must be resolved by
the user.)

Options are:

-   `-0`: Delimit paths in `source_list` with NUL instead of the
    default LF.
-   `-h`: Print a usage message to standard output and exit.
-   `-v`: Print extra status messages to standard output.

The current implementation uses [GNU `cp(1)`][7], which is not included
with macOS. After obtaining it from your friendly neighborhood software
distributor, specify its path in the `CP` environment variable. If
desired, a alternate [rsync][8] installation can be used via the `RSYNC`
environment variable.

  [6]: https://www.arqbackup.com/docs/arqbackup/pages/excludes.html "Arq Help: Excluding Items Within a Folder"
  [7]: https://www.gnu.org/software/coreutils "GNU Coreutils"
  [8]: https://rsync.samba.org
