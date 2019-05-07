# Simple Command-line Utility for Retrieving the Status of an Apache Spark Master

[Apache Spark](https://spark.apache.org/) is an exceedingly fast, well-engineered, and developer friendly environment for running distributed applications over large datasets.

However, Spark does not come with any command line tools for retrieving basic information from the Spark Master, e.g. how many applications are Running or Waiting; how many nodes are alive, etc. The `spark-submit` tool does allow you to check the status of individual applications, but not the cluster overall. As it stands, the only way to check the status of the cluster is to visit the web interface for the Master.

This simple python3 script fills this gap.

## Usage

The basic usage of the script is available via the `--help` command line argument.

     $ spark-status --help
     usage: spark-status [-h] [-c [CONF]] [-u [URL]] [-a] [-f] [-w]

     Get the status of a Spark cluster, such as living nodes, CPU usage, and
     current and historical applications.

     optional arguments:
       -h, --help            show this help message and exit
       -c [CONF], --conf [CONF]
       	                     The full path to the Spark configuration file. The
                             hostname for the Spark Master will be read from this
                             file, unless the URL is provided on the command line.
       -u [URL], --url [URL]
                             The full URL for the Spark Master. If not specified,
                             the URL will be determined from the Spark
                             configuration file.
       -a, --all             Display all application attributes, including ID.
       -f, --finished        Display finished applications.
       -w, --workers         Display information about the workers.

For example, you might execute the following to see the overall status of the master and all applications, including those that have finished.

    $ spark-status --finished
    # Cluster Status
    Status         :   ALIVE
    Alive Workers  :      20
    Cores          :     704
    Cores Used     :     704
    Memory         : 3.67 TB
    Memory Used    : 2.34 TB
    Active Apps    :       1
    Completed Apps :     194

    # Active Applications
    RPKI: Akamai dataset verification         Tue May 07 12:16:30 EDT 2019  tjchung     3.7 h

    # Completed Applications
    blockadvertisments                        Tue May 07 15:48:31 EDT 2019  asadasad    24.5 s
    PySparkShell                              Tue May 07 15:46:11 EDT 2019  asadasad    36.1 s
    PySparkShell                              Tue May 07 15:32:53 EDT 2019  lulu        6.2 m
    ethernodes_analysis                       Tue May 07 15:32:34 EDT 2019  lulu        10.5 s
    blockadvertisments                        Tue May 07 15:02:34 EDT 2019  lulu        32.9 s
    blockadvertisments                        Tue May 07 15:02:09 EDT 2019  lulu        2.5 s
    hashes per block                          Tue May 07 14:41:54 EDT 2019  lulu        3.9 m
    RPKI analysis                             Tue May 07 12:15:57 EDT 2019  tjchung     12.0 s
    UserTweetCount                            Tue May 07 10:44:27 EDT 2019  mwehlast    1.2 h
    RPKI analysis                             Tue May 07 10:10:27 EDT 2019  tjchung     12.8 m
    RPKI analysis                             Tue May 07 08:30:12 EDT 2019  tjchung     1.5 h
    UserTweetCount                            Tue May 07 08:27:20 EDT 2019  mwehlast    1.5 h
    UserTweetCount                            Tue May 07 08:25:59 EDT 2019  mwehlast    39.2 s

## Dependencies and Requirements

This script is written in Python 3 and requires [urllib3](https://pypi.org/project/urllib3/) and [requests](https://pypi.org/project/requests/).

This script retrieves information from the Spark Master by visiting and scraping the JSON endpoint pf the web interface. The script attempts to determine the URL of the Spark Master from the spark-defaults.conf file. By default, this script wants to live in the `spark-2.*.*/bin/` folder. If you run the script without specifying the `--url` for the Spark Master, or the `--conf` full path to the Spark configuration file (spark-defaults.conf), then the script will assume that the relative path `../conf/spark-defaults.conf` is correct.

This script is designed to work with Spark clusters that are being run in standalone mode, using the built-in Spark Master. **This script is not designed to work with Yarn or Mesos clusters**.

## Compatibility

This script has been tested against Spark 2.3.\* and 2.4.\*. I suspect that it will also work for Spark 2.2.\*, and possibly older branches of the 2.\*.\* line. 