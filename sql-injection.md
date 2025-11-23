# SQL Injection (SQLi)

## Overview

SQL Injection (SQLi) is a code injection technique that exploits security vulnerabilities in an application's database layer. Attackers insert malicious SQL code into input fields, allowing them to manipulate database queries, access unauthorized data, modify or delete data, and potentially take complete control of the database server.

## What Is It?

SQL Injection occurs when:
- User input is not properly validated or sanitized
- SQL queries are constructed using string concatenation with user input
- Attackers insert SQL commands into input fields
- The database executes the malicious code as if it were legitimate

This vulnerability can affect any application that uses a SQL database, including web applications, mobile apps, and desktop software.

## How It Works

### The Attack Process

1. **Vulnerability Discovery**:
   - Attacker identifies input fields (login forms, search boxes, URL parameters)
   - Tests inputs with special SQL characters (', ", --, ;)
   - Observes application behavior and error messages

2. **Injection Point Identification**:
   - Determines which inputs are vulnerable
   - Identifies database type through error messages or behavior
   - Maps out database structure

3. **Payload Crafting**:
   - Creates malicious SQL code
   - Designs queries to extract or manipulate data
   - Uses SQL syntax specific to the database system

4. **Exploitation**:
   - Executes the malicious query
   - Extracts sensitive data (passwords, credit cards, personal information)
   - May modify or delete data
   - Could execute system commands on the server

### Simple Example

**Vulnerable Code (PHP)**:
```php
$username = $_POST['username'];
$password = $_POST['password'];
$query = "SELECT * FROM users WHERE username='$username' AND password='$password'";
```

**Normal Input**:
- Username: `john`
- Password: `secret123`
- Query: `SELECT * FROM users WHERE username='john' AND password='secret123'`

**Malicious Input**:
- Username: `admin' --`
- Password: `anything`
- Query: `SELECT * FROM users WHERE username='admin' --' AND password='anything'`

The `--` comments out the rest of the query, bypassing password verification!

### Types of SQL Injection

1. **In-Band SQLi** (Classic):
   - Error-based: Uses error messages to extract data
   - Union-based: Uses UNION operator to combine results

2. **Blind SQLi**:
   - Boolean-based: Asks true/false questions to the database
   - Time-based: Uses time delays to infer information

3. **Out-of-Band SQLi**:
   - Uses different channels (DNS, HTTP) to extract data
   - Used when in-band methods aren't available

## Real-World Examples

### Notable Incidents

1. **Heartland Payment Systems (2008)**:
   - SQL injection led to breach of 130 million credit card numbers
   - One of the largest payment card breaches in history
   - Cost: over $200 million in damages

2. **TalkTalk (2015)**:
   - UK telecom company breached via SQL injection
   - 157,000 customers' data compromised
   - Fine: £400,000 by UK regulators

3. **Tesla Motors (2014)**:
   - Researchers discovered SQL injection vulnerability
   - Could have exposed customer information
   - Responsibly disclosed and fixed

4. **Fortra (GoAnywhere MFT) (2023)**:
   - Zero-day SQL injection vulnerability exploited
   - Led to ransomware attacks on multiple organizations
   - Affected healthcare, government, and financial sectors

5. **Government Websites**:
   - Regular SQL injection attacks on government portals
   - Often expose citizen data, tax records, and sensitive documents

### Statistics

- SQL injection represents 65%+ of web application attacks
- OWASP ranks injection as #3 in Top 10 Web Application Security Risks
- Average cost of a SQL injection breach: $4+ million
- Automated scanning finds SQLi vulnerabilities in 35% of web applications

## Prevention Methods

### Input Validation and Sanitization

1. **Whitelist Validation**:
   - Only accept known-good inputs
   - Validate data type, length, format, and range
   - Reject any input that doesn't match expected patterns

2. **Input Sanitization**:
   - Escape special characters
   - Remove or encode SQL metacharacters
   - Use built-in security functions

### Parameterized Queries (Prepared Statements)

**Best Practice** - The most effective defense:

**Vulnerable Code**:
```php
$query = "SELECT * FROM users WHERE id = " . $_GET['id'];
```

**Secure Code**:
```php
$stmt = $pdo->prepare("SELECT * FROM users WHERE id = ?");
$stmt->execute([$_GET['id']]);
```

**Benefits**:
- Separates SQL logic from data
- Database treats input as data, not code
- Prevents injection regardless of input content

### Stored Procedures

Use stored procedures with parameterized inputs:

```sql
CREATE PROCEDURE GetUser (@id INT)
AS
BEGIN
    SELECT * FROM users WHERE id = @id
END
```

### Least Privilege Principle

1. **Database Account Restrictions**:
   - Use separate accounts for different application functions
   - Grant minimal necessary permissions
   - Read-only accounts where possible
   - No admin privileges for application accounts

2. **Limit Database Functionality**:
   - Disable unnecessary stored procedures
   - Remove sample databases and accounts
   - Restrict access to system tables

### Web Application Firewalls (WAF)

- Deploy WAF to filter malicious requests
- Configure rules to block SQL injection patterns
- Use both signature-based and behavioral detection
- Examples: ModSecurity, Cloudflare WAF, AWS WAF

### Additional Security Measures

1. **Error Handling**:
   - Don't display detailed error messages to users
   - Log errors server-side only
   - Use generic error pages
   - Prevent information disclosure

2. **Regular Security Updates**:
   - Keep database software patched
   - Update application frameworks and libraries
   - Apply security patches promptly

3. **Code Review and Testing**:
   - Regular security code reviews
   - Automated static analysis (SAST tools)
   - Dynamic application security testing (DAST)
   - Penetration testing

4. **ORM Frameworks**:
   - Use Object-Relational Mapping tools
   - Built-in protection against SQLi
   - Examples: Hibernate (Java), Entity Framework (.NET), Django ORM (Python)

## Detection Techniques

### Signs of SQL Injection Attempts

1. **Log Analysis**:
   - Unusual characters in input fields (', ", --, ;, UNION, SELECT)
   - Error messages related to SQL syntax
   - Unexpected database queries in logs
   - Multiple failed query attempts

2. **Application Behavior**:
   - Unusual application responses
   - Unexpected error messages
   - Slower response times (time-based blind SQLi)
   - Changes in page content

3. **Database Monitoring**:
   - Unusual query patterns
   - Queries from unexpected sources
   - Unauthorized data access attempts
   - Large data extractions

### Monitoring Tools

1. **Web Application Firewalls**:
   - Real-time request inspection
   - Pattern matching for SQL injection signatures
   - Automatic blocking of suspicious requests

2. **Intrusion Detection Systems (IDS)**:
   - Snort
   - Suricata
   - OSSEC

3. **Database Activity Monitoring (DAM)**:
   - Monitors database queries in real-time
   - Detects anomalous behavior
   - Examples: Imperva, IBM Guardium, Oracle Audit Vault

4. **Security Scanners**:
   - SQLMap (testing tool)
   - Burp Suite
   - OWASP ZAP
   - Acunetix
   - Nessus

### Testing for Vulnerabilities

Common test inputs:
```
' OR '1'='1
' OR '1'='1' --
' OR '1'='1' /*
admin' --
' UNION SELECT NULL--
1' AND '1'='1
1' AND '1'='2
```

## Response Actions

### If SQL Injection Is Detected

1. **Immediate Response**:
   - Block the attacking IP address
   - Terminate affected database connections
   - Review recent database transactions
   - Preserve logs for forensic analysis

2. **Investigation**:
   - Identify all affected systems and data
   - Determine scope of unauthorized access
   - Check for data exfiltration
   - Look for backdoors or persistent access

3. **Remediation**:
   - Patch the vulnerable code immediately
   - Change database credentials
   - Review and fix all similar code patterns
   - Restore compromised data from clean backups if needed

4. **Post-Incident**:
   - Conduct full security audit
   - Implement additional monitoring
   - Update security policies and procedures
   - Notify affected parties if data was compromised
   - Report to authorities if required by regulations

## Attack Examples by Database

### MySQL/MariaDB
```sql
' UNION SELECT username, password FROM users--
' AND 1=0 UNION ALL SELECT table_name, NULL FROM information_schema.tables--
```

### PostgreSQL
```sql
'; DROP TABLE users--
' OR 1=1; SELECT version()--
```

### Microsoft SQL Server
```sql
'; EXEC xp_cmdshell('whoami')--
' UNION SELECT name FROM sys.tables--
```

### Oracle
```sql
' UNION SELECT username, password FROM all_users--
' AND 1=2 UNION SELECT NULL, banner FROM v$version--
```

## Best Practices Summary

✅ Always use parameterized queries/prepared statements
✅ Implement strict input validation and sanitization
✅ Use stored procedures with parameters
✅ Apply principle of least privilege to database accounts
✅ Deploy Web Application Firewall (WAF)
✅ Never display detailed error messages to users
✅ Keep all software and frameworks updated
✅ Use ORM frameworks when possible
✅ Conduct regular security testing and code reviews
✅ Implement database activity monitoring
✅ Train developers on secure coding practices
✅ Escape all user input before use in queries
✅ Use security headers and HTTPS

## Conclusion

SQL Injection remains a critical vulnerability despite being well-understood and preventable. The key to defense is treating all user input as untrusted and using parameterized queries consistently. Modern frameworks and ORMs provide built-in protections, but developers must use them correctly. Regular testing, security awareness, and defense-in-depth strategies are essential for protecting against this persistent threat.

**Remember**: Never trust user input. Always validate, sanitize, and use parameterized queries!
