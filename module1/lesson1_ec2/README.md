# SEIR Foundations — Module 1, Lesson 1: EC2

## Week 3 — Balerica Legacy Bank

**Student:** Zac Smith  
**Course:** SEIR Foundations — Class 8  
**Assignment:** Lesson 1 — EC2, COBOL, Git, and APIs  
**Time Zone:** America/Chicago  
**GitHub Repository:** https://github.com/zsmithgthb/seir-foundations-homework

**Instructor's Runbook:** https://github.com/BalericaAI/SEIR-Foundations/blob/main/module1/lesson1_ec2/API/runbook.md

## Overview

For Week 3, I completed seven individual EC2 lab runs using the Balerica Legacy Bank Student Runbook.

For each run, I connected to an EC2 instance using Git Bash and SSH from my Windows computer. I manually installed Git, cloned the SEIR Foundations repository, installed Balerica Legacy Bank, tested COBOL programs, activated the Python virtual environment, and launched the FastAPI application using Uvicorn.

I completed these seven runs across three separate calendar days:

- October 30, 2026 — Runs 1–3
- October 31, 2026 — Runs 4–6
- November 1, 2026 — Run 7

Every official run screenshot shows the identification command, EC2 instance ID, instance boot time, and successful Uvicorn startup.

All seven EC2 instances were terminated after completing the lab work.

## Seven EC2 Lab Runs

The public DNS hostnames below were recovered from my local Git Bash SSH history and matched to the runs using my recorded connection order. The instance IDs and boot times were recorded directly from my EC2 screenshots.

| Run | Date | Time zone | Instance ID | Boot time (UTC) | Public IPv4 DNS | Screenshot | Terminated? |
|---|---|---|---|---|---|---|---|
| 1 | 2026-10-30 | America/Chicago | `i-0bf1ac82bd81b2f3d` | 2026-10-31 03:17:48 | `ec2-54-198-87-63.compute-1.amazonaws.com` | [lab1_run1.jpg](screenshots/lab1_run1.jpg) | Yes |
| 2 | 2026-10-30 | America/Chicago | `i-04c3c1f99d18bef8c` | 2026-10-31 03:17:01 | `ec2-54-91-66-102.compute-1.amazonaws.com` | [lab1_run2.jpg](screenshots/lab1_run2.jpg) | Yes |
| 3 | 2026-10-30 | America/Chicago | `i-061865a0243382a78` | 2026-10-31 03:19:58 | `ec2-54-175-235-87.compute-1.amazonaws.com` | [lab1_run3.jpg](screenshots/lab1_run3.jpg) | Yes |
| 4 | 2026-10-31 | America/Chicago | `i-08226c104a8043b4f` | 2026-10-31 18:25:12 | `ec2-100-54-213-152.compute-1.amazonaws.com` | [lab1_run4.jpg](screenshots/lab1_run4.jpg) | Yes |
| 5 | 2026-10-31 | America/Chicago | `i-02f05a000b33b73f8` | 2026-10-31 18:26:29 | `ec2-100-53-185-143.compute-1.amazonaws.com` | [lab1_run5.jpg](screenshots/lab1_run5.jpg) | Yes |
| 6 | 2026-10-31 | America/Chicago | `i-059a37e9a98814025` | 2026-10-31 18:27:52 | `ec2-100-58-130-93.compute-1.amazonaws.com` | [lab1_run6.jpg](screenshots/lab1_run6.jpg) | Yes |
| 7 | 2026-11-01 | America/Chicago | `i-099a4999305700832` | 2026-11-01 21:38:06 | `ec2-34-201-149-62.compute-1.amazonaws.com` | [lab1_run7.jpg](screenshots/lab1_run7.jpg) | Yes |

## Identification and Application Startup

The required identification command used for the lab evidence was:

```bash
whoami && TZ='America/Chicago' date && ec2-metadata -i && uptime -s
```

Immediately afterward, I started the API with:

```bash
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

Each successful run displayed:

```text
INFO: Application startup complete.
```

## COBOL Core Practice

I completed the COBOL practice on **Run 7**, instance `i-099a4999305700832`.

### Return Codes

| Return code | Test | Result | Screenshot |
|---|---|---|---|
| 0 | Valid account lookup | Success | [cobol_rc0.jpg](screenshots/cobol_rc0.jpg) |
| 4 | Nonexistent account | Account not found | [cobol_rc4.jpg](screenshots/cobol_rc4.jpg) |
| 8 | Zero-dollar payment | Business rejection | [cobol_rc8.jpg](screenshots/cobol_rc8.jpg) |
| 12 | Account lookup from `/tmp` | Environment/file access failure | [cobol_rc12.jpg](screenshots/cobol_rc12.jpg) |

I used `echo $?` immediately after running the COBOL programs to confirm their return codes.

### Fixed-Width Ledger Decoding

I decoded Alice Johnson's account using `grep` and `cut -c`.

| Field | Decoded result |
|---|---|
| Account number | `100001` |
| Customer | Alice Johnson |
| Account status | ACTIVE |
| Raw balance | `000000500000` |
| Actual balance | $5,000.00 |
| Currency | USD |

**Evidence:** [cobol_ledger_decode.jpg](screenshots/cobol_ledger_decode.jpg)

### Transaction Processing

**Rejected transaction:** `TXN-ZS-0004`

I attempted a payment of $0.00. The payment was rejected with return code 8 because the amount must be greater than zero.

**Successful transaction:** `TXN-ZS-0005`

I posted a $125.50 payment for account `100001`.

The COBOL program returned:

```text
TXN=TXN-ZS-0005
ACCOUNT=100001
AMOUNT=125.50
CURRENCY=USD
STATUS=POSTED
REASON=APPROVED
```

The transaction appeared in the payment journal:

```text
TXN-ZS-0005         100001000000012550USDPOSTED  INV-1005
```

Afterward, Alice's displayed account balance remained **$5,000.00**.

The exact commands, outputs, and observations are documented in [lab1_notes.md](lab1_notes.md).

## Git Practice

I cloned my personal homework repository onto an EC2 instance using:

```bash
git clone https://github.com/zsmithgthb/seir-foundations-homework.git
```

I then listed the `module1` directory and verified that it contained my existing homework folders:

```text
lab0.5_basics_contd
lab0_basics
week1_website
```

I did not push from EC2 or store GitHub authentication tokens on the server.

**Evidence:** [git_clone.jpg](screenshots/git_clone.jpg)

## Udemy — Stéphane Maarek

I completed the required Week 3 quizzes.

| Section | Score | Screenshot |
|---|---|---|
| Section 21 | 9 / 9 — 100% | [Maarek_Section21_quiz.jpg](screenshots/Maarek_Section21_quiz.jpg) |
| Section 23 | 12 / 13 — 92.3% | [Maarek_Section23_quiz.jpg](screenshots/Maarek_Section23_quiz.jpg) |
| Section 25 | 5 / 5 — 100% | [Maarek_Section25_quiz.jpg](screenshots/Maarek_Section25_quiz.jpg) |

## Additional Written Assignments

The Week 3 homework also includes the following reports, to be completed and added separately:

**AI Runbook Review:** `ai_review_ZacSmith.txt`

**Spotter Learning Report:** `spotter_ZacSmith.txt`

**AI-Enabled Research:** `AI_enabled_study_lab1.0.txt`

The research covers databases, COBOL, SWIFT, APIs, and the intersection of those systems in legacy modernization.

## Personal Lab Notes

My detailed documentation is available in [lab1_notes.md](lab1_notes.md).

The notes describe all seven lab runs, unique Uvicorn process IDs, troubleshooting, actual COBOL command output, transaction records, mistakes, and commands learned.

## AWS Cleanup

I terminated all seven EC2 instances after completing their assigned lab runs and saved the required evidence on my Windows computer.

I retained the reusable launch template.

A separate check of any leftover EBS volumes, Elastic IPs, and other billable resources should be completed as part of final cleanup.

## What I Learned

Completing seven manual installations helped me gain confidence with AWS EC2, Linux, Git Bash, SSH, COBOL, GitHub, Python virtual environments, and Uvicorn.

I learned how to investigate port conflicts, interpret COBOL return codes, decode fixed-width financial records, and manage multiple EC2 instances simultaneously.

I also learned the importance of recording identification information and collecting accurate screenshot evidence before terminating cloud resources.

The repetition helped me become more comfortable performing the installation without relying on a preconfigured application image.