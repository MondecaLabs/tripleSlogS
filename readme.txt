Content :
   1 Install and structure of project
   2 COMMAND LINE version
     2.1 About version
     2.2 Help screen
     2.3 Table of ACTIONS
     2.4 Table of SUPPORT OPTIONS 
     2.5 Table of ARGUMENTS
     2.6 STATISTICS and ERRORS
     2.7 Structure DB
     2.8 Examples
   3 WEB version
     3.1 About version
  
======================================================================================================================================

1 Install and structure of project
     
     This project includes:
       - command line version (JAR file)
       - web version (WAR file)
       - installed MongoDB and Java drivers
       - file of settings "settings.cfg"
       - help file "readme.txt"

     Steps for install MongoDB in Window:
       - in project used 2.2.0 version of MongoDB
       - unzip MongoDB.zip to C:\
       - add "C:\mongodb\bin" to system variable Path 
       - execute command:
            "mongod --config C:\mongodb\mongod.cfg --install"
       - press WIN+R, input and run 'services.msc'
       - find service 'MongoDB' and set to it in Automatic state

     Requirements for Java and plugins:
       - 1.8.X version of Java Runtime Environment (JRE)
       - for verifying version of Java can input command "java -version"

======================================================================================================================================

2 COMMAND LINE version

2.1 About version
     In command line version can input TripleSlogS plugin with attributes in follow format:
          "java -jar TriplesLogs.jar [-options] [-support]"
     This program takes as input NoSQL store or text log files of tree vendors (virtuoso_http, virtuoso_sparql and GDB), analyzes 
data and output in report file info of text parsing, sparql parsing and calculation statistic. Paths of input log files and output 
report can set as attributes in command line or in file of settings.

======================================================================================================================================

2.2 Help screen

*** Action Options ***
usage: java -jar Tripleslogs.jar [-options] [-support]

where options include:
   -h,--help                Help on command line
   -t,--test <logfile>      Test mode (show only files/queries with errors without saving in DB)
   -r,--report <logfile>    Generate report without saving to DB from log file or folder
   -a,--add <logfile>       Add queries from log file or folder
   -s,--save <logfile>      Add or update queries from log file or folder
   -l,--load                Load data from DB
   -d,--delete              Delete log files and queries from DB

*** Support Options***
usage: java -jar Tripleslogs.jar [-action] [-options]

where options include:
   -all                     Execute action for all log types
   -type <logtype>          Type of logs (can be follow values: vh, vs, gdb)
   -f,--full                Output statistics and count of errors for every log file
   -g <repname>             Path and name of report
   -distinct                Show only distinct queries in test mode
   -period <date|from:to>   Date period for deleting or loading Virtuoso_htp and GDB data in format '20yy-MM-dd'
   -range <index|min:max>   Range for deleting or loading Virtuoso_sparql data in format '123456|123456:123456'

======================================================================================================================================

2.3 Table of ACTIONS

   Required the presence of the same value. When found several actions left only first action and ignored others.
   <logtype> argument is optional and has default value from settings.cfg file which described in section 2.5 below.

 -------------------------------------------------------------------------------------------------------------------------------------
|   Action    |  Argument  |                        Description                      |               Support options*                 |
|-------------------------------------------------------------------------------------------------------------------------------------|
| -h/--help   |            | Help Screen                                             |                                                |
|-------------------------------------------------------------------------------------------------------------------------------------|
| -t/--test   | <logfile>  | Show files only with encode and unparsed errors without  | -distinct  Show only distinct wrong queries    |
|             |            | update DB                                               | -f/--full  Show full text of wrong queries and |
|             |            |                                                         |            first line from error message       |
|-------------------------------------------------------------------------------------------------------------------------------------|
| -r/--report | <logfile>  | Show report without update DB                           |                                                |
|-------------------------------------------------------------------------------------------------------------------------------------|
| -a/--add    | <logfile>  | Build report and add queries in DB as new               |                                                |
|             |            | Faster then save mode (it's better to use in first time)|                                                |
|-------------------------------------------------------------------------------------------------------------------------------------|
| -s/--save   | <logfile>  | Build report and save queries in DB (new queries are    |                                                |
|             |            | added and old queries are updated).                     |                                                |
|             |            | Virtuoso_http, GDB: log files are compared by date,     |                                                |
|             |            |                     queries are compared by datetime    |                                                |
|             |            |                     and content**                       |                                                |
|             |            | Virtuoso_sparql   : log files and queries are compared  |                                                |
|             |            |                     by index given from name log file   |                                                |
|-------------------------------------------------------------------------------------------------------------------------------------|
| -l/--load   |            | Load data from DB                                       |                                                |
|-------------------------------------------------------------------------------------------------------------------------------------|
| -d/--delete |            | Delete log files and queries from DB                    |                                                |
 -------------------------------------------------------------------------------------------------------------------------------------
   *  - Only individual support options or common options with individual notes. All common options are described in section 2.4 below.
   ** - Queries with equal datetime and content are ignored. In first time for loading data to DB better use add mode.

======================================================================================================================================

2.4 Table of SUPPORT OPTIONS 
 
   All support options are optional but they have required arguments. When option not set take default value from settings.cfg.

 -------------------------------------------------------------------------------------------------------------------------------------
|   Option  |     Argument   |  Actions |             Description             |      Default values for option from settings.cfg      |
|-------------------------------------------------------------------------------------------------------------------------------------|
| COMMON support options:                                                                                                             |
|-------------------------------------------------------------------------------------------------------------------------------------|
| -all      |                | for all  | Execute action for each log types   |                                                       |
|           |                |          | in cycle                            |                                                       |
|-------------------------------------------------------------------------------------------------------------------------------------|
| -type     | <logtype>      | for all  | Type of vendors for current action: |   default.logs.type=vh                                |
|           |                |          |  vh  - Virtuoso_http                |                                                       |
|           |                |          |  vs  - Vitruoso_sparql              |                                                       |
|           |                |          |  gdb - GDB                          |                                                       |
|-------------------------------------------------------------------------------------------------------------------------------------|
| -f/--full |                | except   | Output to report statistics for     |                                                       |
|           |                | delete   | every log file                      |                                                       |
|-------------------------------------------------------------------------------------------------------------------------------------|
| -g        | <repname>      | except   | Path and name of report.            | report / add / save   modes:                          |
|           |                | delete   | Note: When <repname> contains path  |   default.report.logs.vh=Reports/report-logs-vh.txt   |
|           |                |          |       need this path exists already |   default.report.logs.vs=Reports/report-logs-vs.txt   |
|           |                |          |                                     |   default.report.logs.gdb=Reports/report-logs-gdb.txt |
|           |                |          |                                     | test mode:                                            |
|           |                |          |                                     |   default.report.test.vh=Reports/report-test-vh.txt   |
|           |                |          |                                     |   default.report.test.vs=Reports/report-test-vs.txt   |
|           |                |          |                                     |   default.report.test.gdb=Reports/report-test-gdb.txt |
|           |                |          |                                     | load mode:                                            |
|           |                |          |                                     |   default.report.load.vh=Reports/report-load-vh.txt   |
|           |                |          |                                     |   default.report.load.vs=Reports/report-load-vs.txt   |
|           |                |          |                                     |   default.report.load.gdb=Reports/report-load-gdb.txt |
|           |                |          |                                     |                                                       |
|           |                |          |                                     | Note: if don't need to have the various reports, you  |
|           |                |          |                                     |      need to specify everywhere the same value        |
|-------------------------------------------------------------------------------------------------------------------------------------|
| INDIVIDUAL support options:                                                                                                         |
|-------------------------------------------------------------------------------------------------------------------------------------|
| -period   | <date|from:to> | load     | Date period for deleting or loading data from DB (format '20yy-MM-dd|20yy-MM-dd:20yy-MM-dd')|
|           |                | delete   | for Virtuoso_http and GDB vendors (for Virtuoso_sparql will be ignored)                     |
|-------------------------------------------------------------------------------------------------------------------------------------|
| -range    | <index|min:max>| load     | Integer range for deleting or loading data from DB in format '123456|123456:123456' for     |
|           |                | delete   | Virtuoso_sparql (for Virtuoso_http and GDB will be ignored)                                 |
|-------------------------------------------------------------------------------------------------------------------------------------|
| -distinct |                | test     | Shows only distinct wrong queries in test mode                                               |
 -------------------------------------------------------------------------------------------------------------------------------------

======================================================================================================================================

2.5 Table of ARGUMENTS

   Must be after options otherwise will be ignored.

 -------------------------------------------------------------------------------------------------------------------------------------
|     Argument    |  Actions |             Description             |            Default values for argument from settings.cfg         |
|-------------------------------------------------------------------------------------------------------------------------------------|
|  <logfile>      |  test    | Optional argument for name of log   | Path relative of project executing folder:                       |
|                 |  report  | file or folder with log files       |   default.logs.vh=Logs/V_http                                    |
|                 |  add     |                                     |   default.logs.vs=Logs/V_sparql                                  |
|                 |  save    |                                     |   default.logs.gdb=Logs/GDB                                      |
|-------------------------------------------------------------------------------------------------------------------------------------|
| <date|from:to>  |  load    | Format: '20yy-MM-dd|20yy-MM-dd:20yy-MM-dd':                                                            |
|                 |  delete  |   20   first two digits need be equal '2' and '0'                                                      |
|                 |          |   yy   two digits for year                                                                             |
|                 |          |   MM   one or two digits for month (can be with leading zero at begin)                                 |
|                 |          |   dd   one or two digits for day (can be with leading zero at begin)                                   |
|                 |          | Сan be a single date '20yy-MM-dd' or two dates separated by a colon '20yy-MM-dd:20yy-MM-dd'            |
|-------------------------------------------------------------------------------------------------------------------------------------|
| <index|min:max> |  load    | Format: '123456|123456:123456':                                                                        |
|                 |  delete  |   123456   from one to six 0-9 digits                                                                  |
|                 |          | Сan be a one group of from one to six digits or two groups digits separated by a colon                 |
 -------------------------------------------------------------------------------------------------------------------------------------

======================================================================================================================================

2.6 STATISTICS and ERRORS

     This program calculates the following statistics for each log file: 
         - Number of all queries
         - Number of distinct queries in format "{ N / M, p% }": 
                N - number of distinct queries 
                M - number of all queries 
                p% - percentage of distinct queries in file
         - Number of Basic Graph Pattern (BGP) in all queries
         - Number and percentage of CONSTRUCT, ASK, DESCRIBE, SELECT queries in format "{ N / M, p% }"
         - Number of following constructs in format "{ N / M, p% }": 
                UNION, DISTINCT, ORDER BY, REGEX, LIMIT, OFFSET, OPTIONAL, FILTER, GROUP BY

     Summary statistics for all analyzed log files (all values output in format "N / M, p%"): 
         - Number of duplicate queries 
         - Number of ASK, DESCRIBE, SELECT and CONSTRUCT in distinct queries

     After statistics output summary information about all log files and found errors in these:
         - Number of all analyzed log files, number of files with any errors and number of files with distinct errors
         - Number of queries in all log files, numbers of all queries and distinct queries with encoded and unparsed errors 
           in format "{ N / M, p% }"

======================================================================================================================================

2.7 Structure DB
    
     MongoDB store includes three DB (for virtuoso_http, virtuoso_sparql and GDB vendors). Can set new names of databases 
in settings file. For example, the log files of virtuoso_sparql vendor has not relation with date and better for log files 
from every year save in different database.
     Every DB includes two documents:
        - "logs" (fields: vendor type, name, index (for virtuoso_sparql), sparqlFiles (range of sparql files), errors, countBGP, 
                          date of created, date of logging, statistics for GDB, common statistics)
        - "queries" (fields: vendor type, id of log file, name of sparql file, date of created, date of logging, errors, 
                             content of sparql query, statistics for GDB, common statistics)

======================================================================================================================================

2.8 Examples

     HELP action:
        java -jar TriplesLogs.jar [|-h|--h|-help|--help]

     TEST action:
        java -jar TriplesLogs.jar -t|-test|--t|--test      (test of default type, default path of logs and default report name)
        java -jar TriplesLogs.jar -t -all                  (test of all types with default path of logs and report name for each type)
        java -jar TriplesLogs.jar -t Logs/VH -type vh      (test of virtuoso_http with established path of logs and default report name)
        java -jar TriplesLogs.jar -t -type vh -f           (test of virtuoso_http, default path of logs, default report name and 
                                                            output content of queries with errors)
        java -jar TriplesLogs.jar -t -type sh -f -distinct (test of virtuoso_sparql, path of logs, report name and 
                                                            output content only for distinct queries with errors)
        java -jar TriplesLogs.jar -t -f -g Rep/test2.txt   (test of default type, default path of logs, established report name and 
                                                            output errors)

     LOAD action:
        java -jar TriplesLogs.jar -l                                  (load data of default type, default report name)
        java -jar TriplesLogs.jar -l -all -f                          (load full data of all types with default report name)
        java -jar TriplesLogs.jar -l -type gdb                        (load data of GDB with default report name)
        java -jar TriplesLogs.jar -l -f -period 2016-04-01:2016-05-07 (load full data of default type, default report name 
                                                                       from 2016-04-01 to 2016-05-07)
        java -jar TriplesLogs.jar -l -type gdb -f -period 2016-04-01  (load full data of GDB, default report name for 2016-04-01)
        java -jar TriplesLogs.jar -l -type vs -g rep_vs.txt -range 1  (load data of virtuoso_sparql, established report name for index 1)
        java -jar TriplesLogs.jar -l -type vs -range 1:1000           (load data of virtuoso_sparql, default report name for indexes 
       
	DELETE action:
        java -jar TriplesLogs.jar -d -all                             (delete all data from all current databases for vendors)
        java -jar TriplesLogs.jar -d                                  (delete all data from database for default type)
        java -jar TriplesLogs.jar -d -type vh -period 2016-04-01:2016-05-07 (delete virtuoso_http data from 2016-04-01 to 2016-05-07)
        java -jar TriplesLogs.jar -d -type vs -range 1000:2000        (delete virtuoso_sparql data for index from 1000 to 2000)
                                                                       from 1 to 1000)

======================================================================================================================================

3 WEB version

3.1 About version

     Web version of project includes war file wich need deployed to Tomcat Server. Main page of web intarface is located at 
"http://localhost:8080/tripleslogs/". User interface has following fields:
        - 'Choice Files' button for choice of log file(s)
        - label by right of choice button with name of choiced file or number choiced files
        - 'Add File(s)' button for upload of choiced file(s) to temp directory (max size for uploading now equals 5MB)
          Note: before every uploading need choice them.
        - label by right of upload button with number of uploaded files or with info of upload error (on red color)
        - 'Clear All Files' button for clearing temp directory with uploaded files
        -  group of checkboxes for choice of type vendor ('Virtuoso Http', 'Virtuoso Sparql' and 'GDB')
        - 'Report' button for build report in new browser tab for uploaded files from temp directory and for choiced type
        - label by right of report button with info uploaded files with distinct names in temp directory
