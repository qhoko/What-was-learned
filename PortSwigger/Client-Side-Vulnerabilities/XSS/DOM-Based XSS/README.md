## ❓ What is DOM-based Cross-Site Scripting (DOM XSS)?

DOM-based XSS vulnerabilities typically arise when JavaScript takes data from an attacker-controlled source, such as a URL, and passes it to a sink that supports dynamic code execution, such as `eval()` or `innerHTML`. This allows attackers to execute malicious JavaScript, often enabling them to hijack other users' accounts.

To exploit a DOM XSS vulnerability, the data must be placed into a source in a way that it flows to the sink and triggers arbitrary JavaScript execution.

The most common source for DOM XSS is the URL, often accessed via the `window.location` object. An attacker can craft a link to direct the victim to a vulnerable page with a payload in the query string or URL fragment. In certain cases, such as on 404 pages or PHP-powered websites, the payload may also be placed in the path.

For a detailed explanation of data flows between sources and sinks, see [DOM-based vulnerabilities](#).

---

## 🔍 Testing HTML Sinks

To test for DOM XSS in HTML sinks:

1. Insert a random alphanumeric string into the source (e.g., `location.search`).
2. Use browser developer tools to inspect the HTML and locate where your string appears.
   - Note: The browser's "View Source" option is not suitable for testing DOM XSS, as it does not reflect JavaScript-induced changes.
   - In Chrome Developer Tools, use `Ctrl+F` (or `Command+F` on macOS) to search for your string in the DOM.

3. For every location where your string appears in the DOM:
   - Determine the context.
   - Refine your input to understand how it is processed. For example, if the string appears in an attribute enclosed in double quotes, try injecting double quotes into the input to test whether you can break out of the attribute.

4. Note that browsers handle URL encoding differently:
   - Chrome, Firefox, and Safari encode `location.search` and `location.hash`, whereas IE11 and pre-Chromium Microsoft Edge do not.
   - If the data is encoded before processing, an XSS attack is less likely to succeed.

---

## 🔬 Testing JavaScript Execution Sinks

Testing JavaScript sinks for DOM XSS is more complex. The input may not appear in the DOM, so you need to use a JavaScript debugger to trace how the data is handled.

1. For each potential source (e.g., `location`):
   - Search for mentions of the source in the page's JavaScript code using Chrome Developer Tools (`Ctrl+Shift+F` or `Command+Alt+F` on macOS).

2. When you find where the source is read:
   - Add a breakpoint in the debugger and trace how the source value is used.
   - Monitor variables assigned the source value and look for their use in sinks.

3. When you find a sink receiving data from the source:
   - Check the variable's value using the debugger by hovering over it to see the data before it is passed to the sink.
   - Refine your input to test whether you can successfully perform an XSS attack.

---

## ⚙️ Testing DOM XSS with DOM Invader

Detecting and exploiting DOM XSS in real-world conditions can be time-consuming, requiring manual analysis of complex, minified JavaScript. However, if you use the Burp browser, you can leverage the built-in DOM Invader extension, which automates much of the process.

For details, see [DOM Invader documentation](#).

---

# DOM-XSS Vulnerabilities: Common Channels

DOM-XSS vulnerabilities can arise from unauthorized access to Document Object Model (DOM) objects. Below are the primary sinks that can lead to such vulnerabilities:

## Common Sources of Vulnerabilities:
- **`document.write()`** ✍️
  - Adds HTML content to the document, and if given data from an untrusted source, it may lead to vulnerabilities.

- **`document.writeln()`** 📜
  - Similar to `document.write()`, but adds a new paragraph after the data, which can also be exploited.

- **`document.domain`** 🌐
  - Changing the domain can allow scripts from other sources to interact with the DOM.

- **`element.innerHTML`** 🏷️
  - Setting inappropriate HTML content can execute malicious JavaScript.

- **`element.outerHTML`** 📄
  - Similar to `innerHTML`, but can replace the element itself, increasing the risk.

- **`element.insertAdjacentHTML`** ➕
  - Inserts HTML into the document; improper use may make it vulnerable.

- **`element.onevent`** 🔗
  - Binds an event handler to an event, which can be exploited to execute malicious code.

## jQuery Functions Leading to Vulnerabilities:
Certain jQuery functions can also act as sinks for potential DOM-XSS vulnerabilities:
- **`add()`** ➕
- **`after()`** 🪄
- **`append()`** 📥
- **`animate()`** 🎨
- **`insertAfter()`** 📝
- **`insertBefore()`** ✏️
- **`before()`** ⏮️
- **`html()`** 📃
- **`prepend()`** 🥇
- **`replaceAll()`** 🔄
- **`replaceWith()`** 🔄
- **`wrap()`** 🎁
- **`wrapInner()`** 🎊
- **`wrapAll()`** 🎀
- **`has()`** 💼
- **`constructor()`** 🛠️
- **`init()`** ⚙️
- **`index()`** 📊
- **`jQuery.parseHTML()`** 🛠️
- **`$.parseHTML()`** 📄

These methods may lead to vulnerabilities if the data passed to them can be influenced by attackers. Developers must always use secure and validated methods for interacting with the DOM to minimize risks. 💪🔥

---

## 💡 Tips for Preventing DOM XSS

1. **Minimize Use of Dangerous Sinks**:
   - Avoid using `eval()`, `innerHTML`, and similar constructs. Instead, use safer alternatives like `textContent` or `setAttribute`.

2. **Filter Input Data**:
   - Validate input on both the server and client. Use whitelists of allowed values.

3. **Encode Output Data**:
   - For HTML contexts, use encoding functions such as `encodeURIComponent` or specialized libraries.

4. **Implement a Content Security Policy (CSP)**:
   - Enforce a strict CSP to restrict JavaScript execution to trusted sources.

5. **Conduct Regular Security Testing**:
   - Use automated scanning tools and manual testing to identify XSS vulnerabilities.

6. **Train Developers**:
   - Increase team awareness of XSS risks and prevention techniques.
