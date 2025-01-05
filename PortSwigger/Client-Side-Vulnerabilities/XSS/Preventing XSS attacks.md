## 🛡️ How to Prevent XSS

In this section, we will outline some general principles for preventing cross-site scripting (XSS) vulnerabilities and discuss how to use various common technologies to defend against XSS attacks.

### Two Levels of Protection

Preventing XSS is typically achieved through two levels of protection:

1. **Encode Output Data**
2. **Validate Input Data**

You can use **Burp Scanner** to scan your websites for numerous security vulnerabilities, including XSS. Burp's advanced scanning logic emulates the actions of an experienced attacker and achieves high XSS vulnerability coverage. Use Burp Scanner to ensure your XSS defenses are effective.

---

### 1. Encode Output Data

Encoding should be applied directly before writing user-controlled data to a page, as the context in which you write determines the type of encoding needed. For example, values inside a JavaScript string require different escaping compared to those in an HTML context.

- **HTML Context:** Convert untrusted values into HTML entities:
  - `<` becomes `&lt;`
  - `>` becomes `&gt;`

- **JavaScript String Context:** Non-alphanumeric values should be escaped using Unicode:
  - `<` becomes `\u003c`
  - `>` becomes `\u003e`

Sometimes, multiple layers of encoding are required in the correct order. For instance, to safely embed user input into an event handler, you must handle both JavaScript and HTML contexts. First escape the input into Unicode, then encode it into HTML:

```html
<a href="#" onclick="x='This string needs two layers of escaping'">test</a>
```

---

### 2. Validate Input Data

Encoding is likely the most crucial line of XSS defense, but it is insufficient for every context. You should also validate input data as strictly as possible when it is first received from the user.

Examples of input validation include:

- If the user submits a URL that will be returned in responses, ensure it starts with a safe protocol like `http` or `https`.
- If the user enters a value expected to be numeric, ensure it contains only valid integers.
- Restrict inputs to expected characters.

#### Whitelists vs. Blacklists

Input validation should generally use whitelists rather than blacklists. For example, instead of listing all malicious protocols (e.g., `javascript`, `data`), allow only safe protocols (e.g., `http`, `https`). This ensures your protection remains effective even as new attack vectors emerge.

---

### Allowing "Safe" HTML

Avoid allowing users to post HTML markup if possible, but if it is a business requirement, use libraries like **DOMPurify** to filter and encode HTML in the browser. Alternatively, allow users to write content in **Markdown**, converting it to HTML server-side. Be aware that all such libraries occasionally have XSS vulnerabilities, so keep them updated.

> **Note**: Beyond JavaScript, malicious content like CSS and plain HTML can also be harmful.

---

### Preventing XSS Using Templating Engines

Modern web applications often use server-side templating engines, such as Twig or Freemarker, to embed dynamic content into HTML. Most of these engines have built-in escaping mechanisms. For example, in Twig:

```twig
{{ user.firstname | e('html') }}
```

Frameworks like Jinja or React automatically escape dynamic content, effectively preventing most XSS cases. Ensure you understand the escaping features of the templating engine or framework you use.

> **Note**: Directly concatenating user input into template strings can lead to server-side template injection, often more severe than XSS.

---

### Preventing XSS in PHP

In PHP, use the built-in `htmlentities` function to encode entities for HTML contexts. Call it with three arguments:

1. The input string.
2. `ENT_QUOTES` to encode all quotes.
3. The character set, typically `UTF-8`.

Example:

```php
<?php echo htmlentities($input, ENT_QUOTES, 'UTF-8'); ?>
```

For JavaScript strings, escape input into Unicode as PHP does not provide a built-in API for this. Use a custom function:

```php
function jsEscape($str) {
    $output = '';
    foreach (str_split($str) as $char) {
        $chrNum = ord($char);
        if (!ctype_alnum($char)) {
            $output .= sprintf('\\u%04x', $chrNum);
        } else {
            $output .= $char;
        }
    }
    return $output;
}
```

Usage:

```php
<script>x = '<?php echo jsEscape($_GET['x']) ?>';</script>
```

---

### Preventing XSS on the Client-Side in JavaScript

To encode user input in HTML contexts within JavaScript, use a custom HTML encoder:

```javascript
function htmlEncode(str) {
    return String(str).replace(/[^\w. ]/gi, c => `&#${c.charCodeAt(0)};`);
}
```

For JavaScript strings, escape input into Unicode:

```javascript
function jsEscape(str) {
    return String(str).replace(/[^\w. ]/gi, c => `\\u${('0000' + c.charCodeAt(0).toString(16)).slice(-4)}`);
}
```

Usage:

```javascript
<script>document.body.innerHTML = htmlEncode(untrustedValue);</script>
```

---

### Mitigating XSS with Content Security Policy (CSP)

CSP acts as a last line of defense against XSS. Deploy it by including the `Content-Security-Policy` HTTP response header with your policy. Example:

```
default-src 'self'; script-src 'self'; object-src 'none'; frame-src 'none'; base-uri 'none';
```

This policy allows resources only from the same origin as the main page. Even if an attacker injects an XSS payload, their ability to load resources is severely restricted.
