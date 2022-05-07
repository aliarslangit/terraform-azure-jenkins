# terraform-azure-jenkins

`terraform-azure-jenkins` is a sample example of how we can easily automate infrastructure provisioning using Jenkins and terraform.
We will be building Jenkins pipeline using groovy script and we will run terraform scripts on agent to provision our infrastructure on Azure.

# Initial setup

## Jenkins

Jenkins is an open source automation server which enables developers around the world to reliably build, test, and deploy their software.

### Installation on Windows:
1. Install Java Development Kit (JDK)
Download JDK 8 and choose windows 32-bit or 64-bit according to your system configuration. Click on "accept the license agreement." 
2. Set the Path for the Environmental Variable for JDK
Go to System Properties. Under the "Advanced" tab, select "Environment Variables." 
Under system variables, select "new." Then copy the path of the JDK folder and paste it in the corresponding value field. Similarly, do this for JRE.
Under system variables, set up a bin folder for JDK in PATH variables. 
Go to command prompt and type the following to check if Java has been successfully installed: `C:\Users\Simplilearn>java -version`
3. Download and Install Jenkins `https://www.jenkins.io/download/thank-you-downloading-windows-installer-stable`
After the file is downloaded, unzip it. Click on the folder and install it. Select "finish" once done.
4. Run Jenkins on Localhost 8080
Once Jenkins is installed, explore it. Open the web browser and type "localhost:8080". 
Enter the credentials and log in. If you install Jenkins for the first time, the dashboard will ask you to install the recommended plugins. Install all the recommended plugins.

### Installation on Ubuntu:




