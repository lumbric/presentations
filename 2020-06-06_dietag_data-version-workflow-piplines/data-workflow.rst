..
    This is a copy of a file originally located here:
    https://gitlab.com/caichinger/diedag-dietag_20200606/-/blob/master/data-workflow/data-workflow.rst (no public access)

    This file contains presentation slides. One can use this VIM plugin to display the presentation:
    https://github.com/sotte/presenting.vim

~~~~

Workflow, pipelines and version controlling big datasets
========================================================

- typical software engineering workflow
- typical data science workflow
- differences and problems to be solved
- wish list and utopia
- 10 available tools for versioning data

~~~~

A software development workflow
-------------------------------

- idea for a feature or project
- sketch (meeting, ticket, specification, ...)
- in rare cases: prototype
- implementation / write tests
- building and running tests can be done on developers' machines
- continuous integration (CI) runs tests again
- review by other developers
- merge feature branch into main branch
- CI runs tests + runs build system
- binaries / packages / docker images are pushed to package server
- deployment in life system (often automatic too)
- bug / feature / enhancement -> ticket -> back to step (2)



~~~~

Version control
---------------

Why?

- manage versions
- collaboration
- find bugs (git bisect, ftw!)

And have lots of fun with commands like:
    `git filter-branch --tree-filter "rm -rf oh-no.rst --prune-empty HEAD`


~~~~

Code review
-----------

- offers platform to discuss code in written form
- filter bad code before it enters mainline
- test readability before it's too late
- force developers to communicate
- spread knowledge among the team
- enforce coding styles
- help new team members
- ...

Typical workflow:
- author owns the feature branch (nobody else touches it)
- other developers or maintainers:
    - read and test the code
    - they provide positive and negative feedback
    - ask for changes
    - approve the code if it works fine
- feature branch gets merged to master


~~~~

Build System
------------

- store commands and parameters to compile code
- hierarchy of input/out
- know what has changed
- do only necessary steps
- optional: use build cache

Tools
- Make (+ derivations)
- SCons (in Python, used for everything)
- Grunt (mostly for Javascript)

https://en.wikipedia.org/wiki/Make_(software)

~~~~

Continuous Integration
----------------------

Helps to automate tests and build processes on a well maintained machine.

If tests break on CI, but succeed on your machine, it's your fault!

Often there is a hierarchy of dependent jobs similar to the targets in build system.

Examples:

- Github Actions (hosted)
- travis-ci.org (hosted)
- stickler-ci.com (hosted, linting only)
- Gitlab CI (hosted or selfhosted)
- Jenkins (Opensource, selfhosted)
- ...

Warning: travis-ci.org != travis-ci.com

https://github.com/lumbric/lunchbot/commits/master

~~~~

Negative example of coding in science
-------------------------------------

https://philbull.wordpress.com/2020/05/10/why-you-can-ignore-reviews-of-scientific-code-by-commercial-software-developers/amp/



~~~~

Data workflow
-------------

- get the data (tons of data)
- exploration (anything useful in this undocumented CSV file?)
- some vague, probably no real sketch
- try something
- does it work?
    - yes: great! - nice, continue with step (6)
    - no: ?!?!?!  - start over with (1)?
- deployment...?
- publish results?


~~~~

My current workflow... 😬😬😬
-----------------------------

https://github.com/inwe-boku/wind-repowering-usa

- Which code version was used to generate this file?
- How can I sync the data and interim results between server and laptop?
- Did I execute all necessary steps?

~~~~


The problems and the differences
--------------------------------

**Problem 1:** where to store the input data?

...let's just commit them to the Github repository!

- data size: neither Git nor Github can handle 100GB files
- old versions can't be deleted in Git:
    - deleting of blob objects leads to weird warnings
    - no, just don't do it!
    - anyway a lot tooling is missing here to do it comfortably
- license issues: we are not allowed to publish input data, but need to publish code

~~~~

**Problem 2:** Where to store the results?

Similar problems to above, especially for interim data.

Also, it doesn't feel quite right to add reproducible binary data to a git repository - it
definitely can't work if the CI runs these processes.

Could work with a lot of discipline, not sure... ¯\_(ツ)_/¯

~~~~

**Problem 3:** How to run the pipeline?

¯\_(ツ)_/¯

Having a CI is nice, but where to find hardware? Processing may take 10h...?

Which code parts depend on which outputs?

~~~~

Goals
-----

- reproducibility: store input data
- traceability: which code produced outputs?
- run parts by re-using interim results
- allow syncing of data between machines
- allow deletion of large data files


Ideal imagination: programming functions in python which can be plugged together, but also run
individually. They should be stateless and therefore outputs can be stored (cached) as interim
results and should be fast if neither code nor input data have changed since the last run.

~~~~

Tools for data versioning and pipelining
----------------------------------------

+-----------+-----------------------------+-------------------------------------------+-------+------+-------+-----------------------------------------------------------+
| Name      | Website                     | Github                                    | Stars | Fork | Watch | comment                                                   |
+-----------+-----------------------------+-------------------------------------------+-------+------+-------+-----------------------------------------------------------+
| Git LFS   | https://git-lfs.github.com  | https://github.com/git-lfs/git-lfs        |  8.2k | 1.6k |   423 | uses git hooks, symlinks, Github supports storage backend |
+-----------+-----------------------------+-------------------------------------------+-------+------+-------+-----------------------------------------------------------+
| DVC       | https://dvc.org             | https://github.com/iterative/dvc          |  5.2k |  465 |    98 | metadata in files, uses hard links for caches             |
+-----------+-----------------------------+-------------------------------------------+-------+------+-------+-----------------------------------------------------------+
| Pachyderm | https://www.pachyderm.com   | https://github.com/pachyderm/pachyderm    |  4.4k |  418 |   160 | docker only                                               |
+-----------+-----------------------------+-------------------------------------------+-------+------+-------+-----------------------------------------------------------+
| qri       | https://qri.io              | https://github.com/qri-io/qri             |   918 |   53 |    18 | GUI for Windows and Mac                                   |
+-----------+-----------------------------+-------------------------------------------+-------+------+-------+-----------------------------------------------------------+
| quilt     | https://quiltdata.com       | https://github.com/quiltdata/quilt        |   910 |   57 |    24 | Python interface to control the thing                     |
+-----------+-----------------------------+-------------------------------------------+-------+------+-------+-----------------------------------------------------------+
| lazydata  |                             | https://github.com/rstojnic/lazydata      |   613 |   23 |    20 | Python library, supports only S3 as remote                |
+-----------+-----------------------------+-------------------------------------------+-------+------+-------+-----------------------------------------------------------+
| datmo     |                             | https://github.com/datmo/datmo            |   325 |   26 |    10 | focused on machine learning, alpha status                 |
+-----------+-----------------------------+-------------------------------------------+-------+------+-------+-----------------------------------------------------------+
| Datalad   | https://www.datalad.org     | https://github.com/datalad/datalad        |   195 |   58 |    19 | focused more on science than on ML                        |
+-----------+-----------------------------+-------------------------------------------+-------+------+-------+-----------------------------------------------------------+
| dataiku   | https://www.dataiku.com     |                                           |       |      |       | not open source, reduced free version available           |
+-----------+-----------------------------+-------------------------------------------+-------+------+-------+-----------------------------------------------------------+
| osfr      |                             | https://github.com/ropensci/osfr          |       |      |       | uses R                                                    |
+-----------+-----------------------------+-------------------------------------------+-------+------+-------+-----------------------------------------------------------+



~~~~

Git LSF
-------

- Stars 8.2k
- Fork 1.6k
- Watch 423
- https://git-lfs.github.com
- https://github.com/git-lfs/git-lfs
- uses git hooks to do a lot of magic automatically
- very simple to use, just do a `git lfs track largefile.nc`
- supports wild cards like `git lfs track *.nc`
- allows to delete old versions using `git lfs prune`
- no support for pipelines or anything alike
- requires special server software on server side
- Github supports storage backend, free storage up to 2GB of data [0]
- Git LFS on S3 is possible: https://github.com/meltingice/git-lfs-s3


[0] https://help.github.com/en/github/managing-large-files/about-git-large-file-storage


~~~~

dvc
---

- Stars 5.2k
- Watch 98
- Fork 465

- https://dvc.org/
- does not install via apt, deb on webpage
- collects anonymous usage statistics per default
- remotes: SSH, HTTP, local, S3, Azure, Google...
- how it works:
    - adds a .dvc folder which is mostly in .gitignore
    - adds `*.dvc` text files with metadata to git (md5 hash, path, ...)
    - uses hard links to cache files
    - allows to remove files from cache

- note: local remote is not using hard links

- supports metrics and pipelines

- open questions:
    - how about copy on write when pushing to local remote?
    - what if there is disk space only for one version of a large file?
    - how to include external files on the same disk?

~~~~

Demo:

.. code::bash

    mkdir test-dvc
    cd test-dvc

    git init

    dvc init
    cat .dvc/.gitignore

    git commit -m 'Init dvc repository'

    echo "some content" > small-file1
    git add small-file1
    git commit -m 'Normal git commit'

    dd if=/dev/zero of=largefile-1GB count=1000 bs=1M
    dvc add largefile-1GB
    cat .gitignore
    cat largefile-1GB.dvc
    du -hs .
    du -hs .dvc
    find . -samefile largefile-1GB


~~~~


Pachyderm
---------

- Stars 4.4k
- Fork 418
- Watch 160
- https://www.pachyderm.com/
- https://github.com/pachyderm/pachyderm
- partially opensource, partially paid
- works (only) with Docker and Kubernetes
- "Pachyderm seems more enterprise focused"

~~~~

qri
---

- https://qri.io/
- https://github.com/qri-io/qri
- Stars 918
- Fork 53
- Watch 18
- GPL

- GUI for Mac, Windows
- can publish and version data
- suggests nice metadata format


~~~~

quilt
-----

- https://quiltdata.com/
- https://github.com/quiltdata/quilt
- Stars 910
- Fork 57
- Watch 24
- Apache 2-0

Seems to have work similar to a command line tool but being controlled via
Python commands. A bit weird maybe?

https://open.quiltdata.com/b/quilt-example/packages/examples/hurdat/tree/latest/


~~~~

lazydata
--------

    - https://github.com/rstojnic/lazydata
    - Stars 613
    - Forks 23
    - Watch 20
    - Apache 2.0
    - Python
    - supports only Amazon S3 as remote

Differences to dvc:
https://github.com/rstojnic/lazydata/issues/2

~~~~


datmo
-----

- https://github.com/datmo/datmo
- Stars 325
- Fork 26
- Watch 10

~~~~


Datalad
-------
- https://www.datalad.org/
- https://github.com/datalad/datalad
- Stars 195
- Fork 58
- Watch 19

~~~~


dataiku
-------

- https://www.dataiku.com/
- machine learning oriented
- not opensource, reduced free version available

~~~~


osfr
----

- Stars 108
- Fork 25
- Watch 18
- https://github.com/ropensci/osfr
- uses R

~~~~


Todo
----

- datproject
    - https://datproject.org/

- piplining:
    - luigi
    - airflow
    - snakemake
    - pydoit


~~~~

Links
-----

Kaggle workshop on data Versioning:
https://www.kaggle.com/rtatman/kerneld4769833fe

Overview over a couple of tools like DVC, Pachyderm etc:
https://www.dolthub.com/blog/2020-03-06-so-you-want-git-for-data/

A lengthy discussion of data versioning tools:
https://carpentries.topicbox.com/groups/discuss/Tb776978a905c0bf8-Mc391b14e70952e72cff01775/version-control-and-collaboration-with-large-datasets

DVC explained:
https://www.youtube.com/watch?v=BneW7jgB298

Covers reproducible machine learning and has really good links:
https://towardsdatascience.com/why-git-and-git-lfs-is-not-enough-to-solve-the-machine-learning-reproducibility-crisis-f733b49e96e8

More:
https://papers.nips.cc/paper/5656-hidden-technical-debt-in-machine-learning-systems.pdf
https://christophergs.com/machine%20learning/2019/05/13/first-impressions-of-dvc/
https://carpentries.topicbox.com/groups/discuss/Tb776978a905c0bf8-Mc391b14e70952e72cff01775/version-control-and-collaboration-with-large-datasets
https://www.youtube.com/watch?v=7jKTofl2vmM&feature=youtu.be
