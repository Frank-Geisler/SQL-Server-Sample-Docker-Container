#========================================================================================================
#	File:		mssql-server-linux-wideworldimportersdw-standard-2017.dockerfile
#
#	Summary:	Ths script creates an image of a Microsoft SQL Server on Linux
#               with WideWorldImportersDW Standard Database preinstalled. The deployed
#               SQL Server is a SQL Server 2017 Developer Edition. 
#               The Image is based on the latest Image from Microsoft. 
#               This Dockerfile automatically downloads the Backup of 
#               the WideWorldImportersDW-Full Database and restores it.
#               The database is downloaded here:
#               https://github.com/Microsoft/sql-server-samples/releases/tag/wide-world-importers-v1.0
#               Additionally it adds mssql_cli
#               (https://github.com/dbcli/mssql-cli)
#
#	Date:		2018-01-21
#
#   Revision:   2018-06-30
#                 - install mssql-cli (https://github.com/dbcli/mssql-cli)
#                 - changed from attach database to restore database 
#				  - get AdventureWorks2017DW Backup from official Microsoft
#                   github repository
#
#	SQL Server Version: 2017
#---------------------------------------------------------------------------------------------------------
#	Written by Frank Geisler, GDS Business Intelligence GmbH
#
#	This script is intended only for educational purposes
#  
#	THIS CODE AND INFORMATION ARE PROVIDED "AS IS" WITHOUT WARRANTY OF 
#	ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED 
#	TO THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
#	PARTICULAR PURPOSE.
#========================================================================================================
FROM microsoft/mssql-server-linux:latest
LABEL maintainer Frank Geisler <frank_geisler@geislers.net>

# Create a directory where we can copy the Database files. 
RUN mkdir /var/opt/sqldatabase

# Install pip and install mssql-cli
RUN apt-get update
RUN apt-get -y install python-pip
RUN pip install mssql-cli

# This downloads the WideWorldImportersDW-Full.bak file from the official
# Microsoft github repository
WORKDIR /var/opt/sqldatabase
RUN wget https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImportersDW-Standard.bak

# Select Edition of SQL Server. We use Developer Edition.
# If you select another Edition make sure that you have the appropiate license.
ENV MSSQL_PID=Developer 
ENV SA_PASSWORD=!demo54321
ENV ACCEPT_EULA=Y

# Give the SQL Server Service time to start
HEALTHCHECK --interval=10s  \
	CMD /opt/mssql-tools/bin/sqlcmd -S . -U sa -P !demo54321 \
		-Q "RESTORE DATABASE [WideWorldImportersDW] FROM  DISK = N'/var/opt/sqldatabase/WideWorldImportersDW-Standard.bak' WITH  FILE = 1,  MOVE N'WWI_Primary' TO N'/var/opt/mssql/data/WideWorldImportersDW.mdf',  MOVE N'WWI_UserData' TO N'/var/opt/mssql/data/WideWorldImportersDW_UserData.ndf',  MOVE N'WWI_Log' TO N'/var/opt/mssql/data/WideWorldImportersDW.ldf',  NOUNLOAD,  STATS = 5"