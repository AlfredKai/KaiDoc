# Jmeter

## Non-GUI Mode (Command Line mode)

|     |                                                                                                         |
| --- | ------------------------------------------------------------------------------------------------------- |
| -n  | This specifies JMeter is to run in non-gui mode                                                         |
| -t  | [name of JMX file that contains the Test Plan].                                                         |
| -l  | [name of JTL file to log sample results to].                                                            |
| -j  | [name of JMeter run log file].                                                                          |
| -r  | Run the test in the servers specified by the JMeter property "remote_hosts"                             |
| -R  | [list of remote servers] Run the test in the specified remote servers                                   |
| -g  | [path to CSV file] generate report dashboard only                                                       |
| -e  | generate report dashboard after load test                                                               |
| -o  | output folder where to generate the report dashboard after load test. Folder must not exist or be empty |

The script also lets you specify the optional firewall/proxy server information:

|     |                                       |
| --- | ------------------------------------- |
| -H  | [proxy server hostname or ip address] |
| -P  | [proxy server port]                   |

Example:

```bat
jmeter -n -t my_test.jmx -l log.jtl
```

After test you can create `Summary and Aggregate reports` from a `.jtl file` using `Jmeter GUI` or just using jenkins `Performance Plugin`.

## Pass properties on command line

[\_\_P function](http://jmeter.apache.org/usermanual/functions.html#__P)

```bat
__P
```

Example:  
Define the property value:

```bat
jmeter -Jgroup1.threads=7 -Jhostname1=www.realhost.edu
```

Fetch the values(**in jmx**):
`${__P(group1.threads)}` - return the value of group1.thread
`${__P(group1.loops)}` - return the value of group1.loops
`${__P(hostname,www.dummy.org)}` - return value of property hostname or www.dummy.org if not defined
In the examples above, the first function call would return 7, the second would return 1 and the last would return www.dummy.org (unless those properties were defined elsewhere!)

## CSV Data Set Config

using .csv file as variable input

[csv data set config](https://jmeter.apache.org/usermanual/component_reference.html#CSV_Data_Set_Config)
