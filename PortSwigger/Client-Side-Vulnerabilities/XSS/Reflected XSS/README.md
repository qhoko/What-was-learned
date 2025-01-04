## What is Reflected Cross-Site Scripting?
Reflected cross-site scripting (or XSS) occurs when an application receives data in an HTTP request and includes that data in the immediate response in an unsafe manner.

### Example:
Suppose a website has a search function that takes user input from a URL parameter:

```
https://insecure-website.com/search?term=gift
```

The application echoes the specified search term in the response:

```html
<p>You searched for: gift</p>
```

If the application performs no further data sanitization, an attacker can craft a malicious URL like the following:

```
https://insecure-website.com/search?term=<script>/*+Bad+stuff+here...+*/</script>
```

This URL results in the following response:

```html
<p>You searched for: <script>/* Bad stuff here... */</script></p>
```

If another user requests this malicious URL, the attacker-provided script will execute in the victim's browser within the context of their session with the application.

(Laboratory exercise on this topic)

---

## Impact of Reflected XSS Attacks
If an attacker can control a script that executes in the victim's browser, they can usually fully compromise the victim's session. Among other things, the attacker can:

- Perform any action in the application that the user can perform.
- View any information that the user can view.
- Modify any information that the user can modify.
- Initiate interactions with other users of the application, including malicious attacks originating from the initial victim user.

### Delivery Mechanisms:
- Embedding links on a website controlled by the attacker or another website that allows content generation.
- Sending the link via email, tweets, or other messages.

The attack can target a specific known user or be an indiscriminate attack against any users of the application.

The need for an external delivery mechanism makes the impact of reflected XSS generally less severe than stored XSS, where a standalone attack can be implemented within the vulnerable application itself.

---

## Reflected XSS in Different Contexts
There are many different types of reflected cross-site scripting. The location of reflected data in the application's response determines the type of payload required for its exploitation and may also affect the severity of the vulnerability.

Additionally, if the application performs any validation or processing of submitted data before reflecting it, this generally impacts the type of XSS payload required.

---

## How to Identify and Test for Reflected XSS Vulnerabilities

Manual testing for reflected XSS vulnerabilities involves the following steps:

1. **Test each input point.** Individually test each data entry point in the application's HTTP requests. This includes parameters or other data in the URL query string, the message body, and the URL file path. It also includes HTTP headers, although XSS-like behavior triggered only via certain HTTP headers may not be practically exploitable.

2. **Submit random alphanumeric values.** For each input point, submit a unique random value and determine whether this value is reflected in the response. The value should be designed to bypass most input validations, so it should be short and contain only alphanumeric characters. However, it should be long enough to make random matches in the response highly unlikely. A typical example is an 8-character random alphanumeric value. You can use numeric payloads from Burp Intruder with randomly generated hexadecimal values to create suitable random values. You can also use Burp Intruder's grep payload settings to automatically highlight responses containing the submitted value.

3. **Determine the reflection context.** For each location in the response where the random value is reflected, determine its context. This might include text between HTML tags, within a tag attribute enclosed in quotes, inside a JavaScript string, etc.

4. **Test an initial payload.** Based on the reflection context, test an initial XSS payload that will execute JavaScript if it is reflected in the response without changes. The simplest way to test a payload is to send a request in Burp Repeater, modify the request to include the payload, execute the request, and then review the response to see if the payload executed. A practical method is to leave the original random value in the request and insert the XSS payload before or after it. Then, set the random value as the search term in Burp Repeater's response view. Burp will highlight each instance of the search term, allowing you to quickly locate reflections.

5. **Test alternative payloads.** If the initial XSS payload was modified by the application or entirely blocked, you will need to test alternative payloads and techniques that could enable a working XSS attack based on the reflection context and the type of input validation performed.

6. **Test the attack in a browser.** Finally, if you identify a payload that seems to work in Burp Repeater, transfer the attack to a real browser (by entering the URL in the address bar or modifying the request in Burp Proxy's interception view) and verify whether the injected JavaScript executes. Often, it is best to use simple JavaScript such as `alert(document.domain)` to trigger a visible popup in the browser if the attack is successful.

---

## Frequently Asked Questions About Reflected Cross-Site Scripting

### What is the difference between reflected XSS and stored XSS?
- **Reflected XSS** occurs when the application takes input from an HTTP request and immediately embeds it into the response in an unsafe manner.
- **Stored XSS**: Data is stored by the application and embedded in a subsequent response in an unsafe manner.

### What is the difference between reflected XSS and self-XSS?
- **Self-XSS** involves similar application behavior to ordinary reflected XSS; however, it cannot be triggered through a crafted URL or cross-domain request. Instead, the vulnerability is activated only when the victim themselves submits an XSS payload from their browser. Delivering a self-XSS attack typically involves social engineering to persuade the victim to paste attacker-provided data into their browser. As such, it is generally considered a low-impact issue.
