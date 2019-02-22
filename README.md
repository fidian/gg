GG - Git Groups
===============

Manage multiple git repositories quicker and easier.  Extremely similar to [repo](https://source.android.com/source/using-repo.html) that is used to manage the Android open source project.  The main difference is that this one is not specially geared to work with Gerrit, it's a lighter download, and it only requires Bash, which basically comes with any installation of git.

This is essentially a bootstrapper for [gg-core], which is automatically installed within an initialized directory.


Requirements
------------

* bash (version 3 or newer)
* git


Installation
------------

Simply download `gg` and put it in your path.

    # Step 1:  Download with curl ...
    curl https://raw.githubusercontent.com/fidian/gg/master/gg -o gg
    # or wget.
    wget https://raw.githubusercontent.com/fidian/gg/master/gg

    # Step 2:  Make the file executable.
    chmod 755 gg

    # Step 3:  Move gg somewhere to be in your path.
    # This is for a system-wide installation:
    sudo mv gg /usr/local/bin
    
    # A bit safer would be to put executables in ~/bin and make sure it
    # is in your path.
    mv gg ~/bin

There is also a [getting started] guide available.


Command Reference
-----------------

The `help` and `init` commands can be used anywhere.  All others can be executed from anywhere within an initialized folder, including inside any subdirectory.

It is important to note that there are environment variables and config files that alter the behavior of these commands. Read more about [configuration].


### `gg checkout [BRANCH]`

Checks out the given branch in all repositories. When `BRANCH` is not specified, the default branch from the [manifest] is used.

Examples:

    # Check out the default branch in each repository
    gg checkout

    # Check out the develop-testing branch in repositories that have that branch
    gg checkout develop-testing


### `gg help`

Shows a help message.  When used in an initialized directory, the help message will list many more commands.

Examples:

    gg help


### `gg init REMOTE [DEST]`

Initialize the current directory.  `REMOTE` is the URL to a git repository that has a [manifest].  Downloads the [manifest] and [gg-core] into a `.gg/` folder.

* REMOTE - This is the git repository URL.  It's the same as what you'd use for "git clone".
* DEST - Destination for the gg-initialized directory.

Once a folder is initialized, additional commands show up in `gg help`.

Examples:

    # Sets up a gg directory in ./your-manifest
    cd ~
    gg init http://github.com/example/your-manifest.git
    cd your-manifest
    gg sync

    # Specify a directory
    cd ~
    gg init http://github.com/example/another.git the-destination
    cd the-destination
    gg sync


### `gg pull`

Issues a `git pull` on the [manifest], [gg-core], and every repository listed in the [manifest].

Examples:

    gg pull


### `gg run <command> [args]`

Runs a command in all of the repositories. The command is executed at the root of each of the repositories.

Examples:

    # Shows the root of each directory
    gg run ls -l


### `gg status`

Shows the status of your repositories.  If a repository is clean, this only shows a short message.  If it is dirty it shows the entire `git status` message.

Examples:

    gg status

Result:

    *** .gg/gg-core: clean
    *** .gg/manifest: clean
    *** projects/starlight-mandible: dirty
    On branch gg
    Your branch is up-to-date with 'origin/gg'.
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   Makefile
        modified:   client.cpp
        modified:   app-engine.cpp

    no changes added to commit (use "git add" and/or "git commit -a")
    *** protected: clean
    *** tsunami/project: clean
    *** tsunami/configs: clean


### `gg sync`

Synchronize the [manifest], [gg-core], and every repository listed in the [manifest].  First it executes a `git fetch` to pull in the remote's history.  After that, it attempts a rebase.  When successful, your changes are pushed to the remote. If the rebase fails, it falls back to a standard pull and push, possibly creating merge conflicts. There's detection for the merge conflicts, but they must be resolved by hand.

Examples:

    gg sync


[configuration]: https://github.com/fidian/gg-core/blob/master/doc/config.md
[getting started]: https://github.com/fidian/gg-core/blob/master/doc/getting-started.md
[gg-core]: https://github.com/fidian/gg-core
[manifest]: https://github.com/fidian/gg-core/blob/master/doc/manifest.md
