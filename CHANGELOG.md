# Debugger for Mainframe Changelog

All notable changes to the Debugger for Mainframe extension are documented in this file.

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
