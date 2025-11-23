# Password Attack

## Overview

Password attacks are methods used by cybercriminals to gain unauthorized access to systems by discovering or bypassing password authentication. Despite advances in authentication technology, passwords remain the primary security mechanism for most systems, making them a constant target for attackers. Password attacks range from simple guessing to sophisticated cryptographic attacks, and they remain one of the most successful attack vectors in cybersecurity.

## What Is It?

Password attacks involve:
- Attempting to guess or crack passwords
- Exploiting weak password practices
- Stealing password databases
- Intercepting passwords in transit
- Bypassing password authentication mechanisms
- Using social engineering to obtain passwords

These attacks succeed because of weak passwords, password reuse, lack of multi-factor authentication, and human tendencies toward convenience over security.

## How It Works

### Attack Categories

Password attacks can be divided into several types, each with different techniques and success rates:

## Types of Password Attacks

### 1. Brute Force Attack

**Description**: Systematically trying every possible combination until the correct password is found.

**How It Works**:
- Automated tools try all possible character combinations
- Starts with simple combinations (aaa, aab, aac...)
- Continues until password is found or search exhausted
- Time required increases exponentially with password length

**Tools**: Hydra, John the Ripper, Hashcat

**Effectiveness**: 
- Very effective against short, simple passwords
- 6-character password can be cracked in minutes
- 12+ character passwords become impractical to brute force

See [brute-force-attack.md](brute-force-attack.md) for detailed information.

### 2. Dictionary Attack

**Description**: Uses pre-compiled list of common passwords and words.

**How It Works**:
- Tests passwords from wordlist (common passwords, dictionary words)
- Much faster than pure brute force
- Often includes common variations (Password1, password123)
- Can be customized for target (company name, location)

**Common Passwords Tested**:
- password, 123456, qwerty, admin
- Company name + year
- Season + year (Summer2024)
- Sports teams, character names

**Success Rate**: High—millions still use common passwords

### 3. Hybrid Attack

**Description**: Combines dictionary words with common substitutions and additions.

**How It Works**:
- Takes dictionary words
- Adds numbers (password123)
- Substitutes characters (p@ssw0rd)
- Adds special characters (password!)
- Tests common patterns

**Examples of Transformations**:
- password → Password1, passw0rd, password!, Pass@word

### 4. Credential Stuffing

**Description**: Using stolen username/password pairs from one breach to access other accounts.

**How It Works**:
- Attackers obtain credentials from data breaches
- Try same credentials on multiple websites/services
- Exploits password reuse
- Automated at massive scale
- High success rate due to widespread password reuse

**Real Impact**:
- 65% of people reuse passwords across accounts
- Stolen credentials readily available on dark web
- Can compromise dozens of accounts per victim

### 5. Password Spraying

**Description**: Trying common passwords against many accounts instead of many passwords against one account.

**How It Works**:
- Uses list of common passwords
- Tries one password across many usernames
- Avoids account lockout policies
- Targets organizational accounts
- Time-delayed to avoid detection

**Example**:
- Try "Password123" on 1000 accounts
- Wait 30 minutes
- Try "Summer2024" on same 1000 accounts
- Repeat with other common passwords

**Why It Works**: Organizations have many accounts, and some users always choose weak passwords.

### 6. Rainbow Table Attack

**Description**: Using precomputed tables of password hashes to reverse-engineer passwords.

**How It Works**:
- Pre-calculated hash values for millions of passwords
- When attacker gets password hashes, looks them up in table
- Instantly reveals password if match found
- Trade-off: storage space for computation time

**Defense**: Salting passwords makes rainbow tables ineffective

**File Size**: Rainbow tables can be terabytes in size

### 7. Pass-the-Hash

**Description**: Using captured password hash without needing to crack it.

**How It Works**:
- Attacker captures password hash (not actual password)
- Uses hash directly for authentication
- Exploits systems that accept hash-based authentication
- Common in Windows environments (NTLM hashes)

**Tools**: Mimikatz, Metasploit

**Prevention**: Modern authentication protocols, credential guard

### 8. Keylogger Attack

**Description**: Recording keystrokes to capture passwords as they're typed.

**How It Works**:
- Malware records all keyboard input
- Sends logged data to attacker
- Captures passwords, credit cards, messages
- Hardware keyloggers plug into USB/PS2 ports

**Types**:
- Software keyloggers (trojans, spyware)
- Hardware keyloggers (physical devices)
- Wireless keyloggers

### 9. Shoulder Surfing

**Description**: Directly observing someone entering their password.

**How It Works**:
- Physical observation of password entry
- Can use cameras or direct viewing
- Particularly effective in public spaces
- Low-tech but surprisingly effective

**Common Locations**: ATMs, coffee shops, airports, offices

### 10. Social Engineering

**Description**: Tricking users into revealing their passwords.

**Methods**:
- Phishing emails requesting password
- Fake login pages
- Pretexting (posing as IT support)
- Quid pro quo attacks

See [social-engineering.md](social-engineering.md) and [phishing-attack.md](phishing-attack.md) for details.

### 11. Man-in-the-Middle (MITM)

**Description**: Intercepting passwords during transmission.

**How It Works**:
- Attacker intercepts network traffic
- Captures credentials sent over network
- Particularly effective on unencrypted connections
- Can also modify traffic

See [man-in-the-middle-attack.md](man-in-the-middle-attack.md) for details.

### 12. Password Reset Exploitation

**Description**: Exploiting weak password reset mechanisms.

**Methods**:
- Guessing security questions
- Intercepting reset emails
- Social engineering support staff
- Exploiting account recovery features

**Common Weak Questions**:
- Mother's maiden name (publicly available)
- City of birth (often on social media)
- First pet's name (often shared online)

## Real-World Examples

### Notable Incidents

1. **RockYou Breach (2009)**:
   - 32 million plaintext passwords exposed
   - Revealed password patterns and weaknesses
   - Most common password: "123456" (290,000+ users)
   - Changed approach to password security research

2. **LinkedIn Breach (2012)**:
   - 6.5 million password hashes stolen
   - Passwords not salted, easily cracked
   - Over 90% cracked within days
   - Demonstrated importance of proper password storage

3. **Ashley Madison (2015)**:
   - 36 million accounts compromised
   - Weak password hashing (MD5)
   - 11 million passwords cracked quickly
   - Included government and corporate emails

4. **Yahoo Breaches (2013-2014)**:
   - 3 billion accounts affected
   - Password reset question answers stolen
   - Security questions easily guessed or researched
   - Led to forced password resets

5. **Collection #1 (2019)**:
   - Massive compilation of breached credentials
   - 773 million unique email addresses
   - 21 million unique passwords
   - Used for credential stuffing attacks

6. **Facebook (2019)**:
   - Hundreds of millions of passwords stored in plaintext
   - Accessible to thousands of employees
   - Violated basic security practices
   - No evidence of abuse, but massive security failure

### Statistics

- 81% of breaches involve weak or stolen passwords
- Average person has 100+ online accounts
- 53% reuse passwords across multiple accounts
- 59% use name or birthday in passwords
- Most common password length: 8 characters
- Only 31% use password managers
- 2FA adoption rate: only 28% of accounts
- Password attacks cost businesses $6 billion annually

## Prevention Methods

### Strong Password Practices

1. **Password Complexity**:
   - **Minimum 12-16 characters** (longer is better)
   - Mix of uppercase and lowercase letters
   - Include numbers and special characters
   - Avoid dictionary words
   - Avoid personal information

2. **Password Uniqueness**:
   - **Never reuse passwords** across accounts
   - Different passwords for work and personal
   - Change passwords after breaches
   - Assume any leaked password is permanently compromised

3. **Passphrases**:
   - Use long phrases instead of complex words
   - Example: "Coffee!Tastes$Better@Morning!" (31 chars)
   - Easier to remember, harder to crack
   - Can include spaces, punctuation

### Technical Defenses

1. **Multi-Factor Authentication (MFA/2FA)**:
   - **Most effective defense** against password attacks
   - Requires second factor beyond password
   - Types: SMS, authenticator apps, hardware tokens, biometrics
   - Makes stolen passwords largely useless
   - **Enable everywhere possible**

2. **Password Managers**:
   - Generate strong, unique passwords
   - Securely store all passwords
   - Auto-fill credentials
   - Alert for reused or weak passwords
   - Popular options: 1Password, LastPass, Bitwarden, Dashlane

3. **Password Hashing (For Developers)**:
   - Use strong hashing algorithms (bcrypt, Argon2, PBKDF2)
   - Always salt passwords
   - Use slow hash functions (computationally expensive)
   - Never store plaintext passwords
   - Use per-password unique salts

4. **Account Lockout Policies**:
   - Lock after 3-5 failed attempts
   - Temporary lockout (15-30 minutes)
   - CAPTCHA after failed attempts
   - Alert users of failed login attempts
   - Monitor for brute force patterns

5. **Password Policies**:
   - Enforce minimum length (12+ characters)
   - Require periodic changes (controversial—can lead to weak incremental changes)
   - Prevent common passwords
   - Check against breach databases (Have I Been Pwned)
   - Screen for company-specific information

### Organizational Measures

1. **Security Training**:
   - Password best practices
   - Recognize password attacks
   - Use of password managers
   - MFA enrollment
   - Phishing awareness

2. **Monitoring & Detection**:
   - Monitor failed login attempts
   - Detect credential stuffing patterns
   - Geographic anomaly detection
   - Impossible travel detection
   - Alert on suspicious activity

3. **Passwordless Authentication**:
   - Biometric authentication
   - Hardware security keys (YubiKey)
   - Certificate-based authentication
   - Magic links via email
   - Reduces reliance on passwords

## Detection Techniques

### Signs of Password Attack

1. **Log Analysis**:
   - Multiple failed login attempts
   - Login attempts from unusual locations
   - High volume of authentication requests
   - Sequential failed attempts across accounts
   - Off-hours login attempts

2. **Network Monitoring**:
   - Unusual authentication traffic patterns
   - Connections to known malicious IPs
   - Large-scale authentication attempts
   - Traffic to password cracking sites

3. **User Behavior**:
   - Sudden account activity changes
   - Access from new devices/locations
   - Unusual data access patterns
   - Configuration changes by compromised accounts

### Monitoring Tools

1. **SIEM Systems**:
   - Splunk, IBM QRadar, Microsoft Sentinel
   - Aggregate and analyze authentication logs
   - Detect anomalous patterns
   - Alert on suspicious activity

2. **Password Security Tools**:
   - **Have I Been Pwned**: Check if passwords are in breaches
   - **PasswordMeter**: Evaluate password strength
   - **KeePass**: Open-source password manager
   - **Authy/Google Authenticator**: MFA apps

3. **Intrusion Detection**:
   - Monitor authentication attempts
   - Detect brute force patterns
   - Alert on credential stuffing
   - Block suspicious IPs

## Response Actions

### If Password Attack Detected

1. **Immediate**:
   - Block attacking IP addresses
   - Enable CAPTCHA on login
   - Force password resets for affected accounts
   - Notify affected users
   - Enable additional authentication challenges

2. **Investigation**:
   - Identify scope of attack
   - Check for successful logins
   - Review accessed data
   - Preserve logs for analysis
   - Identify attack method

3. **Remediation**:
   - Reset compromised passwords
   - Enable MFA if not present
   - Review and strengthen password policies
   - Patch vulnerabilities
   - Update security controls

### If Your Password Is Compromised

1. **Immediate Actions**:
   - Change password immediately on secure device
   - Change password on all sites where it was reused
   - Enable MFA on all accounts
   - Scan device for malware
   - Review account activity for unauthorized access

2. **Monitoring**:
   - Watch accounts for suspicious activity
   - Monitor credit reports if financial info involved
   - Check for unauthorized purchases or transfers
   - Set up security alerts

3. **Prevention**:
   - Use password manager going forward
   - Create unique passwords for each account
   - Enable MFA everywhere
   - Register with breach notification services

## Best Practices Summary

✅ Use passwords 16+ characters long
✅ Never reuse passwords across accounts
✅ Enable MFA on all accounts (most important!)
✅ Use password manager to generate and store passwords
✅ Use passphrases instead of complex single words
✅ Change passwords after known breaches
✅ Implement account lockout policies
✅ Use strong password hashing (bcrypt, Argon2)
✅ Always salt passwords
✅ Monitor for failed login attempts
✅ Screen passwords against breach databases
✅ Train users on password security
✅ Consider passwordless authentication options
✅ Never store passwords in plaintext
✅ Use unique passwords for administrative accounts

## Password Strength Examples

**Weak** (Cracked instantly):
- password
- 123456
- admin
- qwerty
- letmein

**Moderate** (Cracked in hours/days):
- Password123
- John2024
- MyDog2023
- Summer2024!

**Strong** (Years to crack):
- J$7mK@9pLw#3nQ
- Tr0ub4dor&3 (still not ideal—known pattern)
- Coffee!Tastes$Better@Morning! (good passphrase)
- x7G#mK9@pL$2nQ5vR!8 (excellent random)

## Conclusion

Password attacks remain one of the most common and successful attack methods because passwords are still the primary authentication mechanism and users often choose weak or reused passwords. The combination of strong, unique passwords and multi-factor authentication provides robust defense against most password attacks. Organizations must enforce strong password policies, implement technical controls, and educate users. As technology evolves, the industry is moving toward passwordless authentication, but until then, password security remains critical.

**Remember**: Your password is the key to your digital life. Make it strong, make it unique, and protect it with MFA!
