## ❓ What is Stored Cross-Site Scripting (XSS)?

Stored Cross-Site Scripting (also known as second-order or persistent XSS) occurs when an application receives data from an untrusted source and includes that data in subsequent HTTP responses in an unsafe manner.

For example, a website might allow users to leave comments on blog posts, which are then displayed to other users. Comments are submitted via an HTTP request like this:

```http
POST /post/comment HTTP/1.1
Host: vulnerable-website.com
Content-Length: 100

postId=3&comment=This+post+was+extremely+helpful.&name=Carlos+Montoya&email=carlos%40normal-user.net
```

Once submitted, the comment might be included in the application's response as follows:

```html
<p>This post was extremely helpful.</p>
```

If the application does not process the data properly, an attacker could submit a malicious comment like this:

```html
<script>/* Malicious code here... */</script>
```

The attacker's request would encode this as follows:

```html
comment=%3Cscript%3E%2F*%2BMalicious%2Bcode...%2B*%2F%3C%2Fscript%3E
```

Now, any user viewing the blog post would receive the following response from the application:

```html
<p><script>/* Malicious code here... */</script></p>
```

The attacker's script would then execute in the victim's browser within the context of their session with the application.

---

## 📉 Impact of Stored XSS Attacks

If an attacker can control the script executed in a victim's browser, they can:

- Perform any actions that the user can perform within the application.
- Steal sensitive data accessible to the user.
- Impersonate the user or use their account credentials.

The key difference between reflected and stored XSS lies in their exploitability:

- **Reflected XSS** requires the attacker to trick users into performing specific requests containing the exploit.
- **Stored XSS** automatically embeds the exploit within the application, triggering whenever a user encounters it.

The self-sustaining nature of stored XSS makes it particularly dangerous, especially if the vulnerability affects logged-in users. Unlike reflected XSS, where timing is crucial, stored XSS ensures the exploit is encountered when the user interacts with it.

---

## 📂 Stored XSS in Different Contexts

There are many variations of stored XSS. The location of stored data in the application's response determines:

1. **The type of payload required to exploit the vulnerability.**
2. **The potential impact of the vulnerability.**

If the application performs validation or processing on the data before storage or inclusion in responses, it also influences the type of XSS payload needed.

---

## 🔍 How to Identify and Test Stored XSS Vulnerabilities

Manually testing for XSS vulnerabilities can be challenging. You need to examine all relevant "entry points" where attacker-controlled data can enter the application and all "exit points" where that data might appear in the application's responses.

### Entry Points

Common entry points include:

- Parameters or other data in the URL query string and message body.
- The file path in the URL.
- HTTP request headers (less commonly exploitable for reflected XSS).
- Out-of-band routes, such as:
  - Emails processed by a webmail application.
  - Third-party tweets displayed by a social media app.
  - Data aggregated from other websites.

### Exit Points

Exit points for stored XSS attacks include all possible HTTP responses returned to any user of the application in any context.

### Steps to Test Stored XSS

1. **Identify Links Between Entry and Exit Points**:
   - Determine how data sent to an entry point might appear at an exit point. This may involve testing different combinations of input and analyzing responses.

2. **Account for Data Overwriting**:
   - Stored data can often be overwritten by other actions within the application. For example, a search function might display recent queries, which are replaced by new searches.

3. **Focus on Key Application Features**:
   - Pay special attention to functions like blog post comments or user profiles.

4. **Test Each Entry-Exit Combination**:
   - Submit specific input to an entry point and observe the corresponding output at the exit point. Check if the data appears in responses.

5. **Determine the Context**:
   - When data appears in a response, identify the context (e.g., within HTML tags, JavaScript, or attributes). This helps tailor your XSS payload.

6. **Attempt Exploitation**:
   - Use appropriate payloads to determine if the vulnerability can be successfully exploited.

By following these steps, you can identify and confirm stored XSS vulnerabilities and assess their potential impact on the application.
