DataTorrent RTS Installation Guide
================================================================================

This guide covers installation of the DataTorrent RTS platform.


Requirements
--------------------------------------------------------------------------------

* Linux operating system (tested on CentOS 6.x and Ubuntu 12.04)
* Java 7 or 8 (tested with Oracle Java 7, OpenJDK Java 7)
* Hadoop (2.2.0 or above) cluster with YARN, HDFS configured, and hadoop executable available in PATH
* Minimum of 8G RAM available on the Hadoop cluster
* Google Chrome, Firefox, or Safari to access the DataTorrent Console UI
* Permissions to create HDFS directory for DataTorrent user


Installation
--------------------------------------------------------------------------------

Complete installation configures DataTorrent to run as a system service, installs globally available binaries, and requires root/sudo privileges to run.  After the installation is complete, you will have the following

* DataTorrent platform release ( $DATATORRENT_HOME ) located in /opt/datatorrent/releases/[release_version]
* Binaries are available in /opt/datatorrent/current/bin and links in /usr/bin
* Configuration files located in /opt/datatorrent/current/conf
* Log files located in /var/log/datatorrent
* DataTorrent Gateway service dtgateway running as dtadmin, and managed with "sudo service dtgateway" command

Download and run the installer binary on one of the Hadoop cluster nodes.  This can be done on ResourceManager, NameNode, or any node which has correct Hadoop configuration and is able to communicate with the rest of the cluster.

a.  Installing from self-extracting archive (*.bin)

        curl -LSO https://www.datatorrent.com/downloads/datatorrent-rts.bin
        sudo sh ./datatorrent-rts.bin

b.  Installing from RedHat Package Manager archive (\*.rpm)

      curl -LSO https://www.datatorrent.com/downloads/datatorrent-rts.rpm
      sudo rpm -ivh ./datatorrent-rts.rpm


Limited Local Installation
--------------------------------------------------------------------------------

A limited local installation can be helpful for in environments with no root/sudo privileges, or for testing purposes.  All DataTorrent files will be installed under the user's home directory, and DataTorrent Gateway will be running as a local process.  No services or files outside user's home directory will be created.

* DataTorrent platform release ( $DATATORRENT_HOME ) located under $HOME/datatorrent/releases/[release_version]
* Binaries are available under $HOME/datatorrent/current/bin
* Configuration files located under $HOME/datatorrent/conf
* Log files located under $HOME/.dt/logs
* DataTorrent Gateway running as current user, and managed with dtgateway command


1.  Download and run the installer binary on one of the Hadoop cluster nodes.  This can be done on ResourceManager, NameNode, or any node which has correct Hadoop configuration and is able to communicate with the rest of the cluster.

        curl -LSO https://www.datatorrent.com/downloads/datatorrent-rts.bin
        sh ./datatorrent-rts.bin

2.  Add DataTorrent binaries to user PATH environment variable by pasting following into ~/.bashrc, ~/.profile or equivalent environment settings file.

        DATATORRENT_HOME=~/datatorrent/current
        export PATH=$PATH:$DATATORRENT_HOME/bin


Upgrades
--------------------------------------------------------------------------------

DataTorrent RTS installer automatically performs an upgrade when the same base installation directory is selected.  Configurations stored in local files such as dt-site.xml and custom-env.sh, as well as contents of DataTorrent DFS root directory will be be used to configure the newer version.

Automatic upgrade behavior can be avoided by providing installer with an options to perform full uninstall prior to running the installation.  See Customizing Installation section below.  

Full uninstall prior to installation is strongly recommended when performing major version upgrades.  Not all settings and attributes will be backwards compatible between major releases.  This may result in applications failing to launch when an invalid attribute is loaded from dt-site.xml.  In such cases detailed error messages will be provided during an application launch, helping identify and fix outdated references before successfully launching applications.



Customizing Installation
--------------------------------------------------------------------------------

Various options are available to customize the DataTorrent installation.  List of available options be displayed by running installer with -h flag.

    ./datatorrent-rts*.bin -h
    
    Options:

    -h             Help menu
    -B <path>      Use <path> as base installation directory.  Must be an absolute path.  Default: /opt/datatorrent
    -U <user>      Use <user> user account for installation.  Default: dtadmin
    -G <group>     Use <group> group for installation.  Default: dtadmin ( based on value of <user> )
    -H <path>      Use <path> for location for hadoop executable.  Overrides defaults of HADOOP_PREFIX and PATH.
    -E <expr>      Adds export <expr> to custom-env.sh file.  Used to set an environment variable.  Examples include:
                     -E JAVA_HOME=/my/java                 Java used by dtgateway  and dtcli
                     -E DT_LOG_DIR=/var/log/datatorrent    Directory for dtgateway logs
                     -E DT_RUN_DIR=/var/run/datatorrent    Directory for dtgateway pid files
    -s <file>      Use <file> DataTorrent dt-site.xml file to configure new installation. Overrides default and previous dt-site.xml
    -e <file>      Use <file> DataTorrent custom-env.sh file to configure new installation. Overrides default and previous custom-env.sh
    -g [ip:]port   DataTorrent Gateway address.  Defaults to 0.0.0.0:9090
    -u             Perform full uninstall prior to running installation. WARNING: All current settings will be removed!
    -x             Skip installation steps.  Useful for debugging installation settings with [-v] or perform uninstall with [-u]
    -r             Uninstall current release only.  Does not remove datatorrent, logs, var, etc directories.  Used with RPM uninstall.
    -v             Run in verbose mode, providing extra details for every step.
    -V             Print DataTorrent version and exit.


Some Hadoop distributions may rquire changes to default settings.  For example, when running Apache Hadoop, you may encounter that JAVA_HOME is not set for DTGateway user (defaults to dtadmin):

    sudo -u dtadmin hadoop version
    Error: JAVA_HOME is not set and could not be found.

If JAVA_HOME is not set, you can export it manually at the end of /opt/datatorrent/current/conf/custom-env.sh or run the installer with following option to set JAVA_HOME during installation time:

    sudo ./datatorrent-rts*.bin -E JAVA_HOME=/valid/java/home/path

In some Hadoop installations, you may already have an existing user, which has administrative privileges to HFDS and YARN, and you prefer to use that user to run DTGateway service.  Assuming this user is named myuser, you can run the following command during installation to ensure DTGateway service runs as myuser.

    sudo ./datatorrent-rts*.bin -U myuser


Installation Wizard
--------------------------------------------------------------------------------

After the system installation is complete, remaining configuration steps are performed using the Installation Wizard in the DataTorrent UI Console.  DataTorrent UI Console address is typically hostname of the node where DataTorrent platform was installed, listening on port 9090.  The connection details are provided by the system installer, but may need to modified depending on the way Hadoop cluster is being accessed.

    http://<installation_host>:9090/

The Installation Wizard can be accessed and repeated at any time to update key system settings from the Configuration section of the DataTorrent UI Console.

DataTorrent installation can be verified by running included demo applications.  See [Launching Demo Applications](demos.md) for details.
