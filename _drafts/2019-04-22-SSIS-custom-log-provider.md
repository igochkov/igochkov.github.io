---
layout: post
title:  "SSIS Custom Log Provider"
comments: true
---

SQL Server Integration Services (SSIS) has excellent logging capabilities. Logging can be done in packages, containers tasks and captures wide range of events during package execution. The logs can be written to multiple locations. 

When you configure logging you have to choose log provider and optionaly connection manager. The log provider specifies the format for the log data and stores it at the location specified by the connection manager. SSIS includes the most common log providers:
- Text File
- XML File
- SQL Server Profiler
- SQL server
- Windows Event Log

The Text File, XML File, and SQL Server Profiler log providers use File connection manager to specify the path to the file. SQL Server log provider uses OLE DB connection manager to connect to a SQL database. The Windows Event Log does not use a connection manager at all as it writes to the Event Log directly.

But what if you need to format your log data differently? In case the above mentioned log providers does not fit your needs, you can create your own custom log provider.

### Custom log provider

#### Create
In order to create a custom log provider we have to create new project of type Class Library. Any of the managed programming languages in .NET can be used. Reference `Microsoft.SqlServer.ManagedDTS.dll` into you project which contain the `Microsoft.SqlServer.Dts.Runtime` namespace.

TODO: explain where to find Microsoft.SqlServer.ManagedDTS.dll

Create a class and inherit from `LogProviderBase`. Apply the `DtsLogProviderAttribute` data attribute to it. Then override *InitializeLogProvider*, *OpenLog*, *Log* and *CloseLog* methods with logic needed for formatting the log data according to your needs.

TODO: code example

#### Deploy
TODO: deployment steps

#### Debug
TODO: how to debug

### SSIS Logging

#### Enable (VS & server side)

#### Configure (visual & config file)





### Links
1. [Integration Services (SSIS) Logging](https://docs.microsoft.com/en-us/sql/integration-services/performance/integration-services-ssis-logging?view=sql-server-2017)
2. [Creating a Custom Log Provider](https://docs.microsoft.com/en-us/sql/integration-services/extending-packages-custom-objects/log-provider/creating-a-custom-log-provider?view=sql-server-2017)
3. [Developing Custom Objects for Integration Services](https://docs.microsoft.com/en-us/sql/integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services?view=sql-server-2017)
4. [Building, Deploying, and Debugging Custom Objects](https://docs.microsoft.com/en-us/sql/integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects?view=sql-server-2017)