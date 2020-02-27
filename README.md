# survey_and_rescue
Package for implementing the eYRC  2019-20 theme Survey and Rescue

You will be using this package to implement position control and the task logic in further tasks.

**Changelog for launch files**

The *detect_beacons.launch* and *prerequistes.launch* files were updated to reflect their state as required for the final submission.

**Changelog for monitor.pyc**

**v1.5.4**

- Fixed a bug introduced in 1.5.3 that caused LEDs to not turn off on SUCCESS.
- Introduced functionality for totalSuccessServiced in the /stats_sr message which was not implemented earlier. You can use this at the end of the run to verify your score.

**v1.5.3**

- Fixed a bug that caused incorrect locations of "FAILURE" on the topic /serviced_info when the Beacon's timed out, if the order of the LED modules in LED_Config.tsv was not ascending.
- Fixed bug of 180 second kill timer in monitor.pyc  being launched as soon as the file was launched as opposed to after the countdown or after the base.

**v1.5.2**

- Fixed bug of monitor sending "SUCCESS" for Base on topic /serviced_info in Continuous Mode.
- The END message on /serviced_info is sent on Ctrl+C presses as well.
- This will be the version on which the final task will be based. Please use the custom .pyc files created for the task that you will find along with the Task download.

**v1.5.1**

- Fixed bug of monitor sending FAILURE message on topic /serviced_info on timeout despite SUCCESS earlier in Continuous Mode
- Monitor now publishes FAILURE on /serviced_info in case of incorrect services or insufficient payload of the type decided.
- 180 seconds after the start of the run, the monitor script will publish a message of type on topic /serviced_info with location: BASE and info: END. The monitor will stop recording scores after this point. Refer to the upcoming(*as of 24/2/20*) Task 5 instructions to know what action the quad should take when this message is published.

**v1.5.0**

- Added formerly absent decisionEvent information. Reflects current valid /decision_info in num form
- Fixed bug of cumulative timer not resetting if new message on topic /decision_info is received midway while servicing other Beacons. Thanks to team #7453 for first highlighting this bug.

**v1.4.0**

- Fixed NoneType bug in updating stats that occured on a edge case.

**v1.3.1**

- Fixed bug in payload_manager, won't execute if no Beacon is selected.
- Added print statement for Hovering mode when the .pyc file launches

**v1.3.0**

- Added continuous hovering mode to make the monitor feature complete & compatible with the Rulebook
- Added missing implementation of redundant detections, /detection_info published twice for the same instance of the Beacon now gives a redundant detection the second time.
- Sending decision_info to location with no beacon or base now leads to /serviced_info FAILURE being sent in addition to earlier versions which only caused stats.incorrectServices to be incremented in /stats_sr and a message being printed.
- Added strip function to take care of leading and trailing whitespaces
- The script will no longer evaluate blank lines in .tsv files

**v1.2.0**
-Fixed incorrect variable name while starting from BASE mode

**v1.1.1**
-Added print_time and num_beacons params

**v1.1.0**

- Switched from subscriber queue in Arduino to publisher queue in monitor
- Async configurable rates of stats, hover monitoring and led publishing
- Was the first deployed version

**v1.0.0**
-monitor.pyc lights LEDs according to the configuration and timing specified in the Tab-Separated-Value files LED_Config.tsv and LED_Timing.tsv respectively.
-The timeouts are set as per the Rulebook, 30 seconds for both FOOD and MEDICINE (Green and Blue color) Beacons and 10 seconds for RESCUE (Red color) Beacons.
-Hover times over beacons are set to 3, 3, 5 and 5 over FOOD, MEDICINE, RESCUE and BASE respectively.
-It verifies detections published on the topic /detection_info and marks them as correct or incorrect.
- It verifies decisions published on the topic/decision_info and and marks them as correct or incorrect.
- If correct, it starts monitoring the setpoint of of the decided location for the hovering time, after the pre-specified amount of cumulative hover time is reached, it sends a SUCCESS message on the serviced_info topic.
- Else, it adds 1 to incorrect services in the stats message, NOTE: That it will not monitor hovering on the incorrect location and hence not send a Failed or Success message. It is better to think of this as an incorrect service location sent.
