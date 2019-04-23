---
layout: post
title:  "SSIS Custom Log Provider"
comments: true
---

> This blog summarizes the steps needed to create deploy and use custom log provider and it is based on the excellent documentation provided by Microsoft.


SQL Server Integration Services (SSIS) has extended logging capabilities. Logging can be done in packages, containers tasks and captures wide range of events during package execution. The logs can be written to multiple locations simulteniously. 

To configure logging you have to choose log provider and optionaly connection manager. The log provider specifies the format for the log data which is stored at the location specified by the connection manager. SSIS includes the most common log providers:
- Text File
- XML File
- SQL Server Profiler
- SQL server
- Windows Event Log

But what if you need to format your log data differently? In case the above mentioned log providers does not fit your needs, you can create your own custom log provider.

### Custom Log Provider

#### [Create](https://docs.microsoft.com/en-us/sql/integration-services/extending-packages-custom-objects/log-provider/creating-a-custom-log-provider?view=sql-server-2017){:target="_blank"}
1. Create a new class library project. Any of the managed programming languages in .NET can be used. 
   
2. Reference [`Microsoft.SqlServer.ManagedDTS.dll`](https://docs.microsoft.com/en-us/sql/integration-services/integration-services-programming-overview?view=sql-server-2017#commonly-used-assemblies){:target="_blank"} into your project which contain the [`Microsoft.SqlServer.Dts.Runtime`](https://docs.microsoft.com/en-us/dotnet/api/microsoft.sqlserver.dts.runtime?view=sqlserver-2017){:target="_blank"} namespace. The DLL can be found as described [here](https://docs.microsoft.com/en-us/sql/integration-services/integration-services-programming-overview?view=sql-server-2017#locating-assemblies){:target="_blank"}.

3. Create a class and inherit from [`LogProviderBase`](https://docs.microsoft.com/en-us/dotnet/api/microsoft.sqlserver.dts.runtime.logproviderbase?view=sqlserver-2017){:target="_blank"}. Then override base methods and properties with logic needed for formatting the log data according to your needs.

4. Apply the [`DtsLogProviderAttribute`](https://docs.microsoft.com/en-us/dotnet/api/microsoft.sqlserver.dts.runtime.dtslogproviderattribute?view=sqlserver-2017){:target="_blank"} to the class which provides design-time information. The [*DisplayName*](https://docs.microsoft.com/en-us/dotnet/api/microsoft.sqlserver.dts.runtime.localization.dtslocalizableattribute.displayname?view=sqlserver-2017){:target="_blank"} and the [*LogProviderType*](https://docs.microsoft.com/en-us/dotnet/api/microsoft.sqlserver.dts.runtime.dtslogproviderattribute.logprovidertype?view=sqlserver-2017){:target="_blank"} properties are required.

5. [Sign the assembly](https://docs.microsoft.com/en-us/sql/integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects?view=sql-server-2017#signing){:target="_blank"} to be generated with a strong name.

6. Build the assembly

``` csharp
using System;
using Microsoft.SqlServer.Dts.Runtime;

namespace MyNamespace
{
    [DtsLogProvider(
        DisplayName = "MyCustomLogProvider",
        Description = "This is my first custom log provider",
        LogProviderType = "Custom")]
    public class MyCustomLogProvider : LogProviderBase
    {
        /// <summary>
        /// Called when the log provider is added to a package.
        /// </summary>
        public override void InitializeLogProvider(
            Connections connections, IDTSInfoEvents events, 
            ObjectReferenceTracker refTracker) { }

        /// <summary>
        /// Called to confirm the log provider is properly configured.
        /// </summary>
        public override DTSExecResult Validate(IDTSInfoEvents events) { }

        /// <summary>
        /// Called at the beginning of package execution to establish connections 
        /// to external data sources.
        /// </summary>
        public override void OpenLog() { }

        /// <summary>
        /// Called when a runtime event occurs during package execution.
        /// </summary>
        public override void Log(
            string logEntryName, 
            string computerName, string operatorName, string sourceName, 
            string sourceID, string executionID, 
            string messageText, 
            DateTime startTime, DateTime endTime, 
            int dataCode, byte[] dataBytes) { }

        /// <summary>
        /// Called at the end of package execution.
        /// </summary>
        public override void CloseLog() { }
    }
}
```

#### [Deploy](https://docs.microsoft.com/en-us/sql/integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects?view=sql-server-2017#deploying){:target="_blank"}

TODO: deployment steps

#### [Debug](https://docs.microsoft.com/en-us/sql/integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects?view=sql-server-2017#testing){:target="_blank"}

TODO: how to debug

### [SSIS Logging][1]{:target="_blank"}

#### Enable (VS & server side)

#### Configure (visual & config file)





### Links to Microsoft Docs
- [Integration Services (SSIS) Logging][1]
- [Creating a Custom Log Provider](https://docs.microsoft.com/en-us/sql/integration-services/extending-packages-custom-objects/log-provider/creating-a-custom-log-provider?view=sql-server-2017)
- [Developing Custom Objects for Integration Services](https://docs.microsoft.com/en-us/sql/integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services?view=sql-server-2017)
- [Building, Deploying, and Debugging Custom Objects](https://docs.microsoft.com/en-us/sql/integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects?view=sql-server-2017)
- [Signing the Assembly](https://docs.microsoft.com/en-us/sql/integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects?view=sql-server-2017#signing)
- [Locating SSIS Assemblies](https://docs.microsoft.com/en-us/sql/integration-services/integration-services-programming-overview?view=sql-server-2017#locating-assemblies)


<!-- Links -->
[1]: https://docs.microsoft.com/en-us/sql/integration-services/performance/integration-services-ssis-logging?view=sql-server-2017