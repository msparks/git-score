# git-score

*git-score* is a script that computes 'scores' for authors who have contributed
to a git repository. It displays aggregate stats in a concise format for human
consumption and provides a rough estimation of developer involvement in a
project.

![git-score screenshot](http://farm8.staticflickr.com/7180/6900456317_0d237aba81.jpg)

## Installation

Copy *git-score* to somewhere in your `PATH`. A good solution is to place a
git-score symlink in *~/bin* that points to your git-score repository.

## Usage

Inside a git repository, just run

    % git-score

It takes the following arguments directly:

    -e, --exclude=<pattern>
      Tallies scores per-file, and if <pattern> matches the filename.
      This is useful to exclude directories or particular file extensions.
      Warning: may run much slower. Still untested on very large repos.

    -i, --include=<pattern>
      Same as exclude, but the reverse (<pattern> must match).
      Include and exclude can be used in conjunction.

    -o, --order=[+-]<field>
      Sorts by chosen <field>. Prepend [+,-] for [ascending, descending]
      <field> must be one of: author, files, commits, delta, added, removed
      All fields (except author) default to descending.

    -h, --help
      Shows available options.

All other arguments not beginning with a hypen ('-') will be passed to `git
log`. This way, you can specify revision ranges to narrow scoring to a
particular set of commits.

    % git-score 61bf126e..HEAD

Note: The hyphen ('-') is optional: `git score` works too on recent versions of
git.

## Examples

Note: Emails obscured here.

    % git score
    author                          commits files  delta  (+)    (-)
    Matt Sparks <ms@------------->  13      2      143    184    41
    jbenet <jbenet@---------------> 8       2      76     110    34

    % git score b210b0..HEAD
    author                          commits files  delta  (+)    (-)
    jbenet <jbenet@---------------> 7       2      46     77     31
    Matt Sparks <ms@------------->  6       2      14     31     17

    % git score --include=.*\.[hm]$
    author                          commits files  delta  (+)    (-)
    jbenet <jbenet@---------------> 267     363    24664  47848  23184
    chris <------------------->     46      126    12908  17156  4248
    Rico <----------------------->  39      119    8659   10880  2221

    % git score --include=.*/src/.*$ --exclude=.*\.m$
    author                          commits files  delta  (+)    (-)
    jbenet <jbenet@---------------> 267     124    1884   3568   1684
    Rico <----------------------->  39      46     738    918    180
    chris <------------------->     46      44     696    936    240

    % git score --order=author
    author                          commits files  delta  (+)    (-)
    jbenet <jbenet@---------------> 12      2      136    183    47
    Matt Sparks <ms@------------->  13      2      143    184    41

    % git score --order=commits
    author                          commits files  delta  (+)    (-)
    Matt Sparks <ms@------------->  13      2      143    184    41
    jbenet <jbenet@---------------> 12      2      136    183    47

    % git score --order=-delta
    author                          commits files  delta  (+)    (-)
    jbenet <jbenet@---------------> 13      2      151    198    47
    Matt Sparks <ms@------------->  13      2      143    184    41

    % git score --order=+added
    author                          commits files  delta  (+)    (-)
    Matt Sparks <ms@------------->  13      2      143    184    41
    jbenet <jbenet@---------------> 13      2      151    198    47
