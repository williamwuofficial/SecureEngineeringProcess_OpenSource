#Findbugs Jenkins Plugin - Auditing Extension

##Brief
The Auditing Extension to the findbugs jenkins plugin adds functionality in the form of an auditing view. This would allow warnings found using the findbugs scanner to be identified as false positives and in subsequent builds the warning/s would not show as part of the published statistics. 

![alt tag](https://raw.github.com/willtmwu/SecureEngineeringProcess_OpenSource/master/plugins/pictures/findbugs-audit_view.png)

##How it works
Each time the build process is run, the maven findbugs scanner would publish an XML report in the target directory. When configured, the jenkins post-build action would then trigger, to parse the results of the report and publish the result statistics. 

The auditing extensions is also a triggered post-build action, run after the results have been published. A view is presented, to select which of the published results are false positives and will hence not be displayed as an issue in subsequently published build results. This is accomplished by listing warnings in a table view, with a column for a checkbox to identify FP (false positive). Selected warnings are updated on the button press, then the page reloads have removed the warnings from the published results and displaying the FP in another table. However, the graph will not always immediately display the updated results. 

##Code Architecture
The jenkins findbugs plugin is dependent on the analysis-core plugin, which the majority of the code and logic is derived. Each time a project is built, a number of links are added to the build page, example_project/build/25. These links are reffered to as Action objects and can be added to the AbstractBuild object on a successful build. To expose these objects, the abstract class Publisher or variant of, must be extended. 

To allow for the auditing process to occur, the statistics must be able to be modified from the auditing view. This is done by obtaining a reference to a analysis-core.BuildResult object through the AbstractionBuildAction published beforehand. FileAnnotations (the warnings) are then removed and updated, however to ensure persistence between jenkins restarts, they are serialised into the filesystem. 

##Future Works
###In Progress
1. Moving the current auditing code into the parent (analysis-core) plugin, so that other dependant plugins can also benefit from the feature. 
2. Implement functionality to allow threshold and health of the build to change after each audit update
3. Documentation for this auditing feature. 
        
###Planned
1. Design and development of a FP (false positive) server system. 
2. Extend the auditing feature further to incorporated linking known issues to the bugzilla issue tracker. 

##Installation
As the plugin code has yet to be merged to the main jenkins branch, the way to install the plugin is with the compiled .hpi file. As an administrator, go to 'Manage Plugins' and under the advanced tab upload the .hpi package.  

![alt tag](https://raw.github.com/willtmwu/SecureEngineeringProcess_OpenSource/master/plugins/pictures/installation_process.png)

##Running
To configure and run the plugin, on the job configuration page. Add a post-build action of "Publish Findbugs Warning Results" and then also add "Enable Findbugs Auditing". This will setup the auditing results to be correctly based off and filter the warning results. 

	NOTE: The auditing action must be placed after the publish action

After post-build action has been added, the auditing table should be visible in the next build. 

The table will show a list of the warnings for the build and/or the previously assigned False Positives from a previous build. These results will carry over to the graph in the next build. In the unconfirmed warnings table, select the checkbox for any warnings that are False Positives then press Update. 


![alt tag](https://raw.github.com/willtmwu/SecureEngineeringProcess_OpenSource/master/plugins/pictures/findbugs-audit_configure.png)


##Summary

The following is a list of things to note and be aware of if using this plugin. 

1. The threshold feature of findbugs warnings is not currently supported and compatible with the auditing functionality
2. The user must have CONFIGURE or UPDATE permissions, for the audit view to allow warnings to be marked as FP
3. After auditing has been performed, the trend graph will update in the next successful build
4. Auditing the warnings is restricted to the latest build only
