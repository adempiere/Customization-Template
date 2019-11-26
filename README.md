# ADempiere Customization Template

This project is a template for ADempiere customization projects for swing and 
zk. It allows for quasi-real time debugging in both interfaces and will create 
the customization.jar and zkcustomization.jar.  You can extend the build 
process to create packages and patch files as required.

You will need to have two projects in your development environment: one for the 
ADempiere project and one for your customized code that will depend on the 
ADempiere project.

## Create the ADempiere Project

If you haven't already done so, follow the ADempiere Version Control 
process to checkout a branch of the ADempiere project.
    
See the documentation (https://adempiere.gitbook.io/docs/) and follow 
the instructions in the Developer Guide section "Development Environments" 
and, if you are modifying the zk interface, 
"Creating WebUI Workspace using Eclipse Webtool".
    
Build (using utils_dev/build.xml), install, setup the software (to create 
the .properties file) and import the database seed.
    
Modify the launch configurations as required and test that you can run the
client and zk interfaces. 

You now have the main ADempiere project created. Changes to this project should
be made as part of the Software Development Procedure to fix bugs, add features
and generate common code that will be shared by all ADempiere implementations.

## Create the Customization Project

Fork the customization template project on github from here: 
https://github.com/adempiere/Customization-Template

Add the forked code as a new project to your workspace that contains the 
ADempiere project you created above.  In the Project Properties, add the main ADempiere
project as a dependency.

## Customization of the Swing interface and Core classes

For Swing, its pretty straight forward.  Copy the source code you wish to 
customize from the main project, keeping the source directory structure the
same.  You can modify the code and then run the customization project to see the
effects.  Many changes made while the code is running will be hot-swapped immediately.


## Customization of the ZK interface

To help reduce the delays in starting tomcat every time you make a change, there
is a trick you can use based on an article on Beyond Java 
(https://www.beyondjava.net/eliminate-cumbersome-tomcat-deployment-waits).  Using a 
file synchronization tool, you can copy updated classes to the tomcat server without
having to reload the entire server.  You will lose the state of the application but you will 
not have to wait for 30 seconds or more after every change for the server to restart.

Download and install the FileSync Eclipse plugin.  

In Eclipse, open the Window->Preferences dialog and find the preferences for
Run/Debug->String Substitution.  Add a new variable and path as follows:

    Name: ADEMPIERE_PROJECT_LOC
    Value: <Add the system path to the ADempiere project>


Delete all the contents of the zkwebui folder in the template except for the 
build_custom.xml file.  The template contains one source file in zkwebui/WEB-INF/src,
Login.java, as an example. This simply changes the heading on the login window to 
"My Customization Works!".  If you wish, you can leave this file in place.

Copy the zkwebui directory from the ADempire project to the template. Be 
careful not to overwrite the build_custom.xml file in the template. This will 
provide the same deployment structure as the main ADempiere project. (This 
step is necessary and could be automated but risks overwriting your 
customization so it has been left as a manual process.)

In the Project Properties for the template, verify that the Deployment Assembly for
the template project matches the Deployment Assembly in the ADempiere project. 

Run the External Launcher MyCustomizationProject InitializeZKCustomizations - this will
copy all the classes needed from the ADempiere project to the template. 
Depending on the version of ADempiere, you may need to modify the associated 
build.xml file. (If you do this by hand from the build file, don't forget to
refresh the project files.)

Create a server and add the template to the server.  Note the location where the 
project will be deployed.

In the project explorer, open the server folder and modify the context.xml file. Change
the "context" tag to read

    <context reloadable="false">
    
This will prevent Tomcat from reloading the context/application every time there is a change. 

Open the Project Properties and update the File Synchronization properties.  Add the folder
"MyCustomizationProject/zkwebui" as the source and set the target to the deployment location
of the server. For example:

    C:\dev\eclipse\custom-template-folder\.metadata\.plugins\org.eclipse.wst.server.core\tmp1\wtpwebapps\MyCustomizationProject

Publish the project to the server then, Open the server configuration and set the publishing 
setting to "Never publish". 

## Testing with the template

With the template setup you should be good to go. You may need to update the build 
files to adjust to ADempiere versions. If you customize other directories 
than build and client, also copy the build.xml files from the ADempiere 
project and modify them to add the customized classes to the jar files. 
Compare the build.xml from the base directory in both the template and the 
ADempiere project to see how and what to change.

If you launch the server, you should see the changes in the zk files. When you make
a change, the file synchronization should publish this immediately and you will see 
the effects when you refresh the web page.  You may lose the context and have to 
log-in again.

The launcher for the client will run the client as per the main project. Here,
most changes you make will be hot-swapped into the application which is really
nice for development.

## Exporting the Customization Jars

When your customization is ready, there is a launcher to build the 
customization jars. The two files customization.jar and zkcustomization.jar 
will be added to the lib directory. You can add these to the lib directory 
of the ADempiere installation and execute the setup (RUN_Setup or 
RUN_SilentSetup) to see the changes.



