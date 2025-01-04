# 🛡️ Cross-Site Scripting (XSS)

This section explains what cross-site scripting is, describes the various types of XSS vulnerabilities, and discusses how to identify and prevent them.

---

## ❓ What is Cross-Site Scripting (XSS)?

Cross-Site Scripting (XSS) is a web security vulnerability that allows attackers to compromise the interaction between users and a vulnerable application. It enables attackers to bypass the same-origin policy designed to separate different websites from each other. XSS vulnerabilities typically allow attackers to impersonate victims, perform actions on their behalf, and access their data. If the victim has privileged access to the application, the attacker can gain full control of its features and data.

---

## 🔍 How Does XSS Work?

XSS works by exploiting a vulnerable website to return malicious JavaScript to users. When the malicious code is executed in the victim's browser, the attacker can compromise their interaction with the application.

Example:
![XSS Example](https://github.com/user-attachments/assets/970bd388-0e2e-43c6-a48b-19bdc61279ec)

When executed, the malicious code grants the attacker control over the victim's interaction with the application. 💻

---

## ✅ Proof of Concept for XSS

You can confirm most types of XSS vulnerabilities by injecting a payload that forces your browser to execute arbitrary JavaScript. Using the `alert()` function is a common approach, as it is short, harmless, and easily noticeable when triggered successfully.

However, there is a limitation when using Chrome. Starting from version 92 (July 20, 2021), cross-origin iframes cannot invoke `alert()`. Since these are used for more complex XSS attacks, you may need to use an alternative payload, such as `print()`.

---

## 📂 Types of XSS Attacks

There are three main types of XSS attacks:

1. **🔄 Reflected XSS**: The malicious script originates from the current HTTP request.
2. **💾 Stored XSS**: The script is retrieved from the website's database.
3. **🖼️ DOM-Based XSS**: The vulnerability exists in the client-side code.

### ✏️ Reflected XSS
Reflected XSS is the simplest form of cross-site scripting. It occurs when an application takes data from an HTTP request and includes it in an immediate response in an unsafe manner.

Example:
```html
<p>Status: All is well.</p>
```
Attack:
```html
<p>Status: <script>/* Bad stuff here... */</script></p>
```

If a user clicks a link crafted by the attacker, the attacker's script is executed in the user's browser in the context of their session with the application.

### 📥 Stored XSS
Stored XSS (also known as persistent or second-order XSS) occurs when an application receives data from an untrusted source and includes it in subsequent HTTP responses without proper validation.

Example:
```html
<p>Hello, this is my message!</p>
```
Attack:
```html
<p><script>/* Bad stuff here... */</script></p>
```

### 🖥️ DOM-Based XSS
DOM-Based XSS occurs when an application includes client-side JavaScript that processes data from an untrusted source insecurely.

Example:
```javascript
var search = document.getElementById('search').value;
var results = document.getElementById('results');
results.innerHTML = 'You searched for: ' + search;
```
Attack:
```html
You searched for: <img src=1 onerror='/* Bad stuff here... */'>
```

---

## 🎯 What Can XSS Be Used For?

An attacker exploiting an XSS vulnerability can:

- 🔐 Impersonate the victim.
- 🔗 Perform actions on behalf of the victim.
- 📑 Access data available to the victim.
- 🛠️ Steal user credentials.
- ⚠️ Deface the website.
- 🧩 Inject trojan functionality into the website.

---

## 🔬 How to Detect and Test for XSS?

1. For reflected and stored XSS:
   - Insert unique input (e.g., `test123`) into each entry point.
   - Identify where the data appears in HTTP responses.
   - Test whether this data can be used to execute JavaScript.

2. For DOM-Based XSS:
   - Use browser developer tools to locate data in the DOM.
   - Check whether the data is exploitable.

---

## 🛡️ How to Prevent XSS?

1. **🔍 Input Validation**:
   - Filter inputs based on expected or allowed characters.

2. **💾 Output Encoding**:
   - Encode data based on the output context (HTML, URL, JavaScript).

3. **📜 Response Headers**:
   - Set `Content-Type` and `X-Content-Type-Options` headers.

4. **🔐 Content Security Policy (CSP)**:
   - Use CSP as a last line of defense to mitigate XSS consequences.

---

## ❓ Frequently Asked Questions

- **How does XSS differ from CSRF?**
  - XSS involves the website returning malicious JavaScript, while CSRF forces the victim to perform unintended actions.

- **How to prevent XSS in PHP?**
  - Use `htmlentities` and `ENT_QUOTES`.

- **How to prevent XSS in Java?**
  - Use libraries like Google Guava for HTML encoding.

---
