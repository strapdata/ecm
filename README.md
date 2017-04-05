ECM (Elassandra Cluster Manager)
====================================================

A script/library to create, launch and remove an Elassandra cluster on
localhost.

Requirements
------------

- python 2.7
- pip (to install python dependencies)

Installation
------------

To install it globally on system, run:

    sudo pip install
    sudo ./setup.py install
    
Or just try it locally with:

    virutalenv venv
    . venv/bin/activate
    pip install
    
Usage
-----

Let's say you wanted to fire up a 3 node Elassandra cluster.

    ecm create test-cluster -n 3 -s -e --install-dir=<path-to-elassandra-installation>

which is equivalent to:

    ecm create test-cluster --install-dir=<path-to-elassandra-installation>
    ecm populate -n 3
    ecm start -e

The option `-e` is used to enable ElasticSearch.

To start a cluster with the elassandra code pulled from the `elassandra-rc` repository:

    ecm create test-cluster -n 3 -s -e -v git:master
    
Known issues
------------

- ccm only works on localhost for now. If you want to create multiple
  node clusters, the simplest way is to use multiple loopback aliases. On
  modern linux distributions you probably don't need to do anything, but
  on Mac OS X, you will need to create the aliases with

      sudo ifconfig lo0 alias 127.0.0.2 up
      sudo ifconfig lo0 alias 127.0.0.3 up
      ...

  Note that the usage section assumes that at least 127.0.0.1, 127.0.0.2 and
  127.0.0.3 are available.

Windows only:
  - `node start` pops up a window, stealing focus.
  - cli and cqlsh started from ccm show incorrect prompts on command-prompt
  - non nodetool-based command-line options fail (sstablesplit, scrub, etc)
  - cli_session does not accept commands.
  - To install psutil, you must use the .msi from pypi. pip install psutil will not work
  - You will need ant.bat in your PATH in order to build C* from source
  - You must run with an Unrestricted Powershell Execution-Policy if using Cassandra 2.1.0+
  - Ant installed via [chocolatey](https://chocolatey.org/) will not be found by ccm, so you must create a symbolic
    link in order to fix the issue (as administrator):
    - cmd /c mklink C:\ProgramData\chocolatey\bin\ant.bat C:\ProgramData\chocolatey\bin\ant.exe
