<!--

 Copyright (C) 2019 Intel Corporation

 SPDX-License-Identifier: Apache-2.0

-->

This directory is for code that enables reporting of the pass/fail status to the test results database
Follow the same naming conventions as for other classes, modules, etc
Test Suite Modifications and Execution

Test Suite execution with Report Database
To enter the test results in to a database

In terminal navigate to folder in which test.robot file is present,

pybot -d ./output/ --listener ../utils/src/report/mylistener.py test.robot

Test Suite execution without Report database
To execute the test suite during development or for debugging the code 

In terminal navigate to folder in which test.robot file is present,

pybot -d ./output/ test.robot

Required packages:
Install mysql connector

sudo pip install --allow-external mysql-connector-python mysql-connector-python

If failed, download and install the package from below,

https://dev.mysql.com/downloads/file/?id=458964

Steps for integration:
1.	Place below two python files in ../utils/src/report/ folder
a.	mylistener.py
b.	samplereport.py

Changes in .robot file

2.	Add  ${report} variable under variables section in .robot file

*** Variables ***
${report}                 ''

3.	Add ${EdgeDetails} under each testcase

${EdgeDetails} =  Create List  UUID1  Sensor1  Sensor_Hub1  Gateway1  Inputs1  Comments1  Reference1

4.	Pass the ${report} variable to all testcases
Example:
a.	Sample: 
verify  ${report}
b.	UC7.10 TC1: 
${status} =  login  ${cfg_ethBT}  ${report}
Changes in testcase.py python file
5.	Change the signature of the test case function in python file
Example:
a.	Sample:
def verify(self, report):
b.	UC7.10 TC1:
def login(cfg, report):

6.	Store the test details in testDetails variable
testDetails=["0","100","logs can be added here for additional details"] 

testDetails should have 3 values
        1. Pass/Fail: 1/0
        2. Accuracy: 100
        3. Details: <Value is within the range>

7.	Call report.recordDetail(testDetails)        
try:
report.recordDetail(testDetails) 
except AttributeError as e:
           print(self.__class__.__name__ +"-"+str(e))   

8.	      
Next Steps:
Discuss with team to modify the database model as required.