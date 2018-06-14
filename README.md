# Simple Command-line Utility for Retrieving the Status of an Apache Spark Master

[Apache Spark](https://spark.apache.org/) is an exceedingly fast, well-engineered, and developer friendly environment for running distributed applications over large datasets.

However, Spark does not come with any command line tools for retrieving basic information from the Spark Master, e.g. how many applications are Running or Waiting; how many nodes are alive, etc. The `spark-submit` tool does allow you to check the status of individual applications, but not the cluster overall. As it stands, the only way to check the status of the cluster is to visit the web interface for the Master.

This simple python3 script fills this gap.

## Usage

The basic usage of the script is available via the `--help` command line argument.

     $ spark-status --help
     usage: spark-status [-h] [-c [CONF]] [-u [URL]] [-a] [-f]

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

For example, you might execute the following to see the overall status of the master and all applications, including those that have finished.

    $ spark-status --finished
    # Cluster Status
    Alive Workers: 20
    Cores in use:  704 Total, 0 Used
    Memory in use: 3.7 TB Total, 0.0 B Used
    Applications:  0
    Drivers:       0 Running, 0 Completed
    Status:        ALIVE

    # Applications
    JoinTest                       2018/06/14 12:11:51 cbw        FINISHED 6 s
    JoinTest                       2018/06/14 11:57:43 cbw        FINISHED 7 s
    JoinTest                       2018/06/14 11:53:03 cbw        FINISHED 7 s
    JoinTest                       2018/06/14 11:53:08 cbw        FINISHED 2 s
    JoinTest                       2018/06/14 11:49:16 cbw        FINISHED 7 s

## Dependencies and Requirements

This script is written in Python 3 and requires [urllib3](https://pypi.org/project/urllib3/), [requests](https://pypi.org/project/requests/), and [beautifulsoup4](https://pypi.org/project/beautifulsoup4/).

This script retrieves information from the Spark Master by visiting and scraping its web interface. The script attempts to determine the URL of the Spark Master from the spark-defaults.conf file. By default, this script wants to live in the `spark-2.*.*/bin/` folder. If you run the script without specifying the `--url` for the Spark Master, or the `--conf` full path to the Spark configuration file (spark-defaults.conf), then the script will assume that the relative path `../conf/spark-defaults.conf` is correct.

This script is designed to work with Spark clusters that are being run in standalone mode, using the built-in Spark Master. **This script is not designed to work with Yarn or Mesos clusters**.

## Compatibility

This script has been tested against Spark 2.3.\*. I suspect that it will also work for Spark 2.2.\*, and possibly older branches of the 2.\*.\* line. 