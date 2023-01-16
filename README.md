<div id="header" align="center">

[![GitHub issues](https://img.shields.io/github/issues-raw/BroadcomMFD/debugger-for-mainframe?style=flat-square)](https://github.com/BroadcomMFD/debugger-for-mainframe/issues)
[![slack](https://img.shields.io/badge/chat-on%20Slack-blue?style=flat-square)](https://communityinviter.com/apps/che4z/code4z)
</div>

# Debugger for Mainframe

Debugger for Mainframe provides a debugging interface for [InterTest™ for CICS](https://www.broadcom.com/products/mainframe/devops-app-development/testing-quality/intertest-cics) and [InterTest™ Batch](https://www.broadcom.com/products/mainframe/testing-and-quality/intertest-batch). This extension provides a modern debugging experience for CICS and Batch programs written in COBOL and High-Level Assembler Language (HLASM).

> How can we improve Debugger for Mainframe? [Let us know on our Git repository](https://github.com/BroadcomMFD/debugger-for-mainframe/issues)

Debugger for Mainframe is also part of [Code4z](https://marketplace.visualstudio.com/items?itemName=broadcomMFD.code4z-extension-pack), an all-round package that offers a modern experience for mainframe application developers, including extensions for language support, data editing, testing, and source code management.

## Compatibility

Debugger for Mainframe is supported on Visual Studio Code and Github Codespaces.

We recommend the use of [Zowe Explorer](https://marketplace.visualstudio.com/items?itemName=Zowe.vscode-extension-for-zowe) to access mainframe data sets in connection with your debugging sessions. Zowe Explorer is available as part of the Code4z package.

<a href="https://www.openmainframeproject.org/all-projects/zowe/conformance"><img alt="This extension is Zowe v2 conformant" src="https://artwork.openmainframeproject.org/other/zowe-conformant/zowev2/explorer/color/zowe-conformant-zowev2-explorer-color.png" width=20% height=20% /></a>

## Prerequisites

### Server
- InterTest for CICS and/or InterTest Batch, incremental release 11.0.07 or higher.
- Acquire and install PTFs LU08488, LU08046, LU06771, LU08177, and LU08307.
- [Testing Tools Server](http://techdocs.broadcom.com/content/broadcom/techdocs/us/en/ca-mainframe-software/devops/ca-intertest-and-ca-symdump/11-0/installing/install-testing-tools-server.html)
    - To use Debugger for Mainframe to debug CICS programs, ensure that you complete the tasks in the sections "Activation of the IP CICS Sockets" and "Set Up an IRC Connection" on the linked page when you configure your Testing Tools Server instance.

### Client
- Java version 8.0 or higher with the PATH variable correctly configured. For more information see the [Java documentation](https://www.java.com/en/download/help/path.xml).

### Secure Connection
- Follow the instructions in the **[Set Up a Secure Connection](#set-up-secure-connection)** section below.

### IDE

Debugger for Mainframe is supported on Visual Studio Code version 1.67.0 and above.

## Using Debugger for Mainframe

To debug CICS and Batch programs with Debugger for Mainframe you open the workspace in your IDE and configure your connection to InterTest using the file `launch.json`. 

Debugged files are temporarily saved in the workspace within the ```/.c4z/.extsrcs``` folder.

To debug Batch programs, you also convert the JCL of your program into a new file which is used for debugging.

![](https://raw.githubusercontent.com/BroadcomMFD/debugger-for-mainframe/master/Setup%20and%20config%20Edited.gif)

See [this video](https://www.youtube.com/watch?v=f6ZxwALSb_Y&ab_channel=Educate) for a step-by-step walkthrough of CICS debugging using Debugger for Mainframe.

### Getting Started

To start debugging programs in your IDE, you first create a `launch.json` file within your workspace.

**Follow these steps:**

1. Select the Explorer icon in your IDE and open a folder.
    - The workspace opens.

2. Select the Bug icon to open **Debug view**.

3. In the sidebar, click **create a launch.json file**.  
   A list of configurations displays. 
   
4. Select either **Debugger for Mainframe: INTERTEST™ FOR CICS** or **Debugger for Mainframe: INTERTEST™ BATCH**

5. Fill in the necessary fields as described in the **[Add Configuration](#add-configuration)** section below.

### Add Configuration

The `launch.json` file contains configurations for debugging different types of programs. The configurations supported by Debugger for Mainframe are **Debugger for Mainframe: INTERTEST™ FOR CICS** and **Debugger for Mainframe: INTERTEST™ BATCH**. 

When you create a `launch.json` file for the first time, a configuration is added. You can add more configurations by clicking the **Add configuration** button.

After you add your configuration, populate the following fields:

- **"type":**
    - Specify "intertest-cics" or "intertest-batch".
- **"name":**
    - Specifies the name of the debugging session.
- **"programName"**:
    - Specifies the name of the program that you want to debug using this configuration. To debug a program along with other programs called within it, specify all program names you want to debug in this field.
    - Specify an array with either one value or multiple values separated by commas.
- **"protsym"**:
    - (Batch only) Specify an array with any number of PROTSYM DSNs separated by commas. The newest PROTSYM which matches your executable is used for the debug session.
- **"interTestHost"**:
    - Specifies the host address of your Testing Tools Server instance.
- **"interTestPort"**:
    - Specifies the port number of your Testing Tools Server instance.
- **"interTestUserName"**:
    - Specifies your mainframe username.
- **"interTestSecure"**:
    - Specify "true" to use a secure connection to the InterTest server or "false" to use a non-secure connection.  
      Ensure that you complete the steps in the **[Set Up a Secure Connection](#set-up-secure-connection)** section if you want to use a secure connection.
- **"originalJCL"**:
    - (Batch only) Specifies the location of the JCL containing the program that you want to debug. Debugger for Mainframe converts this JCL so that it can be debugged.  
      This is a JSON element containing the following fields:
        - **"inDSN"**:
            - Specifies the DSN and member name of the JCL containing the program that you want to debug.
        - **"stepName"**:
            - Specifies the step name of the program that you want to debug.
        - **"procLibs"**:
            - (Optional) Specify an array with any number of DSNs containing procedure libraries. Specify this field if your JCL requires a procedure library to be converted.
- **"convertedJCL"**:
    - (Batch only) Specifies the DSN and member name where you want to store your converted JCL. Specify the full name of a partitioned data set and a member in the format DSN(MEMBER). Debugger for Mainframe creates or overwrites this member when you convert the JCL.
- **"cicsApplId"**:
    - (CICS only) Specifies the CICS Application ID (cicsApplID) of your CICS region.
- **"cicsUserId"**:
    - (CICS only, optional) Specify a CICS user logon ID to debug a CICS program or transaction as it executes for that specific ID. You cannot modify this value during an active debugging session.
- **"interTestCharset"**:
    - (Optional) Specifies the Testing Tools Server Charset for Listings. Specify this field only if your Testing Tools Server instance is configured to use a client code page other than UTF-8.
- **"paragraphBreakpoints"**:
    - (Optional) Specify "true" to have the debugging session stop automatically at each new paragraph or label. For more information, see [Paragraph Breakpoints](#paragraph-breakpoints).
- **"callTrace"**:
    - (CICS only, optional) Specify "false" to disable the call trace feature. The feature is enabled by default. For more information, see [Call Trace and Statement Trace](#call-trace-and-statement-trace).
- **"statementTrace"**:
    - (Optional) Specify "false" to disable the statement trace feature. The feature is enabled by default. For more information, see [Call Trace and Statement Trace](#call-trace-and-statement-trace).
- **"executionCounts"**:
    - (Batch only, optional) Specify "true" to enable the execution counts feature. The feature is disabled by default. For more information, see [Execution Counts](#execution-counts).

### Run a Debug Session

After you define your configuration in `launch.json`, you can run your debug session in the debugging interface.

**Follow these steps:**

1. Press F1 to open the interface.

2. (Batch only, optional) To convert your JCL, type **Batch: Convert JCL** and press enter. Complete this step when debugging a program for the first time, or if your program changed since the last debug session.

3. Type **Fetch Extended Sources**.
    - This auto-populates as you type, so the required source might appear before you type the full source name.

4. Select the required source, for example:
    - ``` INTERTEST™ FOR CICS ```

5. Enter your password.
    - The expanded source is displayed.

6. Set breakpoints as required. You can set breakpoints before you start the debugging process or as the process is running.
    
    - For more information, see **[Conditional and Unconditional Breakpoints](#conditional-and-unconditional-breakpoints)**

7. Set logpoints as required. Logpoints can be used to highlight an issue within the program while it is running, however logpoints do not cause the program to terminate.

    - For more information, see **[Logpoints](#logpoints)**
                
8. Click the play icon in the top left of the interface to start the debugging process.

9. (CICS only) Run your program on your CICS region.

![](https://raw.githubusercontent.com/BroadcomMFD/debugger-for-mainframe/master/Breakpoints.gif)

You have successfully initiated a debugging session.   
Once the session is running, the debugging session stops at each breakpoint, or if an abend occurs.  
You can use the **Continue** and **Step over** functions of the IDE debugging controller. The **Step into**, **Step out** and **Restart** functions are not currently supported.

### Call Trace and Statement Trace

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
       
### Conditional and Unconditional Breakpoints

Breakpoints can be unconditional or conditional. Unconditional breakpoints always stop the process at the specified point until they are removed.
        
Conditional breakpoints contain specified scenarios which trigger the breakpoint, and are formatted as follows:
        
    leftvalue operator rightvalue
            
**Important:** Include a space before and after the operator value.
    
Where:
- **leftvalue**
    - Specifies either a variable or a keyword.
        - A variable is any PICTURE value that is contained within the program.
        - A keyword is one of the following values: **R0, R1, R2, R3, R4, R5, R6, R7, R8, R9, R10, R11, R12, R13, R14, R15, CMAR, CSA, CURR, CWA, CWK, DSA, ITBE, LCL, MXR, MXS, OPFL, PREV, TAL, TGT, TIOA, TWA**
- **operator**
    - Specifies one of the following values: **=, <, >, <=, >=, !=**
- **rightvalue**        
    - Specifies a variable, a keyword, a constant, or a literal.
        - A constant is one of the following values: **LOW-VALUES, HIGH-VALUES, ZEROES, SPACES**
        - A literal is a string inside single quotation marks (' ') with a leading value of C, P, H, F or X, which specifies the string format:
            - **C** specifies a string of any type of character. Example: C'ASDF'
            - **P** specifies a positive or negative number. Example: P'-1548'
            - **H** specifies a hexadecimal value which is four characters long. Example: H'A2DF'
            - **F** specifies a hexadecimal value which is eight characters long. Example: F'A86FE567'
            - **X** specifies a hexadecimal value of any length. Example: X'A0DF27'
            
Correctly defined breakpoints are marked by a red dot.

Incorrectly defined breakpoints are marked by a grey dot or circle, with a summary error message indicating the cause of the error.

### Paragraph Breakpoints

Set the `launch.json` parameter **paragraphBreakpoints** to "true" to trigger a breakpoint at the beginning of every new paragraph in COBOL code, and every label in Assembler code. For more information, see [Add Configuration](#add-configuration).

You can also turn this feature on and off in the debugger console by submitting the commands `/AT LABEL` and `/LABEL OFF`.

### Logpoints
![](https://raw.githubusercontent.com/BroadcomMFD/debugger-for-mainframe/master/LogPoints.gif)

Logpoints mark a particular part of the code, however unlike breakpoints, they do not break or stop the program. Logpoints can  highlight an issue within a program while it is running, without causing the program to terminate.

Logpoints can contain text and variables from the code. Enclose the variables in curly brackets without any spaces, as follows: 

    ‘text {variableName} text’

**Important:** Ensure that the variableName is a single block of text with no spaces and is contained within curly brackets.

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

**Note:** The execution count appears in column 80 of the editor window. Ensure that your IDE window is wide enough to see it.

## Set up Secure Connection

You can use Debugger for Mainframe over a secure connection if your Testing Tools Server instance is configured for a secure connection. 

To use Debugger for Mainframe over a secure connection, obtain the server certificate for your Testing Tools Server instance and import it to the trust store of the JRE under which the Debugger for Mainframe DA client is running on your local machine. You can use command line or a UI tool.

### Import Server Certificate Using Command Line
    
Enter the following command: `sudo keytool -import -alias hostname -file hostname.cer -storetype JKS -keystore cacerts`
        
### Import Server Certificate on a Linux Subsystem

**Follow these steps:**
     
1. Verify that Java is installed by running the command `java -version`.
2. Locate your subsystem's java installation.
3. Go to **/lib/security** to find **cacerts**.
4. Run the following command to import the certificate: `sudo keytool -import -alias hostname -file hostname.cer -storetype JKS -keystore cacerts`
          
### Import Server Certificate using a UI

**Follow these steps:**

1. In your preferred UI, locate and open **cacerts**.
2. Import the certificate to cacerts.
3. Name the certificate with an appropriate alias to ensure it is easily identified.
4. Save your changes.
        
After you have imported your certificate, run a test debug session with **"interTestSecure": true** in your launch.json file. If the session fails, ensure that you imported the correct certificate to the correct JRE trust store and try again.

You have configured Debugger for Mainframe to use a secure connection to InterTest.

## Troubleshooting

### Known Issues

The following issues are known and will be fixed in future releases:

- Conditional breakpoints validator does not work on VS Code version 1.74.

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
