# ADempiere Customization Template

This project is a template for ADempiere customization projects for swing and 
zk. It allows for quasi-real time debugging in both interfaces and will create 
the customization.jar and zkcustomization.jar.  You can extend the build 
process to create packages and patch files as required.

You will need to have two projects in your development environment: one for the 
ADempiere project and one for your customized code that will depend on the 
ADempiere project.

Create the ADempiere Project

    If you haven't already done so, follow the ADempiere Version Control 
    process to checkout a branch of the ADempiere project.
    
    See the wiki and follow the instructions in pages "Create your ADempiere 
    development environment" and, if you are modifying the zk interface, 
    "Creating WebUI Workspace using Eclipse Webtool".
    
    Build (using utils_dev/build.xml), install, setup the software (to create 
    the .properties file) and import the database seed.
    
    Modify the launch configurations as required and test that you can run the
    client and zk interfaces. 

You now have the main ADempiere project created. Changes to this project should
be made as part of the Software Development Procedure to fix bugs, add features
and generate common code that will be shared by all ADempiere implementations.

Create the Customization Project

Fork the customization template project on github from here: 
https://github.com/mckayERP/template (Note: This will be moved to the 
official repository eventually.)

Add the forked code as a new project to your workspace that contains the 
ADempiere project you created above.

In the template, modify the utils_dev/buildCustomization.properties file to 
point to the correct directories.

Customization of the ZK interface

Delete all the contents of the zkwebui folder in the template except for the 
build_custom.xml file.

Copy the zkwebui directory from the ADempire project to the template. Be 
careful not to overwrite the build_custom.xml file in the template. This will 
provide the same deployment structure as the main ADempiere project. (This 
step is necessary and could be automated but risks overwriting your 
customization so it has been left as a manual process.)

Delete the zkwebui/WEB-INF/src source tree, leaving only the files you wish to 
customize. The template has only one file LoginPanel.java which changes the 
login header to "My Customization Works!".

Run the launcher MyCustomizationProject InitializeZKCustomizations - this will
copy all the classes needed from the adempiere project to the template. 
Depending on the version of ADempiere, you may need to modify the associated 
build.xml file. (If you do this by hand from the build file, don't forget to
refres the project files.)

If you are customizing zk, add a server and add the template to the server. In
the server launcher, the class path for the user entries needs to include the 
following:

    C:/dev/apache/tomcat-6.0/bin/bootstrap.jar
    adempiereTrunk/tools/lib/jnlp.jar
    adempiereTrunk/tools/lib/javaee.jar
    adempiereTrunk/tools/lib/jcommon-1.0.18.jar
    adempiereTrunk/tools/lib/jfreechart-1.0.15.jar
    adempiereTrunk/tools/lib/log4j.jar
    adempiereTrunk/JasperReportsTools/lib/jasperreports-5.1.0.jar
    adempiereTrunk/tools/lib/c3p0-0.9.1.2.jar
    adempiereTrunk/tools/lib/iText-2.1.7.jar
    adempiereTrunk/tools/lib/poi-3.9-20121203.jar
    adempiereTrunk/lib/postgresql.jar
    adempiereTrunk/tools/lib/commons-net-1.4.0.jar
    adempiereTrunk/tools/lib/commons-collections-3.1.jar 

These are the defaults in the launcher included with the project. You will 
need to point the classes in the launcher at the correct project name for 
the ADempiere project.

Testing with the template

With that the template should be good to go. You may need to update the build 
files to adjust to ADempiere versions. If you customize other directories 
than build and client, also copy the build.xml files from the ADempiere 
project and modify them to add the customized classes to the jar files. 
Compare the build.xml from the base directory in both the template and the 
ADempiere project to see how and what to change.

If you launch the server, you should see the changes in the zk files. If you 
make any changes, you will have to restart the server to see them.

The launcher for the client will run the client as per the main project. Here,
most changes you make will be hot-swapped into the application which is really
nice for development.

Exporting the Customization Jars

When your customization is ready, there is a launcher to build the 
customization jars. The two files customization.jar and zkcustomization.jar 
will be added to the lib directory. You can add these to the lib directory 
of the ADempiere installation and execute the setup (RUN_Setup or 
RUN_SilentSetup) to see the changes.



