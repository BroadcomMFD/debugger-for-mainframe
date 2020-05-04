<div id="header" align="center">

[![GitHub issues](https://img.shields.io/github/issues-raw/BroadcomMFD/debugger-for-mainframe?style=flat-square)](https://github.com/BroadcomMFD/debugger-for-mainframe/issues)
[![slack](https://img.shields.io/badge/chat-on%20Slack-blue?style=flat-square)](https://join.slack.com/t/che4z/shared_invite/enQtNzk0MzA4NDMzOTIwLWIzMjEwMjJlOGMxNmMyNzQ1NWZlMzkxNmQ3M2VkYWNjMmE0MGQ0MjIyZmY3MTdhZThkZDg3NGNhY2FmZTEwNzQ)
</div>

# Debugger for Mainframe

Debugger for Mainframe provides the debugging interface to [CA InterTest™ for CICS](https://www.broadcom.com/products/mainframe/devops-app-development/testing-quality/intertest-cics). This extension provides a modern debugging experience for COBOL applications running in a CICS region.

> How can we improve Debugger for Mainframe? [Let us know on our Git repository](https://github.com/BroadcomMFD/debugger-for-mainframe/issues)

Debugger for Mainframe is also part of [Code4z](https://marketplace.visualstudio.com/items?itemName=broadcomMFD.code4z-extension-pack), an all-round package that offers a modern experience for mainframe application developers, including [HLASM Language Support](https://marketplace.visualstudio.com/items?itemName=broadcomMFD.hlasm-language-support), [COBOL Language Support](https://marketplace.visualstudio.com/items?itemName=broadcomMFD.cobol-language-support), [Explorer for Endevor](https://marketplace.visualstudio.com/items?itemName=broadcomMFD.explorer-for-endevor) and [Zowe Explorer](https://marketplace.visualstudio.com/items?itemName=Zowe.vscode-extension-for-zowe) extensions.

## Upgrading to Version 1.1

When upgrading to this version of Debugger for Mainframe from an older version, ensure that you change the "type" parameter in your launch.json files from "cbl" to "intertest-cics".

## Getting Started

### Prerequisites

#### Server
- CA InterTest for CICS, incremental release 11.0.07 or higher.
- CICS region
- [Testing Tools Server](http://techdocs.broadcom.com/content/broadcom/techdocs/us/en/ca-mainframe-software/devops/ca-intertest-and-ca-symdump/11-0/installing/install-testing-tools-server.html)

#### Client
- Java version 8.0 or higher.

### Set up Secure Connection to InterTest Server

#### Prerequisites
- InterTest server running and configured to support secure communication.
- Security certificate for the InterTest server.

**Follow these steps**

1. Download the Server Certificate to your local machine

2. Import the certificate to the trust store of the JRE under which the Debugger for Mainframe DA client is running. Use either Command Line, or a UI Tool:

    ##### Command Line:
    
      Enter the following command:
      
         `sudo keytool -import -alias hostname -file hostname.cer -storetype JKS -keystore cacerts`

    ##### UI:
      1. In your preferred UI, locate and open **cacerts** with the password **changeit**.
      2. Import the certificate to cacerts.
      3. Name the certificate with an appropriate alias to ensure it is easily identified.
      4. Save your changes.
        
    ##### Linux Subsystem
      1. Verify that Java is installed by running the command `java -version`.
      2. Locate your subsystem's java installation.
      3. Go to **/lib/security** to find **cacerts**.
      4. Run the following command to import the certificate:
        
            `sudo keytool -import -alias hostname -file hostname.cer -storetype JKS -keystore cacerts`
        
3. Check that your launch.json includes **"interTestSecure": true**:

4. Run a test debug session.

You have activated secure data connection to InterTest for Debugger for Mainframe

**Troubleshooting:**
- Make sure that you imported the certificate to the correct JRE's trust store.
- Make sure that the certificate you imported is the correct certificate.

## Using Debugger for Mainframe

To debug programs with Debugger for Mainframe you open the workspace in your IDE and configure your connection to CA InterTest using the file launch.json. Debugged files are temporarily saved in the workspace within the ``` .extrcs ``` folder.

![](https://raw.githubusercontent.com/BroadcomMFD/debugger-for-mainframe/master/Setup%20and%20config%20Edited.gif)

**Follow these steps:**

1. Select the Explorer icon in your IDE and open a folder.
    - The workspace opens.

2. Select the Bug icon to open **Debug view**.

3. In the sidebar, click **create a launch.json file**.

4. Click **Add Configuration**.

5. Select **Debugger for Mainframe: CA INTERTEST™ FOR CICS**.

6. Populate the following fields:

    - **"type":**
        - Set this to "intertest-cics".
    - **"name":**
        - Specifies the name of the debugging session.
    - **"programName"**:
        - Specifies the name of the program that you want to debug. This field cannot exceed eight characters.
    - **"interTestHost"**:
        - Specifies the host address of your Testing Tools Server instance.
    - **"interTestPort"**:
        - Specifies the port number of your Testing Tools Server instance.
    - **"interTestUserName"**:
        - Specifies your mainframe username.
    - **"interTestSecure"**:
        - Secure server connections are not yet supported, so the only allowed value is 'false'.
    - **"cicsApplId"**:
        - Specifies the CICS Application ID (cicsApplID) of your CICS region.

7. To open the interface, press F1.

8. Type **Fetch Extended Sources**.
    - This auto-populates as you type, so the required source might appear before you type the full source name.

9. Select the required source, for example:
    - ``` CA INTERTEST™ FOR CICS ```

10. Enter your password.
    - The expanded source is displayed.

11. Set breakpoints as required. You can set breakpoints before you start the debugging process or as the process is running.
    
    For more information, see **[Conditional and Unconditional Breakpoints](#conditional-and-unconditional-breakpoints)**

12. Set logpoints as required. Logpoints can be used to highlight an issue within the program while it is running, however logpoints do not cause the program to terminate.

    For more information, see **[Logpoints](#logpoints)**
                
13. Click the play icon in the top left of the interface to start the debugging process.
    - Once the CICS transaction is running, the debugging session stops at each breakpoint, or if an abend occurs.

![](https://raw.githubusercontent.com/BroadcomMFD/debugger-for-mainframe/master/Breakpoints.gif)

You have successfully initiated a debugging session.

### Conditional and Unconditional Breakpoints

Breakpoints can be unconditional or conditional. Unconditional breakpoints always stop the process at the specified point until removed.
        
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
