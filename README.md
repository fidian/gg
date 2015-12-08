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


Command Reference
-----------------

The `help` and `init` commands can be used anywhere.  All others can be executed from anywhere within an initialized folder, including inside any subdirectory.


### gg help

Shows a help message.  When used in an initialized directory, the help message will list many more commands.

Examples:

    gg help


### gg init REMOTE

Initialize the current directory.  `REMOTE` is the URL to a git repository that has a [manifest].  Downloads the [manifest] and [gg-core] into a `.gg/` folder.

Once a folder is initialized, additional commands show up in `gg help`.

Examples:

    gg init http://github.com/example/your-manifest.git


### gg status

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


### gg sync

Synchronize the [manifest], [gg-core] and every repository listed in the manifest.  First it executes a `git fetch` to pull in the remote's history.  After that, it attempts a rebase.  If the rebase fails, it falls back to a pull and push with possible merge conflicts detected and then they would need to be handled by hand.

Can be executed anywhere inside an initialized directory.

Examples:

    gg sync


[gg-core]: https://github.com/fidian/gg-core
[manifest]: https://github.com/fidian/gg-core/blob/master/doc/manifest.md
