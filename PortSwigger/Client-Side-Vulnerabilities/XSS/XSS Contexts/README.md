## 🔎 Contexts of Cross-Site Scripting (XSS)

When testing reflected and stored XSS attacks, a key task is to identify the XSS context:

1. **The location in the response** where attacker-controlled data appears.
2. **The data processing** performed by the application on that data.

Based on this information, you can choose one or more potential XSS payloads and test their effectiveness.

> 💡 **Note:** PortSwigger has created a comprehensive [XSS cheat sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet) to assist with testing web applications and filters. It includes filtering by events and tags, as well as examples of sandbox escapes like in AngularJS.

---

### XSS Between HTML Tags

If the XSS context is text between HTML tags, you need to introduce new HTML tags to execute JavaScript.

#### Example Payloads:

```html
<script>alert(document.domain)</script>
<img src=1 onerror=alert(1)>
```

---

### XSS via Client-Side Template Injection

Some websites use client-side templating frameworks, such as AngularJS, for dynamic page rendering. If user input is embedded into these templates insecurely, an attacker can inject malicious template expressions to execute XSS.

For more detailed information, read the article [Client-Side Template Injection](https://github.com/qhoko/What-was-learned/tree/main/PortSwigger/Client-Side-Vulnerabilities/XSS/Client-side%20template%20injection).
