# Debugger for Mainframe Changelog

All notable changes to the Debugger for Mainframe extension are documented in this file.

## [1.9.0] 2023-12-01

#### Added
- Option to connect to InterTest via the API Mediation Layer
- Support for table variables
- Support for data breakpoints

#### Changed
- Launch.json parameters `interTestHost` and `interTestPort` changed to `host` and `port`
- Readme update

## [1.8.1] 2023-06-29

#### Changed
- Readme update
- IMS fields accept wildcard character '*'
- Fetch extended sources not allowed for 'attach' configurations

#### Fixed
- IMS transaction connection with wrong transaction name
- Attach session with automatic breakpoint on first line
- Expected breakpoint behavior on attach session

## [1.8.0] 2023-06-05

#### Added
- Batch Link support
- Batch attach debug configuration
- Support for the Suspend feature on Batch sessions
- Full support for the Step into and Step out functions
- DB2 stored procedure and IMS DC application support
- Basic batch demo walkthrough

#### Changed
- Readme update

## [1.7.0] 2023-01-13

#### Added
- High-Level Assembler Language support
- Full support for conditional breakpoints (validator currently not supported on VS Code version 1.74)
- Support for label breakpoints in Assembler code
- Getting Started with CICS Debugging walkthrough

#### Changed
- Variable enhancements
- Readme update

## [1.6.1] 2022-10-26

#### Changed
- Readme update

#### Fixed
- Issue with insufficient websocket buffer
- Compatibility with extensions which contribute languages

## [1.6.0] 2022-06-30

#### Added
- Execution counts feature

#### Changed
- Readme update

## [1.5.4] 2022-06-07

#### Fixed
- Issue when using multiple PROTSYMs
- Notify user when unsupported debug action is used

## [1.5.3] 2022-05-17

#### Changed
- Readme update

## [1.5.2] 2022-05-16

#### Changed
- Readme update

#### Fixed
- Support for Java 17
- Minor bugfixes

## [1.5.1] 2021-09-17

#### Fixed
- Breakpoints in COBOL programs now work in other environments.

## [1.5.0] 2021-04-08

#### Added
- Statement trace and call trace features

#### Changed
- Readme update

## [1.4.0] 2020-12-18

#### Added
- Paragraph breakpoints feature
- Support for multiple-user sessions in CICS

#### Changed
- Readme update

## [1.3.1] 2020-11-04

#### Changed
- Changelog update

## [1.3.0] 2020-10-22

#### Added
- Support for debugging nested programs within programs
- Support for the variable watch functionality

#### Changed
- Variables are now read by package instead of individually
- Readme update

## [1.2.0] 2020-08-20

#### Added
- Support for Batch debugging using CA InterTest Batch
- Conversion of JCL to be used for Batch debugging
- Support for non-UTF8 charsets
- Holistic logging 

#### Fixed
- Dropdown list to choose a configuration no longer displays the same configuration twice.

#### Changed
- Readme update

## [1.1.0] - 2020-04-20

#### Added
- Support for conditional breakpoints
- Support for logpoints
- Support for hitcounts

#### Fixed
- Fixed several minor issues

#### Changed
- The "type" parameter in launch.json now requires the value "intertest-cics". When upgrading to this version of Debugger for Mainframe from an older version, ensure that you update the "type" parameter in your launch.json files.
- Readme update

## [1.0.2] - 2020-02-14

- Support same name variables
- Fix debugging with lowercase username
- Fix lowercase username failed fetch

## [1.0.1] - 2020-01-24

- Minor readme and package.json changes

## [1.0.0] - 2019-12-10

- Initial release
