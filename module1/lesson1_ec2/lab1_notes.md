# SEIR Foundations — Week 3 Lab 1 Notes

**Student:** Zac Smith  
**Lab:** Balerica Legacy Bank — Lesson 1, Steps 0–4  
**Local Time Zone:** America/Chicago  
**Repository:** https://github.com/zsmithgthb/seir-foundations-homework

## Run 1 — October 30, 2026

**Instance ID:** `i-0bf1ac82bd81b2f3d`  
**Boot time (UTC):** `2026-10-31 03:17:48`  
**Screenshot:** `screenshots/lab1_run1.jpg`

I connected to a new EC2 instance using SSH from Git Bash on Windows, installed Git, cloned the course repository, and manually installed Balerica Legacy Bank. The COBOL commands and application worked, but my initial screenshot did not include the required identification command immediately before Uvicorn started. I stopped Uvicorn, ran the full identification command, and restarted the application to capture the correct evidence.

I learned that successful application startup alone is not enough to prove which EC2 instance I used.

**Exact output unique to this run:**

```text
INFO:     Started server process [37087]
```

## Run 2 — October 30, 2026

**Instance ID:** `i-04c3c1f99d18bef8c`  
**Boot time (UTC):** `2026-10-31 03:17:01`  
**Screenshot:** `screenshots/lab1_run2.jpg`

I used a second Git Bash window to connect to a different EC2 instance and repeated the installation manually. I verified that this instance had its own ID and boot time, even though I launched it using the same launch-template configuration. I also corrected the order of the identification command and Uvicorn startup so the screenshot met the homework requirements.

I learned how to manage multiple SSH sessions without confusing the evidence from different instances.

**Exact output unique to this run:**

```text
INFO:     Started server process [37486]
```

## Run 3 — October 30, 2026

**Instance ID:** `i-061865a0243382a78`  
**Boot time (UTC):** `2026-10-31 03:19:58`  
**Screenshot:** `screenshots/lab1_run3.jpg`

I launched and configured a third EC2 instance, then repeated the Git installation, repository clone, bank installation, COBOL commands, and Uvicorn startup. I was initially uncertain whether creating an AMI would have changed my existing launch template, so I checked the installed application rather than assuming the instance was preconfigured. I followed the identification and startup sequence to save evidence tied to this instance.

I learned that creating an AMI does not automatically change an existing launch template and that every required lab run needs independent evidence.

**Exact output unique to this run:**

```text
INFO:     Started server process [36983]
```

## Run 4 — October 31, 2026

**Instance ID:** `i-08226c104a8043b4f`  
**Boot time (UTC):** `2026-10-31 18:25:12`  
**Screenshot:** `screenshots/lab1_run4.jpg`

I launched another EC2 instance on the second day and repeated the manual Balerica Bank installation process. I tested the COBOL lookup, posted the runbook's example payment, examined the payment journal, activated the Python virtual environment, and started Uvicorn. I did not record a new installation error on this run, so I concentrated on following the sequence accurately.

I learned that repetition helps turn a long installation procedure into a familiar process.

**Exact output unique to this run:**

```text
INFO:     Started server process [36463]
```

## Run 5 — October 31, 2026

**Instance ID:** `i-02f05a000b33b73f8`  
**Boot time (UTC):** `2026-10-31 18:26:29`  
**Screenshot:** `screenshots/lab1_run5.jpg`

I connected to another new EC2 instance through its own Git Bash SSH session. I manually completed the installation, ran the COBOL exercises, displayed the payment journal, and started the bank API. Although the commands were the same as my previous runs, I checked the instance ID, boot time, and Uvicorn process information separately.

I learned that identical commands can produce different evidence because each EC2 instance is independent.

**Exact output unique to this run:**

```text
INFO:     Started server process [36289]
```

## Run 6 — October 31, 2026

**Instance ID:** `i-059a37e9a98814025`  
**Boot time (UTC):** `2026-10-31 18:27:52`  
**Screenshot:** `screenshots/lab1_run6.jpg`

I completed the sixth manual installation and successfully started Uvicorn. While testing the application, I saw successful HTTP responses for the main page, API documentation, and OpenAPI specification. The browser also requested `/favicon.ico`, which returned a 404 error, but the actual API endpoints continued working correctly.

I learned to distinguish an optional missing browser icon from an actual application failure.

**Exact output unique to this run:**

```text
INFO:     Started server process [36286]
```

## Run 7 — November 1, 2026

**Instance ID:** `i-099a4999305700832`  
**Boot time (UTC):** `2026-11-01 21:38:06`  
**Screenshot:** `screenshots/lab1_run7.jpg`

I completed the seventh EC2 lab run on the third calendar day and captured the required identification command and successful Uvicorn startup. During the final day's work, I encountered an `Address already in use` error on another EC2 session when port 8000 was occupied, which taught me to check whether a server process was already running. On Run 7, I practiced stopping and restarting Uvicorn and collected the evidence needed for the homework.

I also learned that the `code` command for VS Code works on my Windows computer but is not automatically available inside an EC2 Linux SSH session.

**Exact output unique to this run:**

```text
INFO:     Started server process [35216]
```

---

## COBOL Practice

**Run:** 7  
**Instance ID:** `i-099a4999305700832`  
**Application Directory:** `/opt/balerica-bank`

I practiced COBOL account lookups, payment processing, return codes, ledger decoding, and transaction journals.

### Return Code 0 — Successful Account Lookup

**Commands:**

```bash
./cobol/bin/account_lookup 100001
echo $?
```

**Actual output:**

```text
ACCOUNT=100001
NAME=Alice Johnson
STATUS=ACTIVE
BALANCE=5000.00
CURRENCY=USD
0
```

The program successfully found Alice Johnson's account.

**What I learned:** Return code `0` indicates successful execution.

**Evidence:** `screenshots/cobol_rc0.jpg`

### Return Code 4 — Account Not Found

**Commands:**

```bash
./cobol/bin/account_lookup 999999
echo $?
```

**Actual output:**

```text
ERROR=ACCOUNT NOT FOUND
ACCOUNT=999999
4
```

I intentionally searched for an account that did not exist.

**What I learned:** Return code `4` identifies an account-not-found condition.

**Evidence:** `screenshots/cobol_rc4.jpg`

### Return Code 8 — Business Rule Rejection

**Commands:**

```bash
./cobol/bin/payment_post TXN-ZS-0004 100001 0.00 USD INV-1004
echo $?
```

**Actual output:**

```text
TXN=TXN-ZS-0004
ACCOUNT=100001
AMOUNT=0.00
CURRENCY=USD
STATUS=REJECTED
REASON=AMOUNT MUST BE GREATER THAN ZERO
8
```

I deliberately attempted a payment of zero dollars.

**What I learned:** Return code `8` means the program rejected the transaction because it violated a business rule. A valid account does not automatically mean every payment is acceptable.

**Evidence:** `screenshots/cobol_rc8.jpg`

### Return Code 12 — Environment Failure

I intentionally changed the working directory to `/tmp`.

**Commands:**

```bash
cd /tmp
/opt/balerica-bank/cobol/bin/account_lookup 100001
echo $?
```

**Actual output:**

```text
ERROR=CANNOT OPEN data/accounts.dat
FILE-STATUS=35
12
```

The program could not locate the relative data file because I was outside the bank's expected working directory.

I returned to `/opt/balerica-bank` afterward.

**What I learned:** Running an executable using its absolute path does not guarantee that its relative data-file paths will work.

**Evidence:** `screenshots/cobol_rc12.jpg`

---

## Fixed-Width Ledger Decoding

I decoded Alice Johnson's account record using `grep` and `cut -c`.

**Account number:**

```bash
grep '^100001' data/accounts.dat | cut -c1-6
```

Output: `100001`

**Customer name:**

```bash
grep '^100001' data/accounts.dat | cut -c7-36
```

Output: `Alice Johnson`

**Account status:**

```bash
grep '^100001' data/accounts.dat | cut -c37-44
```

Output: `ACTIVE`

**Account balance:**

```bash
grep '^100001' data/accounts.dat | cut -c45-56
```

Output: `000000500000`

**Currency:**

```bash
grep '^100001' data/accounts.dat | cut -c57-59
```

Output: `USD`

### Decoded Record

| Field | Decoded value |
|---|---|
| Account | 100001 |
| Name | Alice Johnson |
| Status | ACTIVE |
| Balance | $5,000.00 |
| Currency | USD |

The balance is stored as an integer with two implied decimal places. Therefore, `000000500000` represents `$5,000.00`.

**What I learned:** Fixed-width files store information at specific character positions, and tools such as `cut -c` can extract those fields without requiring a database query.

**Evidence:** `screenshots/cobol_ledger_decode.jpg`

---

## My COBOL Transactions

### Rejected Transaction

**Transaction ID:** `TXN-ZS-0004`  
**Account:** `100001`  
**Amount:** `$0.00`  
**Status:** REJECTED  
**Return Code:** `8`

This transaction was rejected because the amount was not greater than zero. The payment journal displayed an entry for `TXN-ZS-0004` with the rejected status.

### Successful Transaction

**Transaction ID:** `TXN-ZS-0005`  
**Account:** `100001`  
**Amount:** `$125.50`  
**Status:** POSTED  
**Return Code:** `0`

**Commands:**

```bash
./cobol/bin/payment_post TXN-ZS-0005 100001 125.50 USD INV-1005
echo $?
```

**Actual output:**

```text
TXN=TXN-ZS-0005
ACCOUNT=100001
AMOUNT=125.50
CURRENCY=USD
STATUS=POSTED
REASON=APPROVED
0
```

**Exact approved transaction journal entry:**

```text
TXN-ZS-0005         100001000000012550USDPOSTED  INV-1005
```

I used:

```bash
cat data/payments.dat
```

to inspect the journal.

I then looked up Alice Johnson's account again.

**Balance after posting:** `$5,000.00`

The transaction was approved and appeared in the payment journal, but Alice's displayed account balance did not change.

**What I learned:** The application's payment journal and account lookup are separate operations. I should verify the actual results instead of assuming that a successful journal entry automatically changes the displayed account balance.

---

## Git Clone Practice

I cloned my own homework repository onto the Run 7 EC2 instance.

**Command:**

```bash
git clone https://github.com/zsmithgthb/seir-foundations-homework.git
```

The repository cloned successfully.

I then ran:

```bash
ls seir-foundations-homework/module1
```

**Actual output:**

```text
lab0.5_basics_contd  lab0_basics  week1_website
```

I did not push changes from EC2 or use a GitHub access token.

**Evidence:** `screenshots/git_clone.jpg`

**What I learned:** Git can clone a public repository directly onto a Linux server, allowing me to inspect project files without configuring credentials for pushing changes.

---

## Mistakes I Made

1. **Missing identification line:** My early Uvicorn screenshots showed successful startup but did not show the required identification command immediately before it. I restarted Uvicorn and retook the screenshots.

2. **AMI versus launch template confusion:** I initially wondered whether creating an AMI had automatically changed my launch template. I learned that these are separate resources and checked for a preinstalled application.

3. **Port 8000 already in use:** When trying to start Uvicorn, I encountered an address-in-use error. I learned why two processes cannot bind to the same listening address and port simultaneously.

4. **Wrong COBOL working directory:** Running the COBOL executable from `/tmp` caused it to fail when opening `data/accounts.dat`. This was an intentional test that helped me understand return code 12.

5. **Running VS Code commands inside EC2:** I tried using `code` over SSH and received `command not found`. I learned to edit the homework files on my Windows machine.

6. **SSH private key location:** I encountered an SSH error when Git Bash could not locate `nvkey1.pem`. I learned that the terminal must be given the correct path to the private key.

7. **Following unnecessary commands:** Some instructions repeated directory changes or altered the examples from the original runbook. I learned to compare the commands directly with the Markdown instructions.

8. **Evidence management:** I had to keep track of seven different instance IDs, boot times, screenshots, and calendar days. I learned that collecting proof is part of the lab, not something to leave until the end.

9. **AWS cleanup:** I learned the difference between stopping a program, disconnecting an SSH session, shutting down Linux, and terminating an EC2 instance.

---

## Commands I Now Know by Heart

- `whoami` — displays the current Linux user.
- `pwd` — displays the current working directory.
- `sudo dnf install -y git` — installs Git on Amazon Linux.
- `git clone` — downloads a repository.
- `cd` — changes the current directory.
- `ls` — lists files and folders.
- `./cobol/bin/account_lookup` — looks up an account.
- `./cobol/bin/payment_post` — attempts a payment.
- `echo $?` — displays the last command's return code.
- `cat data/payments.dat` — displays the payment journal.
- `grep` — searches for matching text.
- `cut -c` — extracts fixed-width fields by character position.
- `source venv/bin/activate` — activates the Python environment.
- `ec2-metadata -i` — displays the instance ID.
- `uptime -s` — displays the instance boot time.
- `uvicorn app.main:app --host 0.0.0.0 --port 8000` — starts the bank API.
- `Ctrl + C` — stops a foreground process such as Uvicorn.
- `git add`, `git commit`, and `git push` — prepare and upload homework changes to GitHub.

## Final Reflection

Completing seven EC2 installations helped me build confidence working with Linux, SSH, Git, COBOL programs, and Python APIs.

The most useful part was learning to recover when something did not work immediately. I encountered incorrect file paths, missing commands, an occupied port, and screenshot requirements that I initially overlooked. Each issue helped me understand why the commands work, what information they produce, and how to verify the results.

I completed the technical exercises, saved the screenshot evidence, and terminated all seven EC2 instances after the lab work.