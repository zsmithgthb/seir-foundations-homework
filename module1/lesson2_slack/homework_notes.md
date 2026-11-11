# Lesson 2 Homework — Seven Alerts, Seven Wins

**Student:** Zac Smith  
**Course:** SEIR Foundations — Class 8  
**Assignment:** Module 1, Lesson 2 — Slack Monitoring  
**EC2 Instance:** `i-03c5c140cbaebf433`  
**Time Zone:** America/Chicago  
**Lab Date:** November 10, 2026 (CST)

## Overview

I completed all seven Python monitoring homework scripts on my EC2 instance and confirmed their alerts in the Balerica Slack channel, `#balerica-alerts`.

I also completed the three required experiments involving severity filtering, disabling alerts, and changing the disk WARNING threshold. I used the actual terminal output and Slack messages to check my results.

## Script 1 — Hello Slack

**Command:** `python hw1_hello_slack.py`

I opened the script in nano and changed `MY_NAME` to `Zac Smith` before running it.

The terminal printed:

`Delivered! Go look at #balerica-alerts. That message is yours.`

Slack received incident `HW-001` with severity INFO and this message:

"Hello from Zac Smith! I sent this alert myself, from my own EC2 instance."

**What I learned:** I can use a Python script to call `send_alert()` and send a customized message from my EC2 instance to Slack. The script identifies the agent, severity, message, and incident ID.

## Script 2 — Severity Tour

**Command:** `python hw2_severity_tour.py`

**Question printed by EC2:** Which severities arrived, and WHY those?

**My answer:** All four severities arrived: INFO, WARNING, ERROR, and CRITICAL. The terminal confirmed each one was delivered to Slack. They all arrived because my `.env` configuration was set to `SLACK_ALERT_SEVERITY=INFO`, which permits every severity in this monitoring system.

**What I learned:** Severity levels allow engineers to distinguish normal informational events from warnings, errors, and critical problems. The alert threshold can filter messages without requiring changes to the Python scripts.

## Script 3 — Server Time

**Command:** `python hw3_server_time.py`

**Question printed by EC2:** How many hours apart are server time and YOUR wall clock?

**My answer:** The server reported Wednesday, November 11, 2026, at 01:26:39 UTC. My corresponding local time was Tuesday, November 10, 2026, at 7:26:39 PM Central Standard Time. UTC was six hours ahead of my wall clock.

**What I learned:** Servers commonly use UTC because it provides a consistent reference for monitoring, logs, and incident investigations. The local date can be different even though the two timestamps describe the same moment.

**Result:** Slack received the INFO message for incident `HW-003`.

## Script 4 — Disk Watch

**Command:** `python hw4_disk_watch.py`

**Original terminal output:**

`Disk is 25.6% used -> severity INFO.`

`Delivered!`

Slack reported 25.6% disk usage and approximately 5.9 GB free.

**Question printed by EC2:** Why is a quiet INFO here GOOD design, not a failure?

**My answer:** A quiet INFO alert is good design because my disk was only 25.6% full, well below the original 75% WARNING threshold. There was no reason to treat healthy disk usage as an emergency. Normal measurements should be recorded without constantly interrupting engineers. This helps prevent alert fatigue, which happens when too many unnecessary warnings cause people to ignore important notifications.

**What I learned:** Monitoring follows a measure-decide-alert process. The script measures real disk usage, compares it with thresholds, and selects the appropriate severity using `if`, `elif`, and `else`.

## Script 5 — Load Check

**Command:** `python hw5_load_check.py`

**Actual results:**

- One-minute load average: 0.00
- Five-minute load average: 0.00
- Fifteen-minute load average: 0.00
- Available CPUs: 2
- Selected severity: INFO
- Slack incident: `HW-005`

**Question printed by EC2:** What would make this number spike on the Balerica bank?

**My answer:** A large number of simultaneous customer transactions could increase the server's load average. Heavy COBOL batch processing, multiple FastAPI payment requests, and other demanding processes could also increase the workload. Load can also rise when tasks are waiting on certain system resources. If the five-minute load average rises above the two available CPUs, this script chooses WARNING.

**What I learned:** Linux load averages describe system demand over one, five, and fifteen minutes. The script compares the five-minute average against the CPU count to decide whether the server may be too busy.

## Script 6 — Incident Drill

**Command:** `python hw6_incident_drill.py`

I ran the script multiple times and observed randomly selected, fictional banking incidents. Every message was labeled as a drill rather than a real banking problem.

**Actual incidents recorded:**

1. `DRILL-2406` — ERROR — The FastAPI `/payments` endpoint returned HTTP 500 twice in one minute.
2. `DRILL-8024` — ERROR — The COBOL `payment_post` program returned code 8, with a simulated business rejection rate above 5%.
3. `DRILL-7391` — WARNING — The simulated SWIFT message queue increased to 42 waiting messages.
4. `DRILL-8203` — CRITICAL — The COBOL `account_lookup` program returned code 12 because the ledger file was unreachable.
5. `DRILL-6756` — CRITICAL — Another simulated ledger-file access failure.
6. `DRILL-7307` — ERROR — Another simulated FastAPI payment endpoint failure.

The terminal confirmed successful delivery, and the alerts appeared in Slack.

**Question printed by EC2:** Which of these fake incidents deserves to wake a human at 3 AM?

**My answer:** The CRITICAL ledger-file failures deserve immediate attention because the bank may be unable to look up customer accounts or access important financial records. Repeated FastAPI payment endpoint failures could also justify waking an on-call engineer if they prevent customers from making payments. A high payment rejection rate deserves investigation because it could indicate a business or processing issue. A growing SWIFT queue is important to monitor, but whether it requires an overnight response depends on its impact and urgency. A successful backup or a minor delay normally would not justify waking someone at 3 AM.

**What I learned:** Not every alert should trigger the same response. Severity should reflect the potential impact on banking operations, and incident drills help engineers practice making those decisions without exposing real customer data.

## Script 7 — Morning Report

**Command:** `python hw7_morning_report.py`

Before running the report, I executed the required EC2 identification command:

`whoami && TZ='America/Chicago' date && ec2-metadata -i`

**Identification results:**

- Username: `root`
- Local time: Tuesday, November 10, 2026, at 7:46:53 PM CST
- EC2 instance ID: `i-03c5c140cbaebf433`

**Actual monitoring report:**

- Report time: November 11, 2026, at 01:47:00 UTC
- Hostname: `ip-172-31-27-129.ec2.internal`
- Operating system: Linux x86_64
- Python version: 3.9.25
- Disk usage: 25.6%
- One-minute load average: 0.00
- Status: All checks ran. The bank greets the day.
- Incident ID: `HW-007`
- Severity: INFO

The terminal printed `Delivered!`, and the complete monitoring report appeared in Slack.

**What I learned:** Instead of sending several unrelated measurements, a monitoring report generator combines time, system identification, disk usage, load, and overall status into one message. This makes it easier for a team to review the condition of a server.

## Experiment 1 — Change Severity Threshold

**Configuration change:**

`SLACK_ALERT_SEVERITY=CRITICAL`

**Prediction before testing:** Only one of the four severity messages would arrive in Slack. INFO, WARNING, and ERROR would be skipped, while CRITICAL would be delivered.

**Command tested:** `python hw2_severity_tour.py`

**Actual terminal results:**

- INFO — skipped by threshold
- WARNING — skipped by threshold
- ERROR — skipped by threshold
- CRITICAL — DELIVERED to Slack

**Prediction versus result:** I predicted that exactly one message would arrive, and exactly one arrived. My prediction was correct.

**What I learned:** The severity threshold works as a filter. Changing the `.env` configuration changes which alerts are delivered without requiring modifications to the Python script.

## Experiment 2 — Disable Alerts

**Configuration change:**

`SLACK_ALERTS_ENABLED=false`

**Command tested:** `python hw1_hello_slack.py`

**Actual result:** The terminal reported that the alert was skipped.

To test the kill switch independently, I also set `SLACK_ALERT_SEVERITY=INFO` while keeping `SLACK_ALERTS_ENABLED=false`. When I ran Script 1 again, it was still skipped.

**What happened:** No new Hello Slack notification was delivered during the disabled-alert test.

**Why this is useful:** A kill switch allows engineers to temporarily stop notifications during planned maintenance or troubleshooting. This prevents unnecessary alert noise without requiring them to delete scripts or remove the Slack integration.

**What I learned:** The enabled/disabled setting and severity threshold are separate controls. An INFO alert can be allowed by the severity filter but still blocked by the kill switch.

## Experiment 3 — Change Disk WARNING Threshold

**Original script condition:**

`elif percent_used >= 75:`

**Temporary experimental condition:**

`elif percent_used >= 20:`

**Prediction before testing:** Since the disk was approximately 25.6% full, lowering the WARNING threshold to 20% would change the alert from INFO to WARNING.

**Command tested:** `python hw4_disk_watch.py`

**Actual terminal output:**

`Disk is 25.6% used -> severity WARNING.`

`Delivered!`

Slack received incident `HW-004` with severity WARNING and showed the actual disk usage of 25.6%.

**Prediction versus result:** My prediction was correct. The same disk usage produced a different severity because I changed the threshold in the monitoring code.

**What I learned:** Alert thresholds determine when a measured system condition becomes worthy of attention. An incorrectly chosen threshold can create excessive warnings even when the system is healthy.

## Restoring the Original Settings

After completing the experiments, I restored the following `.env` settings:

`SLACK_ALERT_SEVERITY=INFO`

`SLACK_ALERTS_ENABLED=true`

I also restored the WARNING threshold in `hw4_disk_watch.py` from 20 back to 75.

The original ERROR threshold remained 90.

This returned the monitoring system to the configuration used for the original homework runs.

## Evidence Submitted

**Slack evidence:** `screenshots/slack_homework_alerts.jpg`

This screenshot shows the completed alerts from at least five different homework scripts, including HW-001, HW-002, HW-003, HW-004, and HW-005.

**EC2 terminal evidence:** `screenshots/hw7_morning_report.jpg`

This screenshot shows my EC2 identification command, instance ID, complete Morning Report output, and successful delivery confirmation.

No webhook secrets, passwords, private keys, or customer information are included in the report.

## Final Reflection

Before this lesson, Slack alerts were something I had seen demonstrated, but not something I had repeatedly operated myself. By running all seven scripts, I learned how Python measures system conditions, assigns alert severity, and sends messages to Slack.

The experiments were especially helpful because they showed how configuration changes affect real behavior. I proved that raising the severity threshold filters out lower-priority alerts, disabling notifications prevents delivery, and lowering a disk threshold changes the severity generated by monitoring code.

The final Morning Report helped me see how individual checks can become a useful server-status report. I now understand why monitoring systems need accurate measurements, sensible thresholds, clear messages, and controls to prevent unnecessary alert noise.