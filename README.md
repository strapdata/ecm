ECM (Elassandra Cluster Manager)
====================================================

A script/library to create, launch and remove an Elassandra cluster on
localhost.

It's a fork of [ccm](https://github.com/pcmanus/ccm) modified to run Elassandra.

Requirements
------------

- python 2.7
- pip
- jdk 1.8
- eventually maven, cassandra-driver, cql...

Installation from git repository
------------
    
    $ git clone git@github.com:strapdata/ecm.git
    $ cd ecm
    
To install it globally on system, run:

    $ sudo pip install -r requirements.txt
    $ sudo ./setup.py install
    
Or just try it locally with:

    $ virutalenv venv
    $ . venv/bin/activate
    $ pip install -r requirements.txt
    
Usage
-----

### General
Let's say you wanted to fire up a 3 node Elassandra cluster, using a local installation directory:

    $ ecm create cluster-name --install-dir=<path-to-installation> -n 3 -s -e 

which is equivalent to:

    $ ecm create cluster-name --install-dir=<path-to-installation>
    $ ecm populate -n 3
    $ ecm start -e

The option `-e` is used to enable ElasticSearch.


### Github
To start a cluster with the elassandra code pulled from the `elassandra-rc` repository:

    $ ecm create cluster-name -v git:master -n 3 -s -e

### Release
To start a cluster with a tarball release downloaded from github:
    
    $ ecm create cluster-name -v 2.4.2-10 -n 3 -s -e

### Others commands
```bash
    # see the cluster status
    $ ecm status
    
    # use nodetool on node1 (127.0.0.1)
    $ ecm node1 nodetool status
    
    # start a cqlsh session connected to node1, be sure to have cql installed
    $ ecm node1 cslsh
    
    # stop then restart node2 (127.0.0.2)
    $ ecm node2 stop
    $ ecm node2 start -e
    
    # stop the cluster:
    $ ecm stop
    
    # remove the cluster
    $ ecm remove
    
    # switch to an older cluster you created
    $ ecm switch other-cluster-name
```


Known issues
------------
- Not every options from ccm work with ecm. Only a limited subset is used.
  Support will be extended according to our needs.

- Ecm only works on localhost for now. If you want to create multiple
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
