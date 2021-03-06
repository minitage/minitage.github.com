minitagetool : Usage
###################################

Installing a project
=====================
Now what you cant is to install your project.

Minitage needs a minilay with one or more minibuilds declaring your project, and the project itself.

The minilay can be either contained on the project repository or be on a separate repo, do as you want.


The only thing you have to do is to install the project minilay in ``minitage/minilays/``

    - Either by checkouting your project in its minitage final location (``<minitage>/categ/minibuild``), and then symlick the minilay::

        mkdir minitage/zope
        cd minitage
        git clone git@foo:bar zope/bar
        ln -s $PWD/zope/bar/minilay minilays

    - Or by directly checkouting the minilay::

        cd minitage
        git clone git@foo:bar-minilay minitage/minilays/bar-minilay

- Run the minimerge dance via the tool **+** the **ONLINE** switch::

    ./minitagetool.sh deploy <projectminibuild> [<otherminibuild>]


Upgrade minitage itself
========================
Time to time, you need to update your software stack...
Or you have issues with minitage and want to see if is is now fixed,

Online (internet) mode upgrade
-------------------------------
::

    ./minitagetool.sh selfupgrade

Offline (no internet) mode upgrade
-------------------------------------
After having upgrade local source code (eg from snapshot tarballs)::

    ./minitagetool.sh offlineupgrade

Reinstalling projects while upgrading
-----------------------------------------

Think to redeploy your project dependencies updates,
You can do this command to do the whole at the same time (update minitage+projects)::

    ./minitagetool.sh selfupgrade|offlineupgrade <projectminibuild> [<otherminibuild>]

Upgrade projects
==================
This will pull the new code from origin and then do a regular deploy
Simply run::

    ./minitagetool.sh upgrade <projectminibuild> [<otherminibuild>]

Make a snapshot
========================
You can reproduce a minitage installation by taking a snapshot. This snapshot will be usable offline::

    ./minitagetool.sh snapshot [<project> <projectn>]

Without argument it snapshots **all projects**.
It excludes the **var** directory among others.
Giving argument will filter the "projects" tarball including only the specified projects.

Deploy a snapshot
========================
::

    mkdir minitage
    cd minitage

get the archives produced by ``./minitagetool.sh snapshot``::

     tar xzvf <minitageoffline-CHRONO-base.tar.gz>
     tar xzvf <minitageoffline-CHRONO-downloads.tar.gz>
     tar xzvf <minitageoffline-CHRONO-projects.tar.gz>

Restore the snapshot to an usable state::

     ./minitagetool.sh offlinedeploy <project>

(Re)install a project in offline mode
=======================================
Just as earlier, use the offline switch::

     ./minitagetool.sh offlinedeploy <project>

Launch cgwb
=============
Cgwb is the minitage project generator web frontend::

    ./minitagetool.sh cgwb

Downloads Errors
==================
If you see such a traceback, just relaunch the issued command to see if it was a temporary download problem
::

    minitage.recipe: Cache download http://prdownloads.sourceforge.net/libpng/libpng-1.5.9.tar.gz?download as /home/user/.buildout/downloads/minitage
    minitage.recipe: Downloading http://prdownloads.sourceforge.net/libpng/libpng-1.5.9.tar.gz?download in /home/user/.buildout/downloads/minitage/libpng-1.5.9.tar.gz
    While:
      Installing part.

    An internal error occured due to a bug in either zc.buildout or in a
    recipe being used:
    Traceback (most recent call last):
      File "/usr/home/user/minitage/eggs/cache/zc.buildout-2.1.0-py2.7.egg/zc/buildout/buildout.py", line 1923, in main
        getattr(buildout, command)(args)
      File "/usr/home/user/minitage/eggs/cache/zc.buildout-2.1.0-py2.7.egg/zc/buildout/buildout.py", line 604, in install
        installed_files = self[part]._call(recipe.install)
      File "/usr/home/user/minitage/sources/buildout.minitagificator/src/buildout/minitagificator/minitagificator.py", line 243, in _call
        ret = Options._old_call(self, f)
      File "/usr/home/user/minitage/eggs/cache/zc.buildout-2.1.0-py2.7.egg/zc/buildout/buildout.py", line 1358, in _call
        return f()
      File "/usr/home/user/minitage/sources/minitage.recipe.cmmi/src/minitage/recipe/cmmi/cmmi.py", line 216, in install
        fname = self._download(md5=self.md5, cache=True)
      File "/usr/home/user/minitage/sources/minitage.recipe.common/src/minitage/recipe/common/common.py", line 943, in _download
        use_cache=use_cache
      File "/usr/home/user/minitage/sources/minitage.core/src/minitage/core/common.py", line 364, in get_from_cache
        raise MinimergeError(msg)
    MinimergeError: Failed download for http://prdownloads.sourceforge.net/libpng/libpng-1.5.9.tar.gz?download:     need more than 1 value to unpack
    Backup of the downloaded file has been made in /home/user/.buildout/downloads/minitage/libpng-1.5.9.tar.gz.md5sum_mismatch.0


