0x19. Postmortem |Incident Report| by Natan Folla


On December first we experience an outage in our API infrastructure. Due to that we are providing this Postmortem or incident report.

As we indicate in the paragraph above on Dec 1, 2022 after midnight, we understand earlier that day a service issue has impacted our valued developers, users and customer so we apologize to everyone affected.

Issue Summary

From 9:04 AM to 11:55 PM local time |GMT+3|, requests of most of APIs resulted error response message due to this our applications reduced functionality at 50%. The root cause of this outage was an invalid configuration change that exposed a bug in a widely used internal library.

Timeline (local time GMT+3)

7:46 AM:  Configuration push begins
9:04 AM: Outage begins
12:05 AM: Pagers alerted teams
4:35 PM: Failed configuration change rollback
8:02 PM: Successful configuration change rollback
10:24 PM: Server restarts begin
11:55 PM: 100% of traffic back online

Root Cause

AT 7:46 PM local time|GMT+3|, a configuration change was inadvertently released to our environment without first releasing it in a testing environment. The change specified an invalid address for the authentication server in production. This exposed a bug in the libraries which led to block permanently while attempting to resolve the invalid address to physical services. Since the internal monitoring system permanently blocked the authentication library the combination of the bug and configuration error quickly caused all the server threads to be consumed. The servers began repeatedly hanging and restarting as they attempted to recover and at 9:04 AM local time|GMT+3|, the service outage began.

Resolution and recovery

AT 9:04 AM local time|GMT+3|, the monitoring system alerted our engineers, At 11:05 AM the incident response team identify the monitoring system. At 12:05 AM, we attempted to rollback the problem configuration change. At 8:02 PM the problem was addressed and successfully rolled back after many failed attempts. 

To help with the recovery, we turned off some of our monitoring systems which were triggering the bug. As a result, we decided to restart servers gradually at 10:24 PM local time|GMT+3|, to avoid possible cascading failures. By 11:05 PM, 40% of traffic was restored and 100% of traffic restored by 11:55 PM.

Corrective and Preventative Measures

Since the first of December we conducted an internal review and analysis of the outage. Five major actions taken as a corrective and preventative measure;

1st Disable the current configurations

2nd Change the rollback process

3rd Fix the underlying authentication library

4th Improve process

5th develop better mechanism for quickly delivering status notification


We are committed to quickly improve our technology so we appreciate your feedback and patience. Once again apologies for the impact we cause on developers, users and customers and Thank you for your support.
