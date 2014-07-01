===============
Troubleshooting
===============

Having issues? Things not working as expected?

Here are a few 'Gotchyas' that we've encountered and what to check/how to fix them. 

Permissions
-----------

One of the first things to check is folder permissions. 
The installer tries to figure out what user is running the installer and set the permissions for the folders it creates appropriately. 
If this does not happen properly the user you run "app.sh" as may not have permission to access the necessary folders. 

Items to Check
^^^^^^^^^^^^^^

There are 2 main items that need their permissions checked:

Data directory

::

 /var/lib/mastercoin-tools

Tools directory

::

 ~/mastercoin-tools

Fix
^^^

These need to be owned by the user who is going to run "app.sh"::

 sudo chown -R <youruser>:<youruser> /var/lib/mastercoin-tools
 sudo chown -R <youruser>:<youruser> /home/<youruser>/mastercoin-tools


SX Settimgs
-----------

One of the other issues we've seen is when sx 'Hangs' or just fails to respond. 
Also visible if you are watching the system processes (command below) and notice it not moving/changing from the same command

::

 watch 'ps aux | grep -i -e sx -e sleep | grep -v grep'

Items to Check
^^^^^^^^^^^^^^

* The user running app.sh or calling sx commands needs to have a/the sx config file in the home directory of the user running "app.sh" 

::

 /home/<youruser>/.sx.cfg

* Also check to make sure the sx server is actually responding 

::

 #should return the block height number of the obelisk server
 sx fetch-last-height
 
 #should return the block height number of blockchain.info
 sx bci-fetch-last-height

Fix
^^^

* Make sure you are running "app.sh" as the user who has the sx config file in their home directory
* Try a different sx server. We have had decent experience using: *obelisk.bysh.me:9091*



.. _bitcoind:

Bitcoind
--------

Bitcoind is used by the Smart Property Scripts.

* :ref:`Asset Issuance (generateTX50_SP.py) <msc_tx50>` 
* :ref:`Crowdsale (generateTX51_SP.py) <msc_tx51>`
* :ref:`Close Crowdsale (generateTX53_SP.py) <msc_tx53>`

It can either be run locally or on a remote machine.
The scripts will first attempt to connect to a locally running instance of bitcoind, if that fails then they will look for and 
and try to connect to a remote instance. It will load connection information from ::

 home/<username>/.bitcoin/bitcoin.conf


bitcoin.conf is a 4 line file with the following values (these should be taken straigt from the configuration of your running bitcoind):

* rpcuser     - The username defined in your bitcoin.conf
* rpcpassword - The password defined in your bitcoin.conf
* rpcconnect  - The ip adress of the remote bitcoind machine
* rpcport     - (optional) The port its running on. (If not specified defaults to 8332)


