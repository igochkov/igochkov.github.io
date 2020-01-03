---
layout: post
title:  "Integration Services (SSIS) - Logging"
comments: false
---

> This blog summarizes the steps needed to enable and configure SSIS logging. It is based on the excellent documentation provided by Microsoft.

> The links and examples in this blog are targeting SQL Server 2016.

### Enable

Logging can be enable in two ways: during development in Visual Studio with SQL Server Data Tools (SSDT) or after deployment on the SSIS Server itself. The logging configuration applied to the SSIS Server will override the logging configuration from SSDT.

#### Enable in SSDT

1. Open the package where you want to enable logging
   
2. Open logging dialog from the menu *SSIS/Logging...* (in VS2019 *Extensions/SSIS/Logging...*)

3. Select provider from the **Provider type** list and click Add.
   
4. In column **Configuration** select, if applicable, the configuration manager.

5. In case you want to add more log providers repeat steps 3 and 4.
   
6. Enable the package level check box and then select the logs to use.

    ![CNAME]({{ "/assets/ssis-logging/enable_logging_providers.png" | absolute_url }})

7. On the **Details** tab select the **Events** to log. Optionally select **Advanced** to specified which information to log. Current selection can be stored for future re-use as XML template by clicking the **Save** button.

    ![CNAME]({{ "/assets/ssis-logging/enable_logging_details_basic.png" | absolute_url }})

    ![CNAME]({{ "/assets/ssis-logging/enable_logging_details_advanced.png" | absolute_url }})

8. Confirm selection by clicking **OK** and save the package.

#### Enable on the SSIS Server during package execution

1. In SQL Server Management Studio, navigate to the package in Object Explorer.

2. Right-click the package and select **Execute**.

3. Select the **Advanced** tab in the **Execute Package** dialog box.

4. Under **Logging level**, select the logging level. This topic contains a description of available values.

5. Complete any other package configurations, then click **OK** to run the package.

    ![CNAME]({{ "/assets/ssis-logging/enable_logging_server.png" | absolute_url }})

### Configure

The configuration of the logging can be done visually via the *SSIS Logs Dialog Box* or with *Saved Configuration File*. The second is handy when exactly the same configuration has to be applied multiple times so we need a template.

#### Configure with SSIS Logs Dialog Box

    TODO: write

#### Configure with Saved Configuration File

    TODO: write

### Links to Microsoft Docs
- [Integration Services (SSIS) Logging][1]{:target="_blank"}
  
<!-- Links -->
[1]: https://docs.microsoft.com/en-us/sql/integration-services/performance/integration-services-ssis-logging?view=sql-server-2016