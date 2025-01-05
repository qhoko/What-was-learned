# 💻 Exploiting DOM XSS with Various Sources and Sinks

A website is vulnerable to DOM-based cross-site scripting (XSS) if there is an executable path that allows data to flow from a **source** to a **sink**. In practice, sources and sinks differ in their properties and behavior, which affects exploitability and determines the necessary methods.

Additionally, the website's scripts might perform validation or data processing, which should be taken into account when attempting to exploit the vulnerability. There are numerous sinks relevant to DOM-based XSS vulnerabilities. See the details in the list below.

---

## 🎯 Sink `document.write`

The `document.write` sink works with `script` elements, so you can use a simple payload, such as:

```javascript
document.write('... <script>alert(document.domain)</script> ...');
```

### 🔧 Example Vulnerability

1. This lab contains a DOM-based cross-site scripting vulnerability in the search query tracking function.
2. It uses the JavaScript function `document.write` to write data to the page.
3. The function is called with data from `location.search`, which can be controlled via the website's URL.

To solve this lab, perform an attack using cross-site scripting that triggers the `alert` function.

---

## ⚠️ Important Note

In some cases, the content written by `document.write` includes surrounding context that must be considered in your exploit. For example, you may need to close existing elements before injecting your JavaScript payload.

---

## 🧪 Steps to Exploit Stock Check Functionality

1. **Navigate to any product page** and observe the stock check functionality. In the code, note that JavaScript extracts the `storeId` parameter from the `location.search` source and uses it with `document.write` to create a new option in the `select` element:

   ![image](https://github.com/user-attachments/assets/678c0b7f-52a7-451d-94e9-1c91bac53d97)

2. **Add the `storeId` parameter to the URL** and input a random alphanumeric string as its value:

   ![image](https://github.com/user-attachments/assets/e425dd17-87ef-46be-b89b-534869423a7e)

3. **Request this modified URL** and notice that your random string appears as one of the parameters:

   ![image](https://github.com/user-attachments/assets/4b4ed25f-f08a-4d4b-be3f-d078b07e4af2)

4. **Inject the following payload into the `storeId` parameter** to trigger `alert`:

   ```html
   <script>alert('eyes')</script>
   ```

---

## 🎯 Sink `innerHTML`

The `innerHTML` sink does not accept `script` elements in modern browsers, and `onload` events in `svg` will not trigger. Therefore, you need to use alternative elements like `img` or `iframe`. Event handlers like `onload` and `onerror` can be used with these elements. For example:

```javascript
element.innerHTML='... <img src=1 onerror=alert(document.domain)> ...';
```

### 🔧 Example Vulnerability

1. This lab contains a DOM-based cross-site scripting vulnerability in the blog search functionality. It uses an `innerHTML` assignment that modifies the HTML content of a `div` element using data from `location.search`.

   ![image](https://github.com/user-attachments/assets/8240629b-031c-4a4b-be90-71d354028f4a)

2. Input the following payload in the search field:

   ```html
   <img src=1 onerror=alert(1)>
   ```

3. The `src` attribute value is invalid, which triggers an error. This invokes the `onerror` event handler, which calls the `alert()` function. As a result, the payload executes whenever a user's browser loads the page containing your malicious post.

---

## 🚀 Exploitation Tips

- Ensure the payload is correctly injected into the HTML structure.
- Use browser developer tools to analyze the DOM and confirm the vulnerability.
- Experiment with different sinks to find the most effective method.

---

🎉 Good luck mastering DOM XSS exploitation!
