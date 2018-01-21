#============================================================================
#	File:		mssql-server-linux-wideworldimportersdw-standard-2017.dockerfile
#
#	Summary:	Ths script creates an image of a Microsoft SQL Server on Linux
#               with WideWorldImportersDW Standard Database preinstalled. The deployed
#               SQL Server is a SQL Server 2017 Developer Edition. 
#               The Image is based on the latest Image from Microsoft. 
#               To build this Image you need the WideWorldImportersDW_Standard.bak file. 
#
#	Date:		2018-01-21
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

# For this to work you have to copy the files WideWorldImportersDW_Standard.bak, 
# to the directory this dockerfile is located in.
# From the documentation (http://docs.docker.com/reference/builder/#add):
# "The <src> path must be inside the context of the build; you cannot 
# ADD ../something/something, because the first step of a docker build is to 
# send the context directory (and subdirectories) to the docker daemon."
COPY WideWorldImportersDW-Standard.bak /var/opt/sqldatabase

# Select Edition of SQL Server. We use Developer Edition.
# If you select another Edition make sure that you have the appropiate license.
ENV MSSQL_PID=Developer 
ENV SA_PASSWORD=!demo54321
ENV ACCEPT_EULA=Y

# Give the SQL Server Service time to start
HEALTHCHECK --interval=10s  \
	CMD /opt/mssql-tools/bin/sqlcmd -S . -U sa -P !demo54321 \
		-Q "RESTORE DATABASE [WideWorldImportersDW] FROM  DISK = N'/var/opt/sqldatabase/WideWorldImportersDW-Standard.bak' WITH  FILE = 1,  MOVE N'WWI_Primary' TO N'/var/opt/mssql/data/WideWorldImportersDW.mdf',  MOVE N'WWI_UserData' TO N'/var/opt/mssql/data/WideWorldImportersDW_UserData.ndf',  MOVE N'WWI_Log' TO N'/var/opt/mssql/data/WideWorldImportersDW.ldf',  NOUNLOAD,  STATS = 5"