---
layout: post
title:  "Integration Services (SSIS) - Custom Log Provider"
comments: true
---

> This blog summarizes the steps needed to create and deploy custom log provider. It is based on the excellent documentation provided by Microsoft.

> The links and examples in this blog are targeting SQL Server 2016.

### Create
1. Create a new class library project. For the examples I am using C# but any of the managed programming languages in .NET can be used. 
   
2. Reference [`Microsoft.SqlServer.ManagedDTS.dll`][3]{:target="_blank"} into your project which contain the [`Microsoft.SqlServer.Dts.Runtime`][4]{:target="_blank"} namespace. The DLL can be found at the locations listed in [*Locating Assemblies*][5]{:target="_blank"}.

3. Create a class and inherit from [`LogProviderBase`][6]{:target="_blank"}. Then override base methods and properties with logic needed for formatting the log data according to your needs.

4. Apply the [`DtsLogProviderAttribute`][7]{:target="_blank"} to the class which provides design-time information. The [*DisplayName*][8]{:target="_blank"} and the [*LogProviderType*][9]{:target="_blank"} properties are required.

5. [Sign the assembly][10]{:target="_blank"} to be generated with a strong name. This step is required in order to register the assembly in the Global Assembly Cache (GAC).

6. Build the project.
  
> Custom log provider - simple example

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

### Deploy

1. Copy the compiled assembly to the `LogProviders`  folder.

    `C:\Program Files (x86)\Microsoft SQL Server\130\DTS\LogProviders`

2. Install the assembly in the Global Assembly Cache (GAC)
    * Open the **Developer Command Prompt**
    * If the GAC has earlier version already install, it has to be removed first.
     
        `gacutil /u MyCustomLogProvider.dll`

    * Install the new version
        
        `gacutil /iF MyCustomLogProvider.dll`
    
After deployment restart of the SSIS Designer is required. 
   

### Debug

More information about testing and debugging custom log provider can be found [here][14]{:target="_blank"}.

### Links to Microsoft Docs
- [Creating a Custom Log Provider][2]{:target="_blank"}
- [Coding a Custom Log Provider][13]{:target="_blank"}
- [Developing Custom Objects for Integration Services][11]{:target="_blank"}
- [Building, Deploying, and Debugging Custom Objects][12]{:target="_blank"}
- [Signing the Assembly][10]{:target="_blank"}
- [Testing and Debugging Your Code][14]{:target="_blank"}
  
<!-- Links -->
[2]: https://docs.microsoft.com/en-us/sql/integration-services/extending-packages-custom-objects/log-provider/creating-a-custom-log-provider?view=sql-server-2016
[3]: https://docs.microsoft.com/en-us/sql/integration-services/integration-services-programming-overview?view=sql-server-2016#commonly-used-assemblies
[4]: https://docs.microsoft.com/en-us/dotnet/api/microsoft.sqlserver.dts.runtime?view=sqlserver-2016
[5]: https://docs.microsoft.com/en-us/sql/integration-services/integration-services-programming-overview?view=sql-server-2016#locating-assemblies
[6]: https://docs.microsoft.com/en-us/dotnet/api/microsoft.sqlserver.dts.runtime.logproviderbase?view=sqlserver-2016
[7]: https://docs.microsoft.com/en-us/dotnet/api/microsoft.sqlserver.dts.runtime.dtslogproviderattribute?view=sqlserver-2016
[8]: https://docs.microsoft.com/en-us/dotnet/api/microsoft.sqlserver.dts.runtime.localization.dtslocalizableattribute.displayname?view=sqlserver-2016
[9]: https://docs.microsoft.com/en-us/dotnet/api/microsoft.sqlserver.dts.runtime.dtslogproviderattribute.logprovidertype?view=sqlserver-2016
[10]: https://docs.microsoft.com/en-us/sql/integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects?view=sql-server-2016#signing
[11]: https://docs.microsoft.com/en-us/sql/integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services?view=sql-server-2016
[12]: https://docs.microsoft.com/en-us/sql/integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects?view=sql-server-2016
[13]: https://docs.microsoft.com/en-us/sql/integration-services/extending-packages-custom-objects/log-provider/coding-a-custom-log-provider?view=sql-server-2016
[14]: https://docs.microsoft.com/en-us/sql/integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects?view=sql-server-2016#testing