# aas-find-in-workflow

## Introduction

This Java-based command-line tool provides a simple method to scan through the workflows in your [Adobe Acrobat Sign](https://www.adobe.com/sign.html) account to identify those workflows that reference a specified email address.
It searches in the following locations defined in each workflow:

+ The Email specified in any of the recipients in the Recipients section
+ The Label specified in any of the recipients in the Recipients section
+ The CC information specified in the Agreement Info section of the workflow

Being able to quickly identify the presence of a specified email address across all of the workflows in an account allows an administrator to ensure that workflows are updated when a user changes role or leaves the organisation.

## Set-Up Instructions

+ Download the latest release of the `aas-find-in-workflow-<version>.jar` from the [Releases](https://github.com/esign-org-uk/aas-find-in-workflow/releases) page
+ [Download Java 1.8](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html), if you don't already have it installed on your machine
+ [IP Addresses to add to your allow list](https://helpx.adobe.com/sign/system-requirements.html#IPs), if needed

As the tool makes use of the [Adobe Sign REST API](https://secure.adobesign.com/public/docs/restapi/v6), it is necessary to [provide an integration key](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html) that permits the tool to access your account.

To do this, follow the steps below:

1. Log in to your Acrobat Sign account (as an Administrator)
2. Click **Group** or **Account**, whichever you see at the top
3. Type "Access tokens" in the search field on the left side of the screen
4. Press the "+" icon on the right side
5. Create a key with the scope needed (only `workflow_read` is required)
6. Double-click the key you just created and copy the FULL text (it goes off screen to the right so make sure you get it all)
7. Store the `Integration Key` for later use

## Usage

+ Open a command prompt in the directory where you have the JAR file downloaded
+ Execute the following command, replacing:
  + `<version>` with the appropriate value for the release, such as `1.0.0`
  + `<integrationKey>` with your saved value from above
  + `<emailAddress>` with the email address that you wish to search for in your workflows

```sh
java -jar aas-find-in-workflow-<version>.jar <integrationKey> <emailAddress>
```

Assuming you have specified the two required parameters correctly, then you should see output similar to that below:

![Sample Output](/images/example-usage.png)

## Options

If you add a `--csv` parameter at the end of the other parameters, the output is formatted as [CSV](https://en.wikipedia.org/wiki/Comma-separated_values).
This makes it easier to redirect the output into a file that can then be opened as a basic spreadsheet.

![Sample CSV Output](/images/example-csv-usage.png)

## Notes

The tool iterates through all of the active workflows in your account.
If you have a large number of workflows present this will take some time, so a "." is written to the screen immediately prior to accessing each workflow object, so that you can observe progress.

## Building

If you would like to build a release package locally, you should have the following software installed:

+ OS: Linux, macOS, Windows
+ Java JDK: version 1.8 or above
+ Apache Maven

The tool makes use of the [Adobe Sign Java SDK](https://opensource.adobe.com/acrobat-sign/sdks/java.html), so you will first need to clone the [acrobat-sign repository](https://github.com/adobe/acrobat-sign) and install the package into your Maven repository by issuing the following commands:

```sh
cd <location-of-cloned-acrobat-sign-repo>/sdks/AcrobatSign_JAVA_SDK
mvn clean install
```

Once this has completed, clone this repository and build the package by issuing:

```sh
cd <location-of-cloned-tool-repo>
mvn clean package
```
