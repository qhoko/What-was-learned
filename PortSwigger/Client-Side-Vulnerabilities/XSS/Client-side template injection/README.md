## 🧩 Client-Side Template Injection

In this section, we will explore client-side template injection vulnerabilities and how you can use them for XSS attacks. This attack method was first applied by our research team—read more in "XSS Without HTML: Client-Side Template Injection Using AngularJS." While client-side template injection is a broad issue, we will focus on examples from the AngularJS framework, as it is the most common problem. We will describe how you can craft exploits that bypass the AngularJS sandbox and potentially use AngularJS features to bypass Content Security Policy (CSP).

### ❓ What is Client-Side Template Injection?

Client-side template injection vulnerabilities occur when applications using client-side template frameworks dynamically embed user input into web pages. During page rendering, the framework scans the page for template expressions and executes any it encounters. An attacker can exploit this by providing a malicious template expression that triggers a cross-site scripting (XSS) attack.

### 🛡️ What is the AngularJS Sandbox?

The AngularJS sandbox is a mechanism that prevents access to potentially dangerous objects, such as `window` or `document`, in AngularJS template expressions. It also blocks access to potentially dangerous properties, such as `__proto__`. Although the AngularJS team does not consider it a security boundary, the broader development community generally does. While escaping the sandbox was initially difficult, security researchers discovered many ways to do so. As a result, it was ultimately removed from AngularJS in version 1.6. However, many legacy applications still use older versions of AngularJS and may remain vulnerable.

### 🛠️ How Does the AngularJS Sandbox Work?

The sandbox works by parsing the expression, rewriting the JavaScript, and then using various functions to check whether the rewritten code contains any dangerous objects. For example, the `ensureSafeObject()` function checks if a given object references itself. This is one way to detect the `window` object, for example. The `Function` constructor is detected similarly by checking if the constructor property references itself.

The `ensureSafeMemberName()` function validates every property access on an object, blocking dangerous properties like `__proto__` or `__lookupGetter__`. The `ensureSafeFunction()` function prevents invoking `call()`, `apply()`, `bind()`, or `.constructor()`.

You can see the sandbox in action by visiting this script and setting a breakpoint at line 13275 of the `angular.js` file. The `fnString` variable contains your rewritten code, so you can observe how AngularJS transforms it.

### 🚪 How Does Escaping the AngularJS Sandbox Work?

Escaping the sandbox involves tricking the sandbox into believing a malicious expression is harmless. The most well-known escape uses a globally modified `charAt()` function within an expression:

```javascript
'a'.constructor.prototype.charAt = [].join;
```

When initially discovered, AngularJS did not prevent this modification. The attack works by overwriting the function using the `[].join` method, which causes the `charAt()` function to return all characters passed to it instead of a specific single character. Due to the logic of the `isIdent()` function in AngularJS, it compares what it considers a single character with multiple characters. Since single characters are always less than multiple characters, the `isIdent()` function always returns `true`, as shown in the following example:

```javascript
isIdent = function(ch) {
    return ('a' <= ch && ch <= 'z' || 'A' <= ch && ch <= 'Z' || '_' === ch || ch === '$');
};
isIdent('x9=9a9l9e9r9t9(919)');
```

Once the `isIdent()` function is tricked, you can inject malicious JavaScript. For example, an expression like `$eval('x=alert(1)')` will be allowed because AngularJS processes each character as an identifier. Note that you must use the AngularJS `$eval()` function because the `charAt()` function override takes effect only after isolated code execution. This method bypasses the sandbox and allows arbitrary JavaScript execution. PortSwigger Research has repeatedly broken the AngularJS sandbox.

### 🛡️ How to Prevent Client-Side Template Injection Vulnerabilities

To prevent client-side template injection vulnerabilities, avoid using untrusted user input to generate templates or expressions. If this is impractical, consider filtering template syntax from user input before embedding it into client-side templates.

Note that HTML encoding is insufficient to prevent attacks via client-side template injection, as frameworks perform HTML decoding of relevant content before detecting and executing template expressions.
