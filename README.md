# 🔐 SECURLOG – Cryptographic Audit & Defense Suite

**A C-Based Financial Transaction Logging, Security Monitoring, and Audit Management System**

![Language](https://img.shields.io/badge/Language-C-blue)
![Project](https://img.shields.io/badge/Project-Cybersecurity-red)
![Storage](https://img.shields.io/badge/Storage-File%20Handling-orange)
![Status](https://img.shields.io/badge/Status-Academic%20Project-green)

## 📌 1. Introduction

**SECURLOG (Cryptographic Audit & Defense Suite)** is a C-language-based security application designed to demonstrate the fundamental concepts of cybersecurity, user authentication, financial transaction logging, hash chaining, tamper detection, recursive threat scanning, and data masking.

The main purpose of this project is to provide a basic security-oriented environment where financial transactions can be recorded, monitored, and verified.

The system maintains user accounts with different roles, records transaction details in an audit log, detects suspicious login activities, verifies the integrity of stored transaction records, and generates sanitized compliance reports by masking sensitive account information.

SECURLOG combines important programming concepts such as:

- Structures in C
- File handling
- String manipulation
- Functions and modular programming
- Recursion
- Hash generation
- Authentication and access control concepts
- Data masking
- Audit logging

This project demonstrates how programming and basic cryptographic techniques can be used to build a simple financial security and monitoring system.

## 🎯 2. Objectives of the Project

The major objectives of SECURLOG are:

1. **Secure User Authentication:** Allow registered users to access the system using their username and password.
2. **Account Lockdown:** Detect repeated failed login attempts and automatically lock an account.
3. **Secure Transaction Logging:** Record financial transactions with relevant details.
4. **Hash Chaining:** Link transaction records using hashes to demonstrate tamper detection.
5. **Log Integrity Verification:** Verify whether stored transaction records have been modified.
6. **Recursive Threat Detection:** Scan audit logs recursively to identify suspicious activities.
7. **Brute-Force Detection:** Count failed login attempts associated with a particular username.
8. **Data Masking:** Hide sensitive account information when generating compliance reports.
9. **Audit Management:** Maintain a record of important system activities.

## ✨ 3. Key Features

### 3.1 User Authentication System
- Login using username and password.
- Three default user accounts.
- Role information stored for every user.
- Maximum of three login attempts per login session.
- Locked accounts are denied access.

### 3.2 Automatic Account Lockdown
- Records failed login attempts.
- Uses a recursive function to count failed attempts for a username.
- Automatically changes the account status to `LOCKED` when the failure threshold is reached.

### 3.3 Hash-Chained Transaction Logging
- Records financial transactions.
- Generates a hash for each transaction.
- Uses the previous transaction's hash while creating the next transaction record.
- Creates a chain of transaction records.

### 3.4 Tamper Detection
- Reads the audit log.
- Checks whether the previous hash matches the expected hash.
- Recalculates the hash of each transaction.
- Identifies possible modifications in stored records.

### 3.5 Recursive Threat Scanner
- Reads audit records recursively.
- Searches for suspicious keywords.
- Identifies entries containing `FAILED_LOGIN`, `UNAUTHORIZED`, or `TAMPER`.

### 3.6 Brute-Force Interceptor
- Searches audit logs for failed login records.
- Counts failures associated with a specific username.
- Supports the automatic account-locking mechanism.

### 3.7 Sanitized Compliance Report
- Generates a separate compliance report.
- Applies account-number masking.
- Helps demonstrate the importance of protecting sensitive information in audit reports.

## 🛠️ 4. Technologies and Concepts Used

| Technology / Concept | Purpose |
|---|---|
| C Programming | Main programming language |
| Structures | Stores user account information |
| File Handling | Stores users and audit logs |
| `fopen()` | Opens files |
| `fread()` / `fwrite()` | Reads and writes binary user records |
| `fprintf()` / `fgets()` | Writes and reads text log records |
| `strcmp()` | Compares usernames, passwords and hashes |
| `strstr()` | Searches for keywords in log entries |
| `strcpy()` | Copies string data |
| Recursion | Performs threat scanning and failed-login counting |
| DJB2-style Hashing | Generates hash values |
| Data Masking | Hides sensitive account information |

## 🏗️ 5. Project Architecture and Working

The SECURLOG application follows a modular architecture.

The overall execution process is:

1. System initialization.
2. Creation of default user accounts if the user database does not exist.
3. User authentication.
4. Display of the main menu after successful login.
5. Execution of the selected security operation.
6. Storage and verification of audit information.
7. Generation of a sanitized compliance report when requested.
8. Exit from the application.

### Project Workflow

```text
              START
                |
                v
        Initialize System
                |
                v
        Create User Database
         (If Not Existing)
                |
                v
         User Authentication
                |
          +-----+-----+
          |           |
       Success      Failure
          |           |
          v           v
      Main Menu    Check Failed
          |        Login Attempts
          |           |
          |       Account Lock
          |
          v
     Select Operation
          |
    +-----+------+----------------+
    |     |      |                |
    v     v      v                v
Transaction Integrity Threat    Compliance
 Logging    Check   Scan         Report
    |     |      |                |
    +-----+------+----------------+
                |
                v
             EXIT
```

## 📂 6. Project File Structure

After running the program, the following files are created or used:

```text
SECURLOG/
│
├── SecurLog.c
│
├── users.dat
│
├── audit_logs.dat
│
├── temp_users.dat
│
├── compliance_report.txt
│
└── README.md
```

### Description of Files

| File Name | Description |
|---|---|
| `SecurLog.c` | Main C source code containing all functions and program logic. |
| `users.dat` | Binary file containing registered user account information. |
| `audit_logs.dat` | Stores financial transaction records and failed login activities. |
| `temp_users.dat` | Temporary file used during account status updates. |
| `compliance_report.txt` | Contains the generated sanitized audit report. |
| `README.md` | Documentation explaining the project. |

**Note:** The `.dat` files are generated automatically during program execution. They do not need to be created manually.

## 💻 7. Explanation of Important Code Sections

### 7.1 Header Files

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
```

These header files provide the necessary library functions.

- `stdio.h`: Used for input/output and file handling.
- `stdlib.h`: Provides general utility functions.
- `string.h`: Used for string manipulation and comparison.

### 7.2 Macro Definitions

```c
#define USER_FILE "users.dat"
#define LOG_FILE "audit_logs.dat"
#define TEMP_FILE "temp_users.dat"
```

These macros define the filenames used by the application.

Instead of repeatedly writing filenames throughout the program, we use predefined constants.

### 7.3 User Structure

```c
typedef struct {
    char username[30];
    char password[30];
    char role[15];
    char status[10];
} User;
```

The `User` structure stores information about each registered user.

It contains four important fields:

- `username`: Stores the user's login name.
- `password`: Stores the user's password.
- `role`: Stores the user's assigned role.
- `status`: Indicates whether the account is active or locked.

The structure helps organize user information into a single record.

### 7.4 System Initialization – `initializeSystem()`

```c
void initializeSystem() {
    FILE *fp = fopen(USER_FILE, "r");

    if (!fp) {
        fp = fopen(USER_FILE, "w");

        if (fp) {
            User defaultUsers[3] = {
                {"admin", "admin123", "Admin", "ACTIVE"},
                {"auditor", "audit123", "Auditor", "ACTIVE"},
                {"teller1", "teller123", "Teller", "ACTIVE"}
            };

            fwrite(defaultUsers, sizeof(User), 3, fp);
            fclose(fp);
        }
    } else {
        fclose(fp);
    }
}
```

**Purpose:** Initializes the application and creates the user database if it does not already exist.

**Working:**

1. Checks whether `users.dat` exists.
2. If the file does not exist, creates it.
3. Creates three default user accounts.
4. Stores the accounts in binary format using `fwrite()`.
5. Closes the file.

**Default Login Credentials:**

| Username | Password | Role |
|---|---|---|
| admin | admin123 | Admin |
| auditor | audit123 | Auditor |
| teller1 | teller123 | Teller |

### 7.5 User Authentication – `loginUser()`

```c
int loginUser(char loggedInUser[], char loggedInRole[])
```

**Purpose:** Authenticates users before allowing access to the main application.

**Working:**

1. Requests the username and password.
2. Opens the user database.
3. Searches for the entered username.
4. Checks whether the account is locked.
5. Compares the entered password with the stored password.
6. If credentials match, returns success and stores the username and role.
7. If credentials do not match, decreases the remaining login attempts.
8. Records failed login attempts in the audit log.

The program allows three attempts in one login session.

If the failure threshold is reached, the program uses the brute-force interceptor to check the audit records and lock the corresponding account.

### 7.6 Hash Generation – `computeHash()`

```c
void computeHash(const char *input, char *outputHash) {
    unsigned long hash = 5381;
    int c;

    while ((c = *input++)) {
        hash = ((hash << 5) + hash) + c;
    }

    sprintf(outputHash, "%lX", hash);
}
```

**Purpose:** Generates a hash value from an input string.

This function uses the DJB2-style string hashing algorithm.

**Working:**

1. Initializes the hash value to `5381`.
2. Reads the input string character by character.
3. Performs bit shifting and addition.
4. Updates the hash value for every character.
5. Converts the final value into hexadecimal format using `sprintf()`.

**Why is hashing used?**

Hashing converts input data into a fixed-style numerical representation. In this project, it helps demonstrate how transaction records can be checked for modifications.

**Important:** This is a custom DJB2-style hash implementation, not a secure cryptographic hash such as SHA-256. It is used for educational integrity-checking demonstrations.

### 7.7 Transaction Logging – `addTransactionLog()`

```c
void addTransactionLog(char username[])
```

**Purpose:** Records financial transaction information in the audit log.

The program collects:

- Account number
- Transaction description
- Transaction amount
- Logged-in username

**Working:**

1. Accepts transaction details from the user.
2. Reads the previous audit record.
3. Extracts the previous hash.
4. Uses `GENESIS_BLOCK` as the starting value when no previous hash exists.
5. Constructs a string containing transaction information and the previous hash.
6. Generates a new hash using `computeHash()`.
7. Appends the transaction details and generated hash to `audit_logs.dat`.

**Hash Chain Concept:**

Each transaction contains the hash of the previous transaction.

```text
Transaction 1
     |
     v
Hash 1
     |
     v
Transaction 2 + Previous Hash (Hash 1)
     |
     v
Hash 2
     |
     v
Transaction 3 + Previous Hash (Hash 2)
     |
     v
Hash 3
```

This creates a linked sequence of transaction records.

If a transaction is modified, its recalculated hash may no longer match the stored hash.

### 7.8 Log Integrity Verification – `verifyHashChain()`

```c
void verifyHashChain()
```

**Purpose:** Checks the integrity of stored audit records.

**Working:**

1. Opens the audit log.
2. Reads each record.
3. Skips failed-login tracking entries.
4. Extracts transaction information and stored hashes.
5. Compares the previous hash with the expected previous hash.
6. Recalculates the current transaction hash.
7. Compares the calculated hash with the stored hash.
8. Displays a tampering warning if a mismatch is detected.

**Possible messages:**

```text
[V] INTEGRITY VERIFIED:
All audit log entries are pristine and unbroken.
```

Or:

```text
[X] TAMPER DETECTED at Log Entry #1!
Data content modified on disk.
```

This module demonstrates the importance of maintaining the integrity of financial audit records.

### 7.9 Recursive Threat Scanner – `recursiveThreatScanner()`

```c
void recursiveThreatScanner(FILE *fp, int *failedCount)
```

**Purpose:** Scans the audit log to identify suspicious activities.

This function uses recursion instead of a traditional loop to process log records.

**Working:**

1. Reads one line from the audit log.
2. Checks whether the line contains suspicious keywords.
3. Searches for:
   - `FAILED_LOGIN`
   - `UNAUTHORIZED`
   - `TAMPER`
4. If a keyword is found, displays the suspicious record.
5. Increases the anomaly counter.
6. Calls itself to process the next line.
7. Stops when the end of the file is reached.

**Recursion Concept:**

Recursion occurs when a function calls itself repeatedly until a stopping condition, known as the base case, is reached.

In this project, the end of the file is the base case.

### 7.10 Brute-Force Interceptor – `recursiveBruteForceInterceptor()`

```c
void recursiveBruteForceInterceptor(
    FILE *fp,
    char *targetUser,
    int *count
)
```

**Purpose:** Counts failed login records associated with a particular username.

**Working:**

1. Reads audit records one by one.
2. Creates a search pattern using the target username.
3. Searches for matching `FAILED_LOGIN` entries.
4. Increments the counter when a match is found.
5. Calls itself recursively to process the remaining records.
6. Stops at the end of the file.

The login function uses the resulting count to decide whether to lock the account.

### 7.11 Account Auto-Lockdown

When the failed-login threshold is reached, the application updates the user database.

It performs the following steps:

1. Opens the existing user database.
2. Creates a temporary file.
3. Reads all user records.
4. Changes the matching user's status to `LOCKED`.
5. Writes the updated records into the temporary file.
6. Removes the old user database.
7. Renames the temporary file to `users.dat`.

This demonstrates how file handling can be used to update account information.

### 7.12 Sanitized Compliance Report – `generateSanitizedReport()`

```c
void generateSanitizedReport()
```

**Purpose:** Generates a separate compliance report while attempting to mask sensitive account information.

**Working:**

1. Opens `audit_logs.dat`.
2. Creates `compliance_report.txt`.
3. Reads each audit record.
4. Searches for account information using `strstr()`.
5. Attempts to replace account-number digits with `X` characters.
6. Writes the processed records into the compliance report.
7. Counts the number of processed records.
8. Displays a success message.

**Example of Intended Data Masking:**

Original account number:

```text
ACC-9876543210
```

Masked account number:

```text
ACC-XXXXXX3210
```

The objective is to hide sensitive account information while retaining the last four digits for identification.

## 🖥️ 8. Main Menu and Available Operations

After successful authentication, SECURLOG displays the following menu:

```text
==================================================
   SECURLOG: Cryptographic Audit & Defense Suite
==================================================

------------------ SECURLOG MENU ------------------
1. Record Financial Transaction (Hash-Chained Log)
2. Verify Log File Integrity (Tamper Detection)
3. Run Recursive Threat & Security Scan
4. Generate Sanitized Compliance Report (Data Masking)
5. Exit System
---------------------------------------------------
Enter your choice (1-5):
```

### Explanation of Menu Options

| Option | Operation | Function Called |
|---|---|---|
| 1 | Record a financial transaction | `addTransactionLog()` |
| 2 | Verify audit log integrity | `verifyHashChain()` |
| 3 | Scan for security anomalies | `recursiveThreatScanner()` |
| 4 | Generate a sanitized report | `generateSanitizedReport()` |
| 5 | Exit the application | Terminates the menu loop |

## 📤 9. Sample Output and Demonstration

The following examples illustrate the intended usage and messages printed by the program. Actual results depend on the contents of the log files.

### Example 1: Successful Login

**Input:**

```text
Enter Username: admin
Enter Password: admin123
```

**Output:**

```text
[+] Welcome, admin! Assigned Role: [Admin]
```

The system authenticates the user and displays the main menu.

### Example 2: Recording a Financial Transaction

**Input:**

```text
Enter your choice (1-5): 1

--- RECORD FINANCIAL TRANSACTION ---

Enter Account Number: ACC-9876543210
Enter Transaction Detail/Action: Money Transfer
Enter Amount ($): 5000
```

**Output:**

```text
[+] Transaction successfully logged and cryptographically chained!
```

The transaction is stored in `audit_logs.dat` with its previous hash and newly generated hash.

### Example 3: Verifying Log Integrity

**Input:**

```text
Enter your choice (1-5): 2
```

**Intended Output:**

```text
--- RUNNING LOG INTEGRITY CHECK ---

[V] INTEGRITY VERIFIED:
All audit log entries are pristine and unbroken.
```

This message represents a successful integrity check. See the implementation limitations below regarding the current parsing logic.

### Example 4: Running the Recursive Threat Scanner

**Input:**

```text
Enter your choice (1-5): 3
```

**Example Output:**

```text
--- RUNNING RECURSIVE THREAT SCAN ---

[!] Threat Flagged -> USER:admin | ACTION:FAILED_LOGIN | HASH:000000

[*] Scan Complete. Total Security Anomalies Found: 1
```

The scanner identifies suspicious records in the audit log.

### Example 5: Generating a Compliance Report

**Input:**

```text
Enter your choice (1-5): 4
```

**Output:**

```text
[+] Sanitized Compliance Report generated successfully
('compliance_report.txt').
Total items processed: 1
```

A separate report is generated in the project directory.

### Example 6: Exiting the Application

**Input:**

```text
Enter your choice (1-5): 5
```

**Output:**

```text
[+] Exiting SecurLog. Audit trail secured.
```

## ⚙️ 10. How to Install and Run the Project

### Requirements

- GCC Compiler
- Visual Studio Code, Code::Blocks, or another C development environment
- Windows, Linux, or macOS
- Basic knowledge of C programming

### Step 1: Clone the GitHub Repository

```bash
git clone YOUR_REPOSITORY_URL
```

### Step 2: Open the Project Folder

```bash
cd SECURLOG
```

### Step 3: Compile the Program

Using GCC:

```bash
gcc SecurLog.c -o SecurLog
```

### Step 4: Run the Program

**Windows:**

```bash
SecurLog.exe
```

**Linux/macOS:**

```bash
./SecurLog
```

### Step 5: Login

Use one of the default accounts:

```text
Username: admin
Password: admin123
```

### Step 6: Explore the Menu

Choose the required operation from options 1 to 5.

The application automatically creates its user database when it is first executed.

## 🧪 11. Testing Scenarios

The following scenarios can be used to demonstrate the project during a practical examination or presentation.

| Test Case | Action | Expected Behaviour |
|---|---|---|
| Valid Login | Enter correct credentials | User enters the application |
| Invalid Login | Enter incorrect password | Failed attempt is recorded |
| Repeated Login Failures | Enter incorrect credentials repeatedly | Account lockdown is attempted |
| Transaction Logging | Enter transaction information | Record is appended to audit log |
| Integrity Verification | Select option 2 | Audit records are checked |
| Threat Scanning | Select option 3 | Suspicious keywords are identified |
| Report Generation | Select option 4 | Compliance report is created |
| Exit | Select option 5 | Application terminates |

## 🌍 12. Applications of the Project

SECURLOG demonstrates concepts that are relevant to several real-world application areas.

1. **Banking Systems:** Demonstrates transaction logging and audit record management.
2. **Financial Institutions:** Demonstrates the importance of monitoring suspicious login activities.
3. **Cybersecurity Systems:** Demonstrates basic threat detection and security monitoring.
4. **Audit Management:** Demonstrates maintaining transaction records for verification.
5. **Data Privacy:** Demonstrates masking sensitive account information in reports.
6. **Access Management:** Demonstrates basic username and password authentication.
7. **Compliance Reporting:** Demonstrates generating reports containing processed audit records.

## ⚠️ 13. Current Implementation Limitations

This project is an educational prototype. Its current implementation has several limitations that should be considered before using it as a real security application.

- **Password Storage:** Passwords are stored as plain text in the binary user database. A real system should use secure password hashing.
- **Hash Algorithm:** DJB2 is a non-cryptographic hash algorithm. Production applications should use SHA-256 or another appropriate cryptographic integrity mechanism, preferably with authenticated records.
- **Hash Chain Handling:** Failed-login records use `HASH:000000`, and the transaction logger may incorrectly treat this as the previous transaction hash.
- **Integrity Verification:** The current log parsing includes formatting whitespace that can cause recalculated hashes to differ from the original hashes.
- **Threat Detection:** The recursive scanner identifies only specific keywords and does not perform advanced threat analysis.
- **Failed Login Counting:** The interceptor counts matching historical failed-login records, rather than strictly checking consecutive failures.
- **Data Masking:** The current masking loop does not reliably restrict masking to the account-number field. It should be corrected to avoid modifying other transaction data.
- **Role-Based Permissions:** Although user roles are stored, the current main menu does not enforce different permissions for Admin, Auditor, and Teller.
- **Input Validation:** Some inputs use unbounded `%s` and should be replaced with bounded input handling to avoid buffer overflows.

These limitations provide opportunities for future improvements.

## 🚀 14. Future Enhancements

The project can be improved by introducing:

1. SHA-256-based transaction integrity verification.
2. Secure password hashing and password protection.
3. Proper role-based access control.
4. A more advanced intrusion detection mechanism.
5. Accurate consecutive failed-login tracking.
6. Improved account-number masking.
7. Better input validation and error handling.
8. Database integration for efficient record management.
9. A graphical user interface.
10. Exporting audit reports in PDF or CSV format.
11. Timestamp-based transaction records.
12. Stronger protection against unauthorized modifications to audit files.

## 📚 15. Learning Outcomes

By developing this project, we gain practical knowledge of:

- C programming and modular programming.
- Structures and user-defined data types.
- File handling using binary and text files.
- String manipulation and comparison.
- Recursive function implementation.
- Basic hashing concepts.
- Hash-chain-based integrity checking.
- User authentication concepts.
- Security monitoring and threat detection.
- Data masking and report generation.
- Real-world applications of cybersecurity principles.

## 🎓 16. Conclusion

SECURLOG – Cryptographic Audit & Defense Suite is a C-based educational project that demonstrates how fundamental programming concepts can be applied to financial security and audit management.

The project combines user authentication, automatic account lockdown, transaction logging, hash chaining, tamper detection, recursive threat scanning, and data masking into a single application.

Through this project, we understand the importance of maintaining secure transaction records, detecting suspicious activities, verifying data integrity, and protecting sensitive information.

Although the current implementation is a basic prototype and requires further improvements before real-world deployment, it provides a strong foundation for understanding cybersecurity concepts and their implementation using the C programming language.

---

## 👨‍💻 17. Project Information

**Project Name:** SECURLOG – Cryptographic Audit & Defense Suite

**Programming Language:** C

**Project Domain:** Cybersecurity / Financial Audit Management

**Project Type:** Academic / Educational Project

**Primary Concepts:** File Handling, Recursion, Hashing, Authentication, Data Masking, and Audit Logging.

**Source File:** `SecurLog.c`

---

⭐ If you find this project useful for learning C programming and basic cybersecurity concepts, consider giving the repository a star!

**Thank you for exploring SECURLOG!** 🔐
