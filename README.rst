=======
Git Gud
=======

A git tutorial (kinda) for absolute beginners

What is Git
===========

From https://git-scm.com:

    Git is a free and open source distributed version control system designed
    to handle everything from small to very large projects with speed and
    efficiency.

A version control system is a way of keeping track of the changes you do to
your code (or other files). It allows you to easily undo changes, mark and
compare different versions/snapshots of the code, and to collaborate with other
people on the same code base. Changes to the code (and the history of all
changes) are recorded in so called "code repositories". They can be imagined as
very detailed log books. Every change that to the code that is committed to the
repository is recorded in the log book.

Git is *not* GitHub. Git is *not* GitLab. GitHub and GitLab are websites that
offer to host Git repositories as a service. Using them has certain advantages,
but it is not at all required.

Some important nomenclature
===========================

Code:
    In this document "code" refers to all files you want to keep track of. This
    will mostly be text files (like actual source code), but can also include
    binary files (like images).

Repository:
    The place where the history of code changes is stored. Can be imagined as a
    detailed logbook.

Commit:
    A set of changes to the repository. Can be imagined as an entry in the
    logbook. Every commit (except for the very first) builds up on previous
    commits. It stores only the differences between the previous state/version
    of the code and the updated one. Since all past changes are kept track of,
    it is possible to recover the state of the code at any point in the
    recorded history, i.e. at any commit.

Workspace:
    The folder(s) where your code lives "outside" the repository, i.e. the
    files you see and work on in your directory structure.

Index:
    A temporary staging area where you mark changes in your workspace to be
    committed to the repository.

Branch:
    A branch is a reference to a particular state of the code, i.e. a
    particular commit. A Repository can have many branches and you can switch
    between them to work on/test different versions of your code. Think of them
    as bookmarks in the logbook.

HEAD:
    The HEAD is an alias for the branch you are currently working on (more or
    less). If you create a new commit, it will be based on the current HEAD.

How to get started
==================

Start a shell on your (Linux) machine and run ``git``::

    usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
               [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
               [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
               [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
               <command> [<args>]

    These are common Git commands used in various situations:

    start a working area (see also: git help tutorial)
       clone      Clone a repository into a new directory
       init       Create an empty Git repository or reinitialize an existing one

    work on the current change (see also: git help everyday)
       add        Add file contents to the index
       mv         Move or rename a file, a directory, or a symlink
       reset      Reset current HEAD to the specified state
       rm         Remove files from the working tree and from the index

    examine the history and state (see also: git help revisions)
       bisect     Use binary search to find the commit that introduced a bug
       grep       Print lines matching a pattern
       log        Show commit logs
       show       Show various types of objects
       status     Show the working tree status

    grow, mark and tweak your common history
       branch     List, create, or delete branches
       checkout    Switch branches or restore working tree files
       commit     Record changes to the repository
       diff       Show changes between commits, commit and working tree, etc
       merge      Join two or more development histories together
       rebase     Reapply commits on top of another base tip
       tag        Create, list, delete or verify a tag object signed with GPG

    collaborate (see also: git help workflows)
       fetch      Download objects and refs from another repository
       pull       Fetch from and integrate with another repository or a local branch
       push       Update remote refs along with associated objects

    'git help -a' and 'git help -g' list available subcommands and some
    concept guides. See 'git help <command>' or 'git help <concept>'
    to read about a specific subcommand or concept.

The exact output of this (and any other git command) might be different from
the examples in this document, depending on the exact version of Git you are
using.

If you get a "command not found" error, you need to install Git first. It is
part of the package manager of about every single Linux distribution.

Git is a very powerful tool, which unfortunately also means that it has many
intimidating looking options. The basic usage is fairly simple though, so don't
let it scare you. The ``git help`` command, the documentation, and your favourite
search engine are your friends, if you should ever not know what to do. Even
"experts" regularly look up how to do certain things, so don't feel bad about
doing so yourself.

If you have never used Git on this machine before, you will have to tell it who
you are. All commits also record the author of the changes. You can set this
information by executing::

    $ git config --global user.name "John Doe"
    $ git config --global user.email johndoe@example.com

Just replace the name and address with something appropriate. What exactly you
put here usually does not matter. The name and e-mail address are simply
written into the commit without doing anything else with them. If you use a
service like GitHub though, it might compare the e-mail address with its record
of users, so it can link to the appropriate user for all commits. This is
purely for convenience though, and everything usually also works authors that
are completely unknown to GitHub.

Creating a repository
=====================

If you have some code you want to start keeping track of, you need to first
create a repository. Go to the base directory of the code and run::

    $ git init

This will set up a repository in the hidden folder ``.git`` in the same
directory. All commits and supplementary information will be stored in that
folder. If you lose it, you also lose the repository. To use Git as a way of
creating backups of your code, you will need a separate remote repository (more
on that later).

Internally Git is using relative paths to refer to files within the workspace.
So from Git's point of view it is safe to move/rename the base directory of the
code, should you ever need/want to.

Seeing what Git sees
====================

To get a short summary of the current state of your working directory, you can
use the ``status`` command::

    $ git status
    On branch master

    No commits yet

    Untracked files:
      (use "git add <file>..." to include in what will be committed)

        some_file.txt

    nothing added to commit but untracked files present (use "git add" to track)

Among other things it will tell you what branch you are working on right now.
In this case, that is the default branch "master".

Adding files to the repository
==============================

On its own, Git will not magically start tracking the files in your workspace.
By default files in the working directory will be "untracked". You tell Git to
add them to the repository using the ``add`` command. Afterwards you can check
whether it did what you expected with ``status``::

    $ git add some_file.txt
    $ git status
    On branch master

    No commits yet

    Changes to be committed:
      (use "git rm --cached <file>..." to unstage)

        new file:   some_file.txt

This did not actually add the file to the repository yet, but it added it to
the index, i.e. the staging area. By separating the "adding" from the
"committing" step, Git allows you to sequentially add multiple files and then
commit them all in one single commit.

Committing the index to the repository
======================================

When you are happy with the changes you have added to the index, you can commit them
to repository using the ``commit`` command::

    $ git commit
    [master (root-commit) f45e476] Add some file.
     1 file changed, 1 insertion(+)
     create mode 100644 some_file.txt

This will open your default text editor to write a commit message. A commit
message should begin of a single line with a short description what the commit
does. Conventionally this line should be written in the imperative case, e.g.

    Add some file.

and not

    Adds some file.

The short summary should be followed by a more detailed description of what
changes happened in the commit. There should be a blank line separating the
short summary from the rest.

If you have a simple commit that does not require a detailed explanation, you
can use the ``-m`` option so specify a short commit message directly in the
command line::

    $ git commit -m 'Add some file.'

Viewing the commit history
==========================

You can use the ``log`` command to view the history of commits in your
repository::

    $ git log
    commit f45e476d0f9d35b571d51ea455f030ac00ca252a (HEAD -> master)
    Author: Lukas Koch <lukas.koch@mailbox.org>
    Date:   Mon Mar 2 16:10:28 2020 +0000

        Add some file.

        A longer description goes here.

By default this will show you a list of commits with the full commit message as
well as additional information like the author and the time of each commit.
This is not ideal if one just wants a quick overview of what the commit history
looks like. For this you can modify the output format of the ``log`` command
using a few of its many options::

    $ git log --oneline --graph --date-order --decorate
    * f45e476 (HEAD -> master) Add some file.

Because it is a bit of a pain to type such a long command, it might be useful to
define a bash alias for it::

    $ alias gitl='git log --oneline --graph --date-order --decorate'

After running this (or adding it to your ``bashrc``) you will be able to use the
``gitl`` shortcut::

    $ gitl
    * f45e476 (HEAD -> master) Add some file.

Another way of looking at the commit history is to use graphical interfaces
like ``gitk``. These have to be installed separately from the core Git program
though.

Viewing changes in the workspace
================================

Once a file is tracked by the repository, the ``status`` command will tell you
when it has been changed::

    $ git status
    On branch master
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   some_file.txt

    no changes added to commit (use "git add" and/or "git commit -a")

You can use the ``diff`` command to see the actual changes line-by-line::

    $ git diff
    diff --git a/some_file.txt b/some_file.txt
    index 7b57bd2..7b7ddac 100644
    --- a/some_file.txt
    +++ b/some_file.txt
    @@ -1 +1,3 @@
    -some text
    +Some text.
    +
    +Some more text.

Lines beginning with a ``-`` are present in the HEAD, but not in the workspace.
Lines beginning with a ``+`` are present in the workspace, but not in the HEAD.

Adding changes to the repository
================================

These changes are *not* automatically added to the repository. If you want to
record some changes, you need to explicitly add them to the index and then
commit the index to the repository::

    $ git add some_file.txt
    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

        modified:   some_file.txt

    $ git commit -m 'Change some file.'
    [master df598a5] Change some file.
     1 file changed, 3 insertions(+), 1 deletion(-)

Sometimes one does a couple of changes to a file, but then only want to commit
part of them. For example, if you find (and fix) a bug or typo while working on
a bigger change, you might want to commit just the bug fix and then keep
working on the bigger change before committing that one separately. This can be
achieved with the ``-p`` option of ``add``::

    $ git add -p
    diff --git a/some_file.txt b/some_file.txt
    index 7b7ddac..72c34d9 100644
    --- a/some_file.txt
    +++ b/some_file.txt
    @@ -1,3 +1,5 @@
    -Some text.
    +Some text!

     Some more text.
    +
    +Even more text.
    Stage this hunk [y,n,q,a,d,s,e,?]?

It will go through all changes it sees compared to the HEAD and ask you whether
you want to add it to the index or not. You also have the option to split the
shown changes (called "hunk") into even smaller pieces (``s``), or to edit it
freely (``e``). If you forget what all the different letters actually mean, you
can get an explanation by choosing ``?``.

After you added a part the changes in a file to the index, it will appear twice
in the ``status`` command, both as modified and ready to commit and as having
changes that are not staged yet::

    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

        modified:   some_file.txt

    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   some_file.txt

If you want to see the differences between the index and the HEAD before
committing, them you can use the ``--cached`` option of the ``diff`` command::

    $ git diff --cached
    diff --git a/some_file.txt b/some_file.txt
    index 7b7ddac..3288880 100644
    --- a/some_file.txt
    +++ b/some_file.txt
    @@ -1,3 +1,3 @@
    -Some text.
    +Some text!

     Some more text.


You can view the changes that have *not* been added to the index yet with the
``diff`` command without additional options::

    $ git diff
    diff --git a/some_file.txt b/some_file.txt
    index 3288880..72c34d9 100644
    --- a/some_file.txt
    +++ b/some_file.txt
    @@ -1,3 +1,5 @@
     Some text!

     Some more text.
    +
    +Even more text.

When you are happy with the changes added to the index, you commit them as
usual with ``commit``::

    $ git commit -m 'Exclaim!'
    [master c2a70dd] Exclaim!
     1 file changed, 1 insertion(+), 1 deletion(-)

In general you should strife to "commit early, commit often". Commits are
cheap, so whenever you have a tiny change of code that you know you probably
want to keep (for now), commit it. Even if it is experimental code! Accruing
lots of changes in the workspace that you never commit makes it quite hard to
find the changes that you do want to keep later on.

Creating a new branch
=====================

If you are working on a experimental feature and do not quite know whether you
will want to keep the changes you are doing to the code, it is a good idea to
create a separate branch for it. This will allow you to work on it and do lots
of commits without having to worry about how to undo these changes later if it
does not turn out as hoped.

You can create a new branch and immediately switch to it using the ``checkout``
command::

    $ git checkout -b 'experimental'
    Switched to a new branch 'experimental'

If you want to know which branch you are working on, use ``status``::

    $ git status
    On branch experimental
    nothing to commit, working tree clean

You can now use all previous commands as before, but without affecting the
state of the "master" branch.

Switching branches
==================

You can switch between branches using the ``checkout`` command::

    $ git checkout master
    Switched to branch 'master'

Listing available branches
==========================

You can list all branches in your repository with the ``branch`` command::

    $ git branch
      experimental
    * master

To get an overview of how the branches differ, i.e. what kind of commits are in
which branch, you can use the one-line ``log`` command as defined above with the
``--all`` option::

    $ gitl --all
    * d453b3e (experimental) Add something experimental.
    * 16c90da (HEAD -> master)  Add even more text.
    * c2a70dd Exclaim!
    * 5c0364d Change some file.
    * f45e476 Add some file.

Here you can see that we are currently on the branch "master", ``(HEAD ->
master)``, and that the branch "experimental" is ahead of "master" by a commit
"Add something special".

Undoing commits
===============

If it turns out that a change you committed was not a good idea after all, you
can undo it with the ``revert`` command together with a hash of the commit you
want to undo::

    $ git revert c2a70dd
    [master 000add3] Revert "Exclaim!"
     1 file changed, 1 insertion(+), 1 deletion(-)

This will create a new commit that undoes the changes of the specified one.

Merging branches
================

When you are done with the development on a branch and decide you want to add
these changes to the "master" branch, you can do so by merging them with
``merge``::

    $ git merge experimental
    Merge made by the 'recursive' strategy.
     experimental_textfile.txt | 1 +
     1 file changed, 1 insertion(+)
     create mode 100644 experimental_textfile.txt

After this (and possibly dealing with merge conflicts), the changes introduced
in the branch "experimental" will also be present in the current HEAD::

    $ gitl
    *   60ff7f6 (HEAD -> master) Merge branch 'experimental'
    |\
    * | 000add3 Revert "Exclaim!"
    | * d453b3e (experimental) Add something experimental.
    |/
    * 16c90da  Add even more text.
    * c2a70dd Exclaim!
    * 5c0364d Change some file.
    * f45e476 Add some file.

Deleting branches
=================

Since the changes in "experimental" have been merged into the "master" branch,
the branch "experimental" can be safely deleted::

    $ git branch --delete experimental
    Deleted branch experimental (was d453b3e).

This does *not* delete the commits that were part of "experimental", as those
are also a part of "master" now. This only deletes the "bookmark" labeled
"experimental"::

    $ gitl
    *   60ff7f6 (HEAD -> master) Merge branch 'experimental'
    |\
    * | 000add3 Revert "Exclaim!"
    | * d453b3e Add something experimental.
    |/
    * 16c90da  Add even more text.
    * c2a70dd Exclaim!
    * 5c0364d Change some file.
    * f45e476 Add some file.

