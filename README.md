# Debugger for Mainframe

Debugger for Mainframe provides the debugging interface to CA INTERTEST™ for CICS. This extension provides a modern debugging experience for COBOL applications running in a CICS region.

## Getting Started

### Prerequisites

#### Server
- CICS region
- [Testing Tools Server](http://techdocs.broadcom.com/content/broadcom/techdocs/us/en/ca-mainframe-software/devops/ca-intertest-and-ca-symdump/11-0/installing/install-testing-tools-server.html)

#### Client
- Java version 8.0 or higher.

## Using Debugger for Mainframe

To debug programs with Debugger for Mainframe you open the workspace in your IDE. Debugged files are temporarily saved in the workspace within the ``` .extrcs ``` folder.

![](https://raw.githubusercontent.com/zimlu02/Debugger-for-Mainframe-about/master/Setup%20and%20config%20blurred.gif)

**Follow these steps:**

1. Select the Explorer icon in your IDE and open a folder.
    - The workspace opens.

2. Select the Bug icon to open **Debug view**

3. Click the Cog icon to add a configuration.

4. Select **Debugger for Mainframe: CA INTERTEST™ FOR CICS**.

5. Click **Add Configuration**.

6. Click **COBOL InterTest CICS Debug**.

7. Populate the following fields:

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
        - Specifies whether the server is secure.
            - Values: true, false
                - Default: true
    - **"cicsApplId"**:
        - Specifies the CICS Application ID (cicsApplID) of your CICS region.

8. To open the interface, click F1.

9. Type **fetch extended source**.
    - This auto-populates as you type, so the required source might appear before you type the full source name.
   
10. Select the required source, for example:
    - ``` Fetch Extended Source ```

11. Enter your password.
    - The expanded source is displayed.

12. Set breakpoints as required.
    - You can set breakpoints before you start the debugging process or as the process is running.

13. Click the play icon in the top left of the interface to start the debugging process.
    - Once the CICS transaction is running, the debugging session stops at each breakpoint, or if an abend occurs.
    
![](https://raw.githubusercontent.com/zimlu02/Debugger-for-Mainframe-about/master/Breakpoints.gif)
    
You have successfully initiated a debugging session.
