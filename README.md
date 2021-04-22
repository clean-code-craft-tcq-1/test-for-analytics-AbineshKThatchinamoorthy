# Test for Analytics

Design tests for Analytics functionality on a Battery Monitoring System.

Fill the parts marked '_enter' in the **Tasks** section below.

## Analysis-functionality to be tested

This section lists the Analysis for which _tests_ must be written.

Battery Telemetrics are collected and stored on a server.
Data for a month is stored in a [csv file](https://en.wikipedia.org/wiki/Comma-separated_values).

Analysis must be done on this csv file to find the following:
- minimum
- maximum
- count of breaches (how many times did it cross the threshold in a month?)
- record trends (date & time when the reading was continuously increasing for 30 minutes)

A PDF report of the analysis must be stored every week.
Notification must be sent when a new report is available.

## Tasks

### List Dependencies

List the dependencies of the Analysis-functionality.

1. Access to the Server containing the telemetrics in a csv file
2. Access to the Server to Store the PDF report every week
3. Notification -> Receiver of the notification, Channel used for transmitting the notification
4. Library functions used to get date and time(from clock), PDF creator/converter, etc.,
5. Threshold limits for minimum and maximum to calculate the breaches and trends
6. Is there any specific time on which the week's report should be stored ? Example : Monday Morning 6 AM 

(add more if needed)

### Mark the System Boundary

What is included in the software unit-test? What is not? Fill this table.

| Item                      | Included?     | Reasoning / Assumption
|---------------------------|---------------|---
Battery Data-accuracy       | No            | We do not test the accuracy of data
Computation of maximum      | Yes           | This is part of the software being developed
Off-the-shelf PDF converter | No 			| Library function -> Not part of the Software being developed
Counting the breaches       | Yes 			| This is part of the software being developed
Detecting trends            | Yes 			| This is part of the software being developed
Notification utility        | No 			| Notification has been mocked in the software

### List the Test Cases

Write tests in the form of `<expected output or action>` from `<input>` / when `<event>`

Add to these tests:

1. Write minimum and maximum to the PDF from a csv containing positive and negative readings
2. Write "Invalid input" to the PDF when the csv doesn't contain expected data

ADDED 4 NEW TEST CASES

| 						Input from CSV                    		  |                        expected output / action     				  | Reasoning / Assumption
|-----------------------------------------------------------------|-----------------------------------------------------------------------|--------------------------------------------------
| 						1.Valid data               				  | Write "Minimum and Maximum values" in PDF     						  | Positive and negative readings   
| 						2.Invalid data                     		  | Write "Invalid input" in PDF                 						  | Incorrect data in CSV
| 				3.Valid data with 2 threshold breaches  		  | Write "Count of breaches : 2" in PDF								  | Breaches for minimum and maximum has to be counted and reported
|4.Valid input with reading increasing continuously for 30 minutes| Write "Trend : Values with date & time for complete 30 minutes" 	  |	Trends shall be recorded when the reading was continuously increasing for 30 minutes
| 						5.Valid data							  | Write "Report content" + "Notification successful msg" in the console | Notification has to be done weekly once
| 						6.Valid data							  | Write "Report content" + "Storage successful msg" in the console	  |	Storage has to be done weekly once

### Recognize Fakes and Reality

Consider the tests for each functionality below.
In those tests, identify inputs and outputs.
Enter one part that's real and another part that's faked/mocked.

| Functionality             | Input        | Output                      		 | Faked/mocked part
|---------------------------|--------------|-------------------------------------|-------------------------------
|Read input from server     | csv file     | internal data-structure     		 | Fake the server store
|Validate input             | csv data     | valid / invalid             		 | None - it's a pure function
|Notify report availability | csv data     | New report sent via email/sms etc., | Mock (dynamic -> report shall be notified only when available - weekly once)
|Report inaccessible server | csv data     | Accessible / Inaccessible           | Fake the server store (no logic required -> input shall be always read)
|Find minimum and maximum   | csv data     | Entries of Min & Max                | None - it's a pure function
|Detect trend               | csv data     | Trend : Values with date & time     | None - it's a pure function
|Write to PDF               | csv data     | Report with all the analysis        | Mock (dynamic -> writes only when valid data is available - weekly once)
