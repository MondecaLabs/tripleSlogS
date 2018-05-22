# tripleSlogS
Repository for a JAVA tool based application for gathering statistics out of SPARQL queries logs

This project includes:
       - command line version under /CLI directory (JAR file)
       - web version (WAR file) under /Web directory
       - installed MongoDB and Java drivers for Windows under /NoSQL directory.
       - file of settings "settings.cfg" for using MongoDB
       
	   
# Prerequisite 
- The project uses  MongoDB 2.2.0
- Java 1.8.X Runtime Environment (JRE) should be installed. 
# Installation

Steps for installing MongoDB in Windows environment:
       
       - Unzip MongoDB.zip to your favourite folder e.g., C:\
       - add "C:\mongodb\bin" to system variable Path 
       - execute command:
            `mongod --config C:\mongodb\mongod.cfg --install `
       - press WIN+R, input and run 'services.msc'
       - find service 'MongoDB' and set to it in Automatic state

# Example Usage
`
	

     TEST action:
		java -jar TriplesLogs.jar [|-h|--h|-help|--help]	(Help to use tripleSlogS)
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
																	   `
## A live version of tripleSlogS is available at http://client2.mondeca.com/tripleslogs/ 
