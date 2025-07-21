<div id="header" align="center">

[![GitHub issues](https://img.shields.io/github/issues-raw/BroadcomMFD/debugger-for-mainframe?style=flat-square)](https://github.com/BroadcomMFD/debugger-for-mainframe/issues)
[![slack](https://img.shields.io/badge/chat-on%20Slack-blue)](https://join.slack.com/t/che4z/shared_invite/zt-37ewynplx-wCoabaIDxN6Ofm4_XBinZA)
[![Code4z](https://img.shields.io/badge/Code4z-marketplace-cc092f)](https://marketplace.visualstudio.com/search?term=code4z&target=VSCode)

</div>

# Debugger for Mainframe

Debugger for Mainframe provides a debugging interface for [InterTest™ for CICS](https://www.broadcom.com/products/mainframe/devops-app-development/testing-quality/intertest-cics) and [InterTest™ Batch](https://www.broadcom.com/products/mainframe/testing-and-quality/intertest-batch). This extension provides a modern debugging experience for CICS and Batch programs written in COBOL and High-Level Assembler Language (HLASM).

<img align="left" alt="This extension is part of the Code4z experience" width="80" height="82" src="https://raw.githubusercontent.com/BroadcomMFD/code4z/refs/heads/main/icon5.png" />

Debugger for Mainframe is part of the [Code4z](https://techdocs.broadcom.com/code4z) experience from Broadcom, which offers a modern experience for mainframe application developers. To get started with Code4z, check out our foundational [extension pack](https://marketplace.visualstudio.com/items?itemName=broadcomMFD.code4z-extension-pack).

<br />

- [Address Software Requirements](#address-software-requirements)
- [Set Up Secure Connection](#set-up-secure-connection)
- [Getting Started](#getting-started)
- [Debugging](#debugging)
- [Breakpoints](#breakpoints)
- [Variables](#variables)
- [Call Trace and Statement Trace](#call-trace-and-statement-trace)
- [Integrate with Zowe Explorer](#integrate-with-zowe-explorer)
- [Troubleshooting](#troubleshooting)
- [Report Issues](#report-issues)
- [Technical Assistance and Support for Debugger for Mainframe](#technical-assistance-and-support-for-debugger-for-mainframe)
- [Privacy Notice](#privacy-notice)

## Address Software Requirements

Before you use Debugger for Mainframe, ensure that your site and workstation meet the following requirements:

### Server
- InterTest for CICS and/or InterTest Batch, incremental release 11.0.07 or higher.
- Acquire and install Level Set PTF 11.0.02 (LU15115) and PTFs LU14897, LU15349, LU15858, LU15993, LU15994, LU15995, LU15996 and LU16858. We recommend that you remain current on maintenance for InterTest.
- Configure the Testing Tools Server.
- To use Debugger for Mainframe to debug CICS programs, configure CICS IRC and one of TCP or CAICCI.
- To connect to Debugger for Mainframe via the Zowe API Mediation Layer, integrate your Testing Tools Server instance with Zowe API ML.

For information on PTF releases, the Testing Tools Server and API Mediation Layer, see the [InterTest documentation](http://techdocs.broadcom.com/itsd). To browse PTFs, see [Broadcom Support](https://ftpdocs.broadcom.com/WebInterface/phpdocs/0/MSPSaccount/COMPAT/AllProducts.HTML).

### Client
- Visual Studio Code version 1.67.0 and above, or Github Codespaces.
- Java version 8.0 or higher with the PATH variable correctly configured.

## Set Up Secure Connection
You can use Debugger for Mainframe over a secure connection if your Testing Tools Server instance is configured for a secure connection. 

To use Debugger for Mainframe over a secure connection, obtain the server certificate for your Testing Tools Server instance and import it to a certificate store. You can use a Java trust store or a trust store in your operating system.

### Import Server Certificate to Java Trust Store

We recommend that you import the certificate to the store of the JRE under which the Debugger for Mainframe DA client is running on your local machine. You can use the command line or a UI tool. 

#### Import Certificate using Command Line
    
Enter the following command: `sudo keytool -import -alias hostname -file hostname.cer -storetype JKS -keystore cacerts`
        
#### Import Certificate on a Linux Subsystem
     
1. To verify that Java is installed, run the command `java -version`.
2. Locate your subsystem's java installation.
3. Go to **/lib/security** to find **cacerts**.
4. Run the following command to import the certificate: `sudo keytool -import -alias hostname -file hostname.cer -storetype JKS -keystore cacerts`
          
#### Import Certificate using a UI Tool

1. In your preferred UI, locate and open **cacerts**.
2. Import the certificate to cacerts.
3. Name the certificate with an appropriate alias to ensure it is easily identified.
4. Save your changes.

### Specify Location of Certificate in OS Trust Store

You can also import the certificate to a certificate store in your operating system and specify the location in the extension settings.

1. Select **File** - **Preferences** - **Settings**.   
   The Settings menu opens. 
2. Open the **Extensions** submenu and locate **Debugger for Mainframe**.
3. Do one of the following:
  * On Windows, change **Native Certificate Store for Windows** to **Windows-ROOT**.
  * On Mac OS, fill out the following fields:
    * **Native Certificate Store for Mac OS > Trust Store Type**  
      * Specifies the certificate store type.  
      * Example: keychainStore.
    * **Native Certificate Store for Mac OS > Trust Store**
      * Specifies the path to the certificate store.  
      * Example: /Library/Keychains/System.keychain

### Verify Security Configuration
        
After you have imported your certificate, run a test debug session with **"interTestSecure": true** in your launch.json file. If the session fails, ensure that you imported the correct certificate to the correct JRE trust store and try again.

You have configured Debugger for Mainframe to use a secure connection to InterTest.

## Getting Started

Debugger for Mainframe includes two VS Code walkthroughs which guide you through the basic COBOL demo sessions which are shipped with InterTest for CICS and InterTest Batch. To access the walkthroughs, select **Welcome** from the **Help** menu, click **More...** under **Walkthroughs**, and select either **Basic CICS Demo Session** or **Basic Batch Demo Session**.

For full instructions on how to run the debugging demo sessions, see the [Code4z documentation](https://techdocs.broadcom.com/code4z).

### Run Your First Debug Session

To start debugging programs in your IDE, you first create a `launch.json` file within your workspace.

1. Select the Explorer icon in your IDE and open a folder.
    - The workspace opens.

2. Select the Bug icon to open **Debug view**.

3. In the sidebar, click **create a launch.json file**.  
   A list of configurations displays. 
   
4. Select either **Debugger for Mainframe: INTERTEST™ FOR CICS** or **Debugger for Mainframe: INTERTEST™ BATCH**

5. Fill in the necessary fields as described in the **[Add Configuration](#add-configuration)** section below.

6. Run the debug session as described in the **[Run a Debug Session](#run-a-debug-session)** section below.

## Debugging

To debug CICS and Batch programs with Debugger for Mainframe you open the workspace in your IDE and configure your connection to InterTest using the file `launch.json`. 

Debugged files are temporarily saved in the workspace within the ```/.c4z/.extsrcs``` folder.

To debug Batch programs, you also convert the JCL of your program into a new file which is used for debugging.

![](https://raw.githubusercontent.com/BroadcomMFD/debugger-for-mainframe/master/Setup%20and%20config%20Edited.gif)

### Add Configuration

The `launch.json` file contains configurations for debugging different types of programs. The configurations supported by Debugger for Mainframe are: 
* **Debugger for Mainframe: INTERTEST™ FOR CICS**  
  Basic configuration for CICS programs.
* **Debugger for Mainframe: INTERTEST™ BATCH**  
  Basic configuration for Batch programs.
* **Debugger for Mainframe: INTERTEST™ BATCH - Attach**  
  Configuration to attach Batch programs to the Batch Link Queue.

When you create a `launch.json` file for the first time, a configuration is added. Click the **Add configuration** button in the configuration drop-down list to add further configurations.

#### Basic CICS Configuration

To add a basic configuration to debug a CICS application, add a **Debugger for Mainframe: INTERTEST™ FOR CICS** configuration and populate the following fields:

  - **"type":** (string)
    - Specify "intertest-cics".
  - **"request":** (string)
    - Specify "launch".
  - **"name":** (string)
    - Specifies the name of the debugging session.
  - **"programName"**: (array)
    - Specifies the name of the program that you want to debug using this configuration. To debug a program along with other programs called within it, specify all program names you want to debug in this field.
    - To debug a program that is part of a composite module, use the format MODULE_PROGRAM. To debug all programs in a composite module, specify MODULE_.
      - **Tip**: To see the list of all programs in a composite module, add the module to this field, and run the pallet command **List Composites**. The list of programs displays in the Output Panel.
    - Specify an array with either one value or up to 30 values separated by commas.
  - **"host"**: (string)
    - Specifies the host address of your Testing Tools Server instance or Zowe API Mediation Layer Gateway. Do not include this field if you use the Zowe API ML Single Sign-On feature.
  - **"port"**: (integer)
    - Specifies the port number of your Testing Tools Server instance or Zowe API Mediation Layer Gateway. Do not include this field if you use the Zowe API ML Single Sign-On feature.
  - **"interTestUserName"**: (string)
    - Specifies your mainframe username. Do not include this field if you use the Zowe API ML Single Sign-On feature.
  - **"apimlProfile"**: (string)
    - Specifies the name of a Zowe Explorer profile that contains mainframe credentials and the host and port of your Zowe API Mediation Layer Gateway. Specify this field to use the Zowe API ML Single Sign-On feature.
  - **"path"**: (string)
    - (API Mediation Layer only) Specifies your Zowe API Mediation Layer Service ID. Do not include this field if you connect through a Testing Tools Server instance.
  - **"interTestSecure"**: (boolean)
    - Specify "true" to use a secure connection to the InterTest server or "false" to use a non-secure connection.  
      Ensure that you complete the steps in the **[Set Up Secure Connection](#set-up-secure-connection)** section if you want to use a secure connection.
  - **"cicsApplId"**: (string)
    - Specifies the CICS Application ID (cicsApplID) of your CICS region.
  - **"cicsUserId"**: (string)
    - (Optional) Specify a CICS user logon ID to debug a CICS program or transaction as it executes for that specific ID. You cannot modify this value during an active debugging session.
  - **"interTestCharset"**: (string)
    - (Optional) Specifies the Testing Tools Server Charset for Listings. Specify this field only if your Testing Tools Server instance is configured to use a client code page other than UTF-8.
  - **"paragraphBreakpoints"**: (boolean)
    - (Optional) Specify "true" to have the debugging session stop automatically at each new paragraph or label. For more information, see [Paragraph Breakpoints](#paragraph-breakpoints).
  - **"callTrace"**: (boolean)
    - (Optional) Specify "false" to disable the call trace feature. The feature is enabled by default. For more information, see [Call Trace and Statement Trace](#call-trace-and-statement-trace).
  - **"statementTrace"**: (boolean)
    - (Optional) Specify "false" to disable the statement trace feature. The feature is enabled by default. For more information, see [Call Trace and Statement Trace](#call-trace-and-statement-trace).
    - **Note**: If you disable this parameter, the "step out" and "step over" functions of the debug toolbar are also disabled.
  - **"variableOrder"**: (string)
    - (Optional) Specify "alphabetical" to sort the variables that are defined in your program in alphabetical order in the variables view. Specify "definition" to sort the variables in the order of their definition in the program. The default setting is "definition".

#### Basic Batch Configuration

To add a basic configuration to debug a batch application, add a **Debugger for Mainframe: INTERTEST™ BATCH** configuration and populate the following fields:

  - **"type":** (string)
    - Specify "intertest-batch".
  - **"request":** (string)
    - Specify "launch".
  - **"name":** (string)
    - Specifies the name of the debugging session.
  - **"programName"**: (array)
    - Specifies the name of the program that you want to debug using this configuration. To debug a program along with other programs called within it, specify all program names you want to debug in this field.
    - Specify an array with either one value or up to 30 values separated by commas.
  - **"protsym"**: (array)
    - Specify an array with up to 8 PROTSYM DSNs separated by commas.
  - **DSS**: (string)
    - (Optional) Specify the PROTSYM which is designated to receive dynamic symbolic data from Endevor. For more information, see **[Dynamic Symbolic Support](#dynamic-symbolic-support)**.
    - **Note**: The DSS PROTSYM counts towards the maximum of 8 PROTSYMs per configuration. If you specify this parameter, the maximum allowed number of PROTSYMs in the **"protsym"** array is reduced to 7.
  - **"host"**: (string)
    - Specifies the host address of your Testing Tools Server instance or Zowe API Mediation Layer Gateway. Do not include this field if you use the Zowe API ML Single Sign-On feature.
  - **"port"**: (integer)
    - Specifies the port number of your Testing Tools Server instance or Zowe API Mediation Layer Gateway. Do not include this field if you use the Zowe API ML Single Sign-On feature.
  - **"interTestUserName"**: (string)
    - Specifies your mainframe username. Do not include this field if you use the Zowe API ML Single Sign-On feature.
  - **"apimlProfile"**: (string)
    - Specifies the name of a Zowe Explorer profile that contains mainframe credentials and the host and port of your Zowe API Mediation Layer Gateway. Specify this field to use the Zowe API ML Single Sign-On feature.
  - **"path"**: (string)
    - (API Mediation Layer only) Specifies your Zowe API Mediation Layer Service ID. Do not include this field if you connect through a Testing Tools Server instance.
  - **"interTestSecure"**: (boolean)
    - Specify "true" to use a secure connection to the InterTest server or "false" to use a non-secure connection.  
      Ensure that you complete the steps in the **[Set Up a Secure Connection](#set-up-secure-connection)** section if you want to use a secure connection.
  - **"originalJCL"**: (JSON)
    - (Optional) Specify this parameter if you want Debugger for Mainframe to convert the JCL of your program for debugging.
      This is a JSON element containing the following fields:
        - **"inDSN"**: (string)
            - Specifies the DSN and member name of the JCL containing the program that you want to debug.
        - **"stepName"**: (string)
            - Specifies the step name of the program that you want to debug.
        - **"procLibs"**: (array)
            - (Optional) Specify an array with any number of DSNs containing procedure libraries. Specify this field if your JCL requires a procedure library to be converted.
  - **"convertedJCL"**: (string)
    - Specifies the DSN and member name where you want to store your converted JCL. Specify the full name of a partitioned data set and a member in the format DSN(MEMBER). Debugger for Mainframe creates or overwrites this member when you convert the JCL.
  - **"symbols"**: (JSON)
    - (Optional) Specify values for any number of variables in the converted JCL. Use the format `"PARAM": "VALUE"` and separate each entry by commas within this JSON element. 
    - The value on the right side of each entry can use input variables. For more information, see the [VS Code documentation](https://code.visualstudio.com/docs/editor/variables-reference#_input-variables).
  - **"interTestCharset"**: (string)
    - (Optional) Specifies the Testing Tools Server Charset for Listings. Specify this field only if your Testing Tools Server instance is configured to use a client code page other than UTF-8.
  - **"paragraphBreakpoints"**: (boolean)
    - (Optional) Specify "true" to have the debugging session stop automatically at each new paragraph or label. For more information, see [Paragraph Breakpoints](#paragraph-breakpoints).
  - **"statementTrace"**: (boolean)
    - (Optional) Specify "false" to disable the statement trace feature. The feature is enabled by default. For more information, see [Call Trace and Statement Trace](#call-trace-and-statement-trace).
  - **"executionCounts"**: (boolean)
    - (Optional) Specify "true" to enable the execution counts feature. The feature is disabled by default. For more information, see [Execution Counts](#execution-counts).
  - **"variableOrder"**: (string)
    - (Optional) Specify "alphabetical" to sort the variables that are defined in your program in alphabetical order in the variables view. Specify "definition" to sort the variables in the order of their definition in the program. The default setting is "definition". 
  - **"zoweProfileName"**: (string)  
    - (Optional) Specify the name of a Zowe profile to enable job status messages when you run your debugging session.

#### Enable the Batch Link Queue

Enable the Batch Link Queue to add the Suspend and Attach functionalities to your Batch debugging experience. These functionalities allow you to suspend and later resume a debugging session, for example:

* To run two debugging sessions simultaneously and switch between them.
* To start a debugging session on InterTest Batch and resume it on Debugger for Mainframe.

To enable the Batch Link Queue, add a **Debugger for Mainframe: INTERTEST™ FOR BATCH - Attach** configuration to your existing `launch.json` file, which must already contain at least one `intertest-batch` configuration. Populate the following fields:

  - **"type":** (string)
    - Specify "intertest-batch".
  - **"request":** (string)
    - Specify "attach".
  - **"name":** (string)
    - Specifies the name of the debugging session.
  - **"programName"**: (array)
    - (Optional) Specifies the name of the program that you want to debug using this configuration. To debug a program along with other programs called within it, specify all program names you want to debug in this field.
    - Specify an array with either one value or up to 30 values separated by commas.
  - **"protsym"**: (array)
    - (Optional) Specify an array with up to 8 PROTSYM DSNs separated by commas.
  - **DSS**: (string)
    - (Optional) Specify the PROTSYM which is designated to receive dynamic symbolic data from Endevor. For more information, see **[Dynamic Symbolic Support](#dynamic-symbolic-support)**.
    - **Note**: The DSS PROTSYM counts towards the maximum of 8 PROTSYMs per configuration. If you specify this parameter, the maximum allowed number of PROTSYMs in the **"protsym"** array is reduced to 7.
  - **"host"**: (string)
    - Specifies the host address of your Testing Tools Server instance or Zowe API Mediation Layer Gateway. Do not include this field if you use the Zowe API ML Single Sign-On feature.
  - **"port"**: (integer)
    - Specifies the port number of your Testing Tools Server instance or Zowe API Mediation Layer Gateway. Do not include this field if you use the Zowe API ML Single Sign-On feature.
  - **"interTestUserName"**: (string)
    - Specifies your mainframe username. Do not include this field if you use the Zowe API ML Single Sign-On feature.
  - **"apimlProfile"**: (string)
    - Specifies the name of a Zowe Explorer profile that contains mainframe credentials and the host and port of your Zowe API Mediation Layer Gateway. Specify this field to use the Zowe API ML Single Sign-On feature.
  - **"path"**: (string)
    - (API Mediation Layer only) Specifies your Zowe API Mediation Layer Service ID. Do not include this field if you connect through a Testing Tools Server instance.
  - **"interTestSecure"**: (boolean)
    - Specify "true" to use a secure connection to the InterTest server or "false" to use a non-secure connection.  
      Ensure that you complete the steps in the **[Set Up a Secure Connection](#set-up-secure-connection)** section if you want to use a secure connection.
  - **"interTestCharset"**: (string)
    - (Optional) Specifies the Testing Tools Server Charset for Listings. Specify this field only if your Testing Tools Server instance is configured to use a client code page other than UTF-8.
  - **"paragraphBreakpoints"**: (boolean)
    - (Optional) Specify "true" to have the debugging session stop automatically at each new paragraph or label. For more information, see [Paragraph Breakpoints](#paragraph-breakpoints).
  - **"statementTrace"**: (boolean)
    - (Optional) Specify "false" to disable the statement trace feature. The feature is enabled by default. For more information, see [Call Trace and Statement Trace](#call-trace-and-statement-trace).
  - **"executionCounts"**: (boolean)
    - (Optional) Specify "true" to enable the execution counts feature. The feature is disabled by default. For more information, see [Execution Counts](#execution-counts).
  - **"variableOrder"**: (string)
    - (Optional) Specify "alphabetical" to sort the variables that are defined in your program in alphabetical order in the variables view. Specify "definition" to sort the variables in the order of their definition in the program. The default setting is "definition". 

### Dynamic Symbolic Support

If you have integrated InterTest with Endevor on the backend, you can activate the dynamic symbolic support feature to dynamically load symbolic data into a PROTSYM. To enable dynamic symbolic support, specify a PROTSYM in the **"DSS"** parameter of a batch `launch` or `attach` configuration. This PROTSYM is searched along with the other PROTSYMs that you specify in the **"protsym"** array.

For more information about dynamic symbolic support, see the [InterTest and SymDump documentation](https://techdocs.broadcom.com/itsd).

### Run a Debug Session

After you define your configuration in `launch.json`, you can run your debug session in the debugging interface.

1. Open the **Run and Debug** panel.

2. (Batch only, optional) To convert your JCL, press **F1** to open the command pallet and run the command **Batch: Convert JCL**. Complete this step when debugging a program for the first time, or if your program changed since the last debug session.

3. Press **F1** to open the command pallet and run the command **Fetch Extended Sources**.

4. If you have more than one Debugger for Mainframe configuration defined in `launch.json`, a list of available configurations displays. Select the name of the configuration that you want to use to fetch your source.

5. If more than one listing is found in the CICS regions or PROTSYMs specified in your configuration, or if no exact matching listing is found, a list of listings displays. Select the listing that you want to load.

6. Enter your mainframe password if prompted.
    - The expanded source is displayed.

7. Set breakpoints and logpoints as required. You can set breakpoints and logpoints before you start the debugging process or as the process is running.
    
    - For more information, see **[Breakpoints](#breakpoints)**

8. Click the play icon in the top left of the interface to start the debugging process.

9. (CICS only) Run your program in your CICS region.

You have successfully initiated a debugging session. 

- Once the session is running, the debugging session stops at each breakpoint, or if an abend occurs.  
- You can use the **Continue**, **Step over**, **Step into** and **Step out** functions of the IDE Debug Toolbar. The **Restart** function is not supported. 
- To use the **Step over** and **Step out** functions during a CICS session, ensure that you enable statement trace in your `launch.json` file.
- Use the **Stop** button to terminate the debugging session. If you have a Batch Link Queue (`attach`) configuration in your `launch.json` file, you can use the drop-down arrow to switch between the **Stop** and the **Suspend** buttons. Use the **Suspend** button to temporarily terminate a debug session and add it to the Batch Link Queue, from which you can resume it later.
- If you specified a Zowe profile name in the `zoweProfileName` field of `launch.json`, a message with a batch job ID appears when the session completes. Click the job ID to open information about the job in Zowe Explorer.

### Run a Debug Session From the Batch Link Queue

To run a debug session from the Batch Link Queue, ensure that you have an `attach` configuration in your `launch.json` file (see [Enable the Batch Link Queue](#enable-the-batch-link-queue) for more information.)

1. Select an `attach` configuration from the list of configurations on the Run and Debug panel.
2. Enter your mainframe password if prompted.  
   The batch link queue opens.
3. Select the session that you want to resume from the drop-down list
   The listing is downloaded and the debugging session starts.

When you run a debug session using an `attach` configuration, the **Suspend** button is shown by default in the Debug Toolbar. Use the drop-down arrow to switch to the **Stop** button.

When you suspend a debug session, breakpoints that you define in Visual Studio Code are saved with the session. If you load a debug session from the Batch Link Queue that has breakpoints saved, these breakpoints override ones that are defined locally on the same lines.

### Manage the Batch Link Schedule Table

You can use Debugger for Mainframe to manage the Batch Link Schedule Table, which enables you to debug DB2 Stored Procedures and IMS DC applications.

To manage your Batch Link Scheduling Table, ensure that you have a batch `launch` or `attach` configuration that contains a server address and username, either directly or in a Zowe Explorer profile specified in the `apimlProfile` parameter. You can leave the `programName` and `protsym` fields in this configuration at their default values.

#### Schedule a Db2 Stored Procedure or IMS DC Application

To debug a Db2 stored procedure or an IMS DC application, you add the appropriate criteria to the Batch Link Schedule Table, from which you can later run it from the Batch Link Queue using an `attach` configuration.
    
1. Open the F1 menu and select **Batch: Show Scheduling Table**.
2. Select your batch configuration.
3. Enter your mainframe password if prompted.  
The Batch Link Scheduling Table displays.
4. Select **Add a new schedule entry**.
5. Select either **DB2** or **IMS**.
6. (Optional) Specify a name for your job and press enter.
7. Specify your program name and press enter.
8. (IMS only, optional) Specify an IMS transaction ID for the schedule entry and press enter. If you leave this prompt blank, all transactions are processed. This field can be wildcarded and cannot contain more than eight characters.
9. (IMS only, optional) Specify an IMS user ID for the schedule entry and press enter. This field can be wildcarded and cannot contain more than eight characters.  
The **Is this a one-time entry?** prompt displays.
10. Select **Yes** or **No**. If you select **Yes**, the session is deleted from the Batch Link Schedule Table after it is attached to the Batch Link Queue.  
The stored procedure is scheduled. To view it in the Batch Link Scheduling Table, repeat steps 1 to 3 above.

#### Delete a Batch Link Scheduling Table Entry
    
1. Open the F1 menu and select **Batch: Show Scheduling Table**
2. Select your batch configuration.
3. Enter your mainframe password if prompted.  
The Batch Link Scheduling Table displays.
4. Select the delete icon next to the procedure that you want to delete.
5. When prompted select **Yes** to confirm the deletion.
The entry is deleted.

## Breakpoints

Debugger for Mainframe supports several different kinds of breakpoints that you can use to monitor your debugging sessions.

### Conditional and Unconditional Breakpoints

Breakpoints can be unconditional or conditional. Unconditional breakpoints always stop the process at the specified point until they are removed.
        
Conditional breakpoints contain specified scenarios which trigger the breakpoint. You can only use one conditional breakpoint per line. Conditional breakpoints are formatted as follows:
        
- The left value specifies a variable or a keyword.
- The operator specifies one of the following values: **=, <, >, <=, >=, !=**. Place a space before and after the operator.
- The right value specifies a variable, a keyword, a constant, or a literal.

Supported keywords: **R0, R1, R2, R3, R4, R5, R6, R7, R8, R9, R10, R11, R12, R13, R14, R15, CMAR, CSA, CURR, CWA, CWK, DSA, ITBE, LCL, MXR, MXS, OPFL, PREV, TAL, TGT, TIOA, TWA**

Supported constants: **LOW-VALUES, HIGH-VALUES, ZEROES, SPACES**

Supported literals:
- **C** specifies a string of any type of character. Example: C'ASDF'
- **P** specifies a positive or negative number. Example: P'-1548'
- **H** specifies a hexadecimal value which is four characters long. Example: H'A2DF'
- **F** specifies a hexadecimal value which is eight characters long. Example: F'A86FE567'
- **X** specifies a hexadecimal value of any length. Example: X'A0DF27'
            
### Data Breakpoints

Data breakpoints are attached to variables and cause the program to stop when the value of the attached variable changes. To set a data breakpoint, right-click a variable in the Variable Tree View and select **Break on Value Change**.

When you set a data breakpoint in Debugger for Mainframe, a variable-change breakpoint is set on the same variable on the server side. When you set a variable-change breakpoint in InterTest, a data breakpoint is set in Debugger for Mainframe when you load the program listing. 

- **Note:** Due to a known issue, data breakpoints that are set as variable-change breakpoints on InterTest do not show up in the Variables Tree View.

Data breakpoints are valid for one debug session only and must be reset after each use.

### Paragraph Breakpoints

Set the `launch.json` parameter **paragraphBreakpoints** to "true" to trigger a breakpoint at the beginning of every new paragraph in COBOL code, and every label in Assembler code. For more information, see [Add Configuration](#add-configuration).

You can also turn this feature on and off in the debugger console by submitting the commands `/AT LABEL` and `/LABEL OFF`.

### Logpoints

![](https://raw.githubusercontent.com/BroadcomMFD/debugger-for-mainframe/master/LogPoints.gif)

Logpoints mark a particular part of the code, however unlike breakpoints, they do not break or stop the program. Logpoints can  highlight an issue within a program while it is running, without causing the program to terminate.

Logpoints can contain text and variables from the code. Enclose the variables in curly brackets without any spaces, as follows: 

    ‘text {variableName} text’

The text in the logpoint can be used for detailed observations about the behaviour of the code. The message output from the logpoints is displayed in the debug console.

Logpoints are especially useful as:
- They allow analysis of how the program behaves after this point, and whether the identified issue affects the program and how.
- They allow live analysis while users continue to use functionaility, minimizing costly downtime.
- Logpoints can be added on an ad-hoc basis, with no need to pre-plan, so debugging can be done flexibly according to resource availability.
- Logpoint notes are separate from source code, and require minimal clean-up after debugging is completed. 

### Execution Counts

Debugger for Mainframe includes an execution count feature which states how many times each line of code is run between each breakpoint or abend in your debugging session. The execution count feature is only supported for Batch programs.

The execution count feature is disabled by default. It can be enabled in `launch.json` before you begin your debug session. For more information, see [Add Configuration](#add-configuration).

To enable or disable the execution count feature during your debug session, use the debug console commands `/counts on` and `/counts off`. After you submit the `/counts on` command, perform an action which progresses the debugging session to display the execution count.

The execution count variable is stored on the mainframe and is only reset when you perform an action which progresses the debugging session. It is not reset by the `/counts off` command or by closing Visual Studio Code.

## Variables

Use the VS Code variables tree view to view and edit the value of variables during your debugging session. As well as the regular VS Code functionality, Debugger for Mainframe also enables you to edit the hexadecimal value of a variable, and view variables inline.

### Variables Tree View

Variables in the tree view are sorted under the following categories:

* **Global**
  * Shows the last known value of all variables in the program.
* **Local**
  * (COBOL only) Shows the value of all variables on the currently selected line when the debugging session is paused.
* **CICS Defined Variables**
  * (COBOL only, CICS only) Shows the value of all variables defined in the CICS region where your debug session is running.
* **General Purpose Registers**
  * (Assembler only) Shows the content of all 16 General Purpose Registers.

#### Filter Variables

To find a variable in the variables tree view, click the **Filter** icon and type a search string in the text box at the top of the interface. The search returns all variables which contain the provided string. To clear an active filter, click the **Filter** icon.

The filter searches variable names under the **Global** and **CICS Defined Variables** headings only.

#### Edit the Hexadecimal Value of a Variable

To edit the hexadecimal value of a variable, do the following:  

1. Locate the variable in the variables view.
2. Right-click the variable and select **Set Value**.
3. Enter the hexadecimal value in the format <b>0x<i>value</i></b> and press enter.

You can hover over a variable with an invalid format in the edit window to view the hexadecimal value.

### Inline Variables View

Enable inline variables view to see the values of all variables on the highlighted line of code inline.

![](https://raw.githubusercontent.com/BroadcomMFD/debugger-for-mainframe/master/inlinevars.png)

To enable and disable inline variables view, use the following commands in the debug console:

* `/inline on`, `/inline off`
    - Enables and disables inline variables view.

## Call Trace and Statement Trace

Debugger for Mainframe includes tracing features which send information about statements and programs to the left-hand sidebar of your IDE. There are two tracing features, call trace (for CICS programs only) and statement trace (for CICS and Batch programs).

Call trace displays information about calls from program to program, displaying the logical flow between listings. You can view this information in the **Call Stack View** in the left-hand sidebar of your IDE.

Statement trace displays information about the statements that you pass during your debug session. You can view this information in the **Statement Trace View** in the left-hand sidebar of your IDE. 

You can click on an entry in the Call Stack or Statement Trace View to move the cursor to the relevant line of the source code file.

Records displayed in both views contain the following information:

* Name of the program
* Number of the last statement processed
* Text of the last statement processed

The call trace and statement trace features are enabled by default. They can be disabled in `launch.json` before you begin your debug session. For more information, see [Add Configuration](#add-configuration).

To disable or enable the tracing features during your debug session, use the following commands in the debug console:

* `/trace on`, `/trace off`
    - Enables and disables statement trace.
* `/calltrace on`, `/calltrace off`
    - Enables and disables call trace.

## Integrate with Zowe Explorer

You can integrate Debugger for Mainframe with [Zowe Explorer](https://docs.zowe.org/stable/getting-started/user-roadmap-zowe-explorer) and set up a Zowe profile to enable the Single Sign-On feature of Zowe API ML, access mainframe data sets, and track the status of JCL jobs in connection with your debugging sessions. You can also use Zowe Explorer to submit JCL or to edit your converted JCL before running batch debugging sessions. 

To enable job status messages, the **zoweProfileName** parameter must be specified in your configuration file (see the **[Add Configuration](#add-configuration)** section).

Zowe Explorer is available as part of the Code4z package.

<a href="https://www.openmainframeproject.org/all-projects/zowe/conformance"><img alt="This extension is Zowe v3 conformant" src="https://artwork.openmainframeproject.org/other/zowe-conformant/zowev3/explorer-vs-code/color/zowe-conformant-zowev3-explorer-vs-code-color.png" width=20% height=20% /></a>

## Troubleshooting

### Known Limitations

- Conditional breakpoints validator does not work on VS Code version 1.74.
- We currently only support one user on an active debugging session at one time. If multiple users join the same session from the Batch Link Queue, it might affect performance and cause other unwanted issues.
- COBOL table variables defined with the OCCURS X TIMES keyword can only be updated from the variables view, not from the editor.

### Enable Troubleshooting Log
To generate a troubleshooting log, add the following parameters to your `launch.json` file:

- **"logLevel"**:
    - Specifies the amount of information reported to the log. Specify one of the following values:
        - "SEVERE"
        - "WARNING"
        - "INFO"
        - "CONFIG"
        - "FINE"
        - "FINER"
        - "FINEST"  
    These values are ordered from returning the least information ("SEVERE"; errors only) to the most information ("FINEST"; all details).
- **"logOutput"**:
    - Specifies where the log is displayed. Specify one of the following values:
        - "console"
            - Displays the troubleshooting log on the IDE console.
        - "file"
            - Stores the troubleshooting log in a file in the `/.c4z` folder in your workspace.
           
## Report Issues

How can we improve Debugger for Mainframe? [Let us know on our Git repository](https://github.com/BroadcomMFD/debugger-for-mainframe/issues).

## Technical Assistance and Support for Debugger for Mainframe

The Debugger for Mainframe extension is made available to customers on the Visual Studio Code Marketplace in accordance with the terms and conditions contained in the provided End-User License Agreement (EULA).

If you are on active support for InterTest, you get technical assistance and support in accordance with the terms, guidelines, details, and parameters that are located within the Broadcom [Working with Support](https://support.broadcom.com/external/content/release-announcements/CA-Support-Policies/6933) guide.

This support generally includes:

* Telephone and online access to technical support
* Ability to submit new incidents 24x7x365
* 24x7x365 continuous support for Severity 1 incidents
* 24x7x365 access to Broadcom Support
* Interactive remote diagnostic support
* Technical support cases must be submitted to Broadcom in accordance with guidance provided in “Working with Support”.

Note: To receive technical assistance and support, you must remain compliant with “Working with Support”, be current on all applicable licensing and maintenance requirements, and maintain an environment in which all computer hardware, operating systems, and third party software associated with the affected Broadcom software are on the releases and version levels from the manufacturer that Broadcom designates as compatible with the software. Changes you elect to make to your operating environment could detrimentally affect the performance of Broadcom software and Broadcom shall not be responsible for these effects or any resulting degradation in performance of the Broadcom software. Severity 1 cases must be opened via telephone and elevations of lower severity incidents to Severity 1 status must be requested via telephone.

## Privacy Notice
The extensions for Visual Studio Code developed by Broadcom Inc., including its corporate affiliates and subsidiaries, ("Broadcom") are provided free of charge, but in order to better understand and meet its users’ needs, Broadcom may collect, use, analyze and retain anonymous users’ metadata and interaction data, (collectively, “Usage Data”) and aggregate such Usage Data with similar Usage Data of other Broadcom customers. Please find more detailed information in License and Service Terms & Repository.

This data collection uses built-in Microsoft VS Code Telemetry, which can be disabled, at your sole discretion, if you do not want to send Usage Data.

The current release of Debugger for Mainframe collects anonymous data for the following events:
- Activation of this VS Code extension
- Conversion of batch JCL for debugging
- Batch scheduling table operations
- Batch link queue operations
- Fetch extended sources
- DAP requests and responses
- Setting variables
- Errors

Each such event is logged with the following information:
- Event time
- Operating system and version
- Country or region
- Anonymous user and session ID
- Version numbers of Microsoft VS Code and Debugger for Mainframe

