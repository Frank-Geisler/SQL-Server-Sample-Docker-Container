#============================================================================
#	File:		mssql-server-linux-adventureworks2017.dockerfile
#
#	Summary:	Ths script creates an image of a Microsoft SQL Server on Linux
#               with AdventureWorks2017 Database preinstalled. The deployed
#               SQL Server is a Developer Edition. The Image is based on the
#               latest Image from Microsoft. To build this Image you need
#               the Adventureworks2017.mdf and Adventureworks2017_log.ldf files.
#               These files can be downloaded here 
#               https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks
#
#	Date:		2018-01-16
#
#	SQL Server Version: 2017
#------------------------------------------------------------------------------
#	Written by Frank Geisler, GDS Business Intelligence GmbH
#
#	This script is intended only for educational purposes
#  
#	THIS CODE AND INFORMATION ARE PROVIDED "AS IS" WITHOUT WARRANTY OF 
#	ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED 
#	TO THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
#	PARTICULAR PURPOSE.
#============================================================================*/
FROM microsoft/mssql-server-linux:latest
LABEL maintainer Frank Geisler <frank_geisler@geislers.net>

# Create a directory where we can copy the Database files. 
RUN mkdir /var/opt/sqldatabase

# For this to work you have to copy the files AdventureWorks2017.mdf and 
# AdventureWorks2017_log.ldf to the directory this dockerfile is located in.
# From the documentation (http://docs.docker.com/reference/builder/#add):
# "The <src> path must be inside the context of the build; you cannot 
# ADD ../something/something, because the first step of a docker build is to 
# send the context directory (and subdirectories) to the docker daemon."
COPY AdventureWorks2017.mdf /var/opt/sqldatabase
COPY AdventureWorks2017_log.ldf /var/opt/sqldatabase

# Select Edition of SQL Server. We use Developer Edition.
# If you select another Edition make sure that you have the appropiate license.
ENV MSSQL_PID=Developer 
ENV SA_PASSWORD=!demo54321
ENV ACCEPT_EULA=Y

# Give the SQL Server Service time to start
# then log into the SQL Server and create the database on the copied files 
HEALTHCHECK --interval=10s  \
	CMD /opt/mssql-tools/bin/sqlcmd -S . -U sa -P !demo54321 \
		-Q "CREATE DATABASE [AdventureWorks2017] ON (FILENAME = '/var/opt/sqldatabase/AdventureWorks2017.mdf'),(FILENAME = '/var/opt/sqldatabase/AdventureWorks2017_log.ldf') FOR ATTACH"
