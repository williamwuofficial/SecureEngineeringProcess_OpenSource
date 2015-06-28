# Implementation of a Secure Engineering Process for Open Source Projects

This intern project aims to address the security issues present in many open source projects. In particular, this document will be targeted towards the Open Source Software Defined Networking stack, OpenDaylight (ODL). 

Please see the following [link](https://wiki.opendaylight.org/view/InternProjects:Main#Implement_a_secure_engineering_process_for_OpenDaylight) for more details on this project: 

## Best Practices for Secure Development in Open Source

The following guide will be focued towards secure development flow. For bug patching procedures please see, [link](https://wiki.opendaylight.org/view/Life_Cycle_of_a_Bug) and [instructions](https://wiki.opendaylight.org/view/TSC:Vulnerability_Management) for vulnerability management, ODL security response team.

### Common Practices in Secure Development
1. Training for developers
2. Specific testers and testing prodecures
3. Continuous Integration (CI) testing and standards enforcement
4. Best Practices documentation

## Current Development Lifecycle
1. Gerrit -> Jenkins  (Source Code Management)
2. Bugzilla           (Issue Management)
3. Sonar Qube         (Quality Control)

## Issues to address adoption
1. Scalability
2. Ease of Integration

__General Issues of Open Source__

Open source projects require management of people from a wide variety of skill sets and timezones. Therefore, some of the main issues to adoption of security processes are scability and ease of adoption. Considering this, training and manual testing procedures are difficult to implement and scale for open source projects. CI servers are usually implemented as part of the build process, so tools such as static code analysis can be easily implmented but add a level of assurance. 

## Proposed Solution
1. Dependency Management
    
    

  The current proposed tool is OWASP_dependency_check, that has both a maven and jenkins plugin. Furthermore, the tool checks for vulnerable software against data from the NVD (National Vulnerability Database). In order to address False Positives, an xml file can be included to supress results in a scan. 

2. Static Code Analysis
    
  The tool find-sec-bugs is a findbugs plugin that can be used to scan code for common code security vulnerabilities. A plugin for maven and jenkins is in the process of being tested. An issue that has arisen is the False Positive workflow. 


## Best Practice for Deployment of Opendaylight

### Proposed Threat Model
There are currently 2 proposed models of deployment for OpenDaylight
