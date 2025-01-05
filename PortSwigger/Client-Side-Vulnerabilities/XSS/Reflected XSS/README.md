## ❓ What is Reflected Cross-Site Scripting?

Reflected cross-site scripting (or XSS) occurs when an application receives data in an HTTP request and includes that data in the immediate response in an unsafe way.

### Example:

Suppose a website has a search function that takes a user-entered search query in a URL parameter:

```
https://insecure-website.com/search?term=gift
```

The application reflects the specified search query in the response to this URL:

```html
<p>You searched for: gift</p>
```

If the application performs no additional processing of the data, an attacker can craft an attack as follows:

```
https://insecure-website.com/search?term=<script>/*+Bad+stuff+here...+*/</script>
```

This URL produces the following response:

```html
<p>You searched for: <script>/* Bad stuff here... */</script></p>
```

If another user of the application requests the attacker's URL, the attacker-provided script will execute in the victim's browser within the context of their session with the application.

### Lab on this topic: [![Open Lab](https://img.shields.io/badge/Open-Lab-blue)](./01.%20%28%D0%A3%D1%87%D0%B5%D0%BD%D0%B8%D0%BA%29%20%D0%9E%D1%82%D1%80%D0%B0%D0%B6%D0%B5%D0%BD%D0%B8%D0%B5%20XSS%20%D0%B2%20HTML-%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%BA%D1%81%D1%82%D0%B5%20%D0%B1%D0%B5%D0%B7%20%D0%BA%D0%BE%D0%B4%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F.md)

---

## 💥 Impact of Reflected XSS Attacks

If an attacker can control a script that executes in a victim's browser, they can typically fully compromise that user. Among other things, the attacker could:

- Perform any action in the application that the user can perform.
- View any information that the user can view.
- Modify any information that the user can modify.
- Initiate interactions with other users of the application, including malicious attacks that originate from the initial victim.

There are various ways an attacker can make the victim user issue a controlled request to carry out a reflected XSS attack. These include:

- Embedding links on a website controlled by the attacker or another website that allows user-generated content.
- Sending a link in an email, tweet, or other message.

The attack can be targeted at a specific user or indiscriminately against any users of the application.

The need for an external delivery mechanism means that the impact of reflected XSS is generally less severe than stored XSS, where a standalone attack can be executed within the vulnerable application itself.

---

## 🖼️ Reflected XSS in Different Contexts

There are many different varieties of reflected cross-site scripting. The location of reflected data in the application's response determines what type of payload is required to exploit it and can also affect the impact of the vulnerability.

Additionally, if the application performs any validation or other processing of submitted data before reflecting it, this will generally influence what type of XSS payload is necessary.

---

## 🔍 How to Find and Test Reflected XSS Vulnerabilities

Manually testing for reflected XSS vulnerabilities involves the following steps:

1. **Test every input point.** Test each input point for data in the application's HTTP requests separately. This includes parameters or other data in the URL query string, message body, and the URL file path. It also includes HTTP headers, although XSS-like behavior that can only be triggered through certain HTTP headers may not be exploitable in practice.

2. **Send random alphanumeric values.** For each input point, send a unique random value and determine whether that value is reflected in the response. The value should be designed to bypass most input validation checks, so it should be short and contain only alphanumeric characters. But it should be long enough that random matches in the response are highly unlikely. Typically, a random alphanumeric value of about eight characters is ideal. You can use Burp Intruder's number payloads with randomly generated hexadecimal values to generate suitable random values. And you can use Burp Intruder's grep payload settings to automatically highlight responses containing the submitted value.

3. **Determine the reflection context.** For each place in the response where the random value is reflected, determine its context. This might be plain text between HTML tags, inside a tag attribute that may be quoted, inside a JavaScript string, etc.

4. **Test initial payloads.** Based on the reflection context, test an initial XSS proof-of-concept payload that will trigger JavaScript execution if reflected in the response unmodified. The simplest way to test payloads is to send the request to Burp Repeater, modify the request to insert the payload, reissue the request, and then review the response to see whether the payload executed. An efficient approach is to leave the original random value in the request and place the XSS payload before or after it. Then set the random value as a search term in Burp Repeater's response view. Burp will highlight every occurrence of the search term, allowing you to quickly find the reflection.

5. **Test alternative payloads.** If the initial XSS payload was modified by the application or blocked outright, you will need to test alternative payloads and methods that might enable a working XSS attack based on the reflection context and the type of input validation performed. For more details, see XSS contexts.

6. **Test the attack in a browser.** Finally, if you succeed in finding a payload that appears to work in Burp Repeater, try the attack in a real browser (by pasting the URL into the address bar or modifying the request in Burp Proxy's intercept view) and see whether the injected JavaScript executes. It is often best to execute some simple JavaScript, such as `alert(document.domain)`, which will cause a visible popup in the browser if the attack succeeds.

---

## 🛡️ FAQs About Reflected Cross-Site Scripting

### How is reflected XSS different from stored XSS?
- **Reflected XSS** occurs when an application takes some input from an HTTP request and embeds that data in an immediate response in an unsafe way.
- **Stored XSS** involves data being stored and then used in a subsequent response in an unsafe way.

### How is reflected XSS different from self-XSS?
- **Self-XSS** involves application behavior similar to regular reflected XSS but cannot be triggered through a crafted URL or cross-domain request. Instead, the vulnerability is only activated if the victim themselves submits the XSS payload from their browser. Delivering a self-XSS attack typically involves social engineering the victim into pasting some attacker-supplied data into their browser. As such, it is generally considered a low-impact issue.
