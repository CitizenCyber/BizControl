# BizControl (Made for MSMEs)
Modular, Enterprise-like, Self-hosted, full-suite business management FOSS for MSMEs. Made specifically for India but can be used by businesses in other countries with some changes made to the source code.
(Work in progress)

# Note - This software is made for Windows OS only.

BizControl can be used for the following -

- Invoice/Billing

- Customer Management

- Supplier Management

- Inventory Management

- Business Analytics

- Business Reports

- Employee Management (Attendance, Salary & Leave Management)

- Payments (does not have payment gateways integrated. Relies on the cashier & POS machines for confirming payments)

- Cloud Backup and Restore

## Prerequisites for using BizControl

- BizControl is built on principle of self-hosting. *MySQL* is needed for making, storing, accessing and encrypting the database which is used for storing the business data. MySQL installer can be installed by going [here](https://dev.mysql.com/downloads/installer/). Using the installer to install MySQL can be seen [here](https://www.youtube.com/watch?v=u96rVINbAUI). This project uses the already generated local instance with the username as *root* and password as *Admin123*. Make sure to give *Admin123* as the password in the installation process of MySQL. Whenever you want to use BizControl, always open the local instance in MySQL Workbench and minimize it, which keeps it running in the background.

- Make a *.bizcontrol* directory in C:\Users\<Username>\ and paste your `credentials.json` file there along with the UPI QR (rename the UPI QR to *qr_code.jpeg*). The `credentials.json` file is required for the 'Cloud Backup' feature to work and the UPI QR that you place in *.bizcontrol* will be displayed to the customers for payment. Here is how you can get your `credentials.json` file -->

**Step 1**: Create a Google Cloud Project -->

1. Go [here](https://console.cloud.google.com).
2. Click the project drop-down at the top of the page and select New Project.
3. Name the project BizControl and click Create.
4. Once created, make sure the BizControl project is selected in the top-left drop-down.

**Step 2**: Enable the Google Drive API -->

1. In the left-hand menu, go to APIs & Services > Library.
2. Search for Google Drive API.
3. Click on it and hit the blue Enable button.

**Step 3**: Configure the OAuth Consent Screen -->

1. In the left menu, go to APIs & Services > OAuth consent screen.
2. Select External and click Create.
3. Fill in the required fields:

- App Name: BizControl
- User Support Email: Select your email.
- Developer Contact Information: Enter your email again at the bottom.

4. Click Save and Continue.
5. Scopes: Add the necessary permissions. For creating and managing backups, you would add the .../auth/drive.file scope. Click Save and Continue. You can skip manually adding scopes here and just click Save and Continue (the Java source code automatically requests the drive.file scope at runtime).
6. Scroll down the page until you see the section labeled Test users in APIs & Services > OAuth consent screen.

- Click the + ADD USERS button.
- A side panel will slide out. Type in the Google email addresses (Gmail or Google Workspace emails) of the people who need to test the app. In this case, it's just you.
- Note: You can add up to 100 test users.
- Click SAVE.

**Step 4**: Generate the Credentials -->

1. In the left menu, go to APIs & Services > Credentials.
2. Click + CREATE CREDENTIALS at the very top and select OAuth client ID.
3. Under Application type, you must select Desktop app (This is required for BizControl application to intercept the login redirect).
4. Name it "BizControl" and click Create.

**Step 5**: Download and Place the File -->

1. A pop-up will appear with your new Client ID and Client Secret.
2. Click the Download JSON button.
3. Rename the downloaded file to exactly `credentials.json`.

## How to run BizControl using the source code?
Since it is a *Maven* project and `pom.xml` is configured with the `javafx-maven-plugin` and points to the Main class, you can launch the app directly using the Maven wrapper. Launch Command Prompt in the root directory of BizControl or simply open Command Prompt and navigate to the root directory of BizControl and execute the following command -->

`mvnw.cmd clean javafx:run`

## How to build .exe from the source code

To package the source code into standalone native executable installer (.exe) for Windows OS, the standard approach for Java 14+ is to build a "Fat JAR" (containing all the dependencies) and then use the JDK's built-in `jpackage` tool along with `WiX Toolset`. Go [here](https://github.com/wixtoolset/wix/releases) to download `wix-cli-x64.msi` which is the `WiX Toolset v7.0.0`. Run `wix-cli-x64.msi` to install `WiX Toolset v7.0.0`. Then launch Command Prompt and execute the following commands one by one -->

`wix extension add -g WixToolset.UI.wixext`

`wix extension add -g WixToolset.Util.wixext`

`wix eula accept wix7`

Then launch Command Prompt in the root directory of BizControl or simply open Command Prompt and navigate to the root directory of BizControl and execute the following commands one by one -->

`mvnw.cmd clean package`

(This will generate `BizControl-1.0-SNAPSHOT-standalone.jar` in the *target* directory)

`jpackage --type exe --name BizControl --input target --main-jar BizControl-1.0-SNAPSHOT-standalone.jar --main-class com.bizcontrol.Launcher --icon icon.ico --win-shortcut --win-menu --win-dir-chooser`

(This is a long, single command which generates a `BizControl-1.0.exe` installer in your project root, which I have provided in the 'releases' section of this repository)
