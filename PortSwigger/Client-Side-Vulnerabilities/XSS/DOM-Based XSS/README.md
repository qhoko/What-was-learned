## ❓ What is Stored Cross-Site Scripting (Stored XSS)?

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

If the application does not process the data further, an attacker could submit a malicious comment like this:

```html
<script>/* Bad stuff here... */</script>
```

The attacker's request would encode this as follows:

```html
comment=%3Cscript%3E%2F*%2BBad%2Bstuff%2Bhere...%2B*%2F%3C%2Fscript%3E
```

Now, any user viewing the blog post would receive the following in the application's response:

```html
<p><script>/* Bad stuff here... */</script></p>
```

The attacker's script would then execute in the victim's browser within the context of their session with the application.

---

## 📉 Impact of Stored XSS Attacks

If an attacker can control the script executed in a victim's browser, they can generally fully compromise that user. This includes the ability to:

- Perform any actions the user can perform.
- Steal sensitive data accessible to the user.
- Impersonate the user or their account.

A critical difference between reflected and stored XSS lies in exploitability:

- **Reflected XSS** requires the attacker to trick users into making specific requests containing the exploit.
- **Stored XSS** embeds the exploit directly into the application, making it self-sustaining and triggering whenever a user encounters it.

This self-sustaining nature makes stored XSS particularly dangerous in scenarios where the vulnerability affects authenticated users. With reflected XSS, the attack needs to be timed precisely when the user is logged in. With stored XSS, the exploit waits passively, ensuring the user is logged in when encountering it.

---

## 📂 Stored XSS in Different Contexts

There are many varieties of stored XSS. The location of stored data in the application's response determines:

1. **The type of payload required to exploit the vulnerability.**
2. **The potential impact of the vulnerability.**

Additionally, if the application performs validation or other processing on the data before storage or inclusion in responses, it influences the type of XSS payload needed.

For more information, see [XSS Contexts](#).

---

## 🔍 How to Identify and Test Stored XSS Vulnerabilities

Manually testing for XSS vulnerabilities can be complex. You need to examine all relevant "entry points" through which attacker-controlled data can enter the application and all "exit points" where that data might appear in the application's responses.

### Entry Points

Common entry points include:

- Parameters or other data in the URL query string and message body.
- The file path in the URL.
- HTTP request headers, which are less commonly exploitable in reflected XSS.
- Out-of-band routes by which attackers can deliver data to the application, such as:
  - Emails processed by a webmail application.
  - Third-party tweets displayed by a social media app.
  - Aggregated data from other websites included in a news feed.

### Exit Points

Exit points for stored XSS attacks include all possible HTTP responses returned to any user of the application in any context.

### Steps to Test for Stored XSS

1. **Identify Links Between Entry and Exit Points**:
   - Determine how data sent to an entry point might appear at an exit point. This may involve testing different combinations of input and monitoring responses.

2. **Account for Data Overwriting**:
   - Stored data can often be overwritten by subsequent actions in the application. For example, a search function might display recent searches, which are quickly replaced as new searches are performed.

3. **Focus on Key Application Features**:
   - Pay attention to functions like blog post comments or user profiles where data is likely to be displayed to other users.

4. **Test Each Entry-Exit Combination**:
   - Test each identified combination by sending specific input to an entry point and observing the corresponding output at the exit point. Look for cases where the input appears in responses.

5. **Determine the Context**:
   - When data appears in a response, identify the context (e.g., within HTML tags, JavaScript, or attributes). This helps tailor your XSS payload.

6. **Attempt Exploitation**:
   - Use appropriate payloads to determine if the vulnerability can be exploited successfully.

By following these steps systematically, you can identify and confirm stored XSS vulnerabilities and assess their potential impact on the application.
