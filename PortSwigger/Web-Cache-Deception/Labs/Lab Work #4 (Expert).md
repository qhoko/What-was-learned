# 🌐 Lab Assignment: Using Exact Match Caching Rules to Bypass Web Cache 😎

---

## 📜 Description
In this lab assignment, we will study how to use exact match caching rules between the server and the web cache to change the email address for the user **administrator**.

---

## 🎯 Objectives
- 👉 Get acquainted with path mapping and its use for accessing resources.
- 👉 Learn the technical aspects of web caching and its circumvention possibilities.

---

## 🔑 Credentials
To log in to the account, use the following details:
- **Username:** `wiener`
- **Password:** `peter`

---

## 📚 Theoretical Background

### 🛤️ URL Path Mapping
- 🔍 **API endpoints** may use regular expressions to match URL paths to resources.
- 🔍 It is important to understand how different paths can be handled differently depending on the configuration of the cache and server.

### 🔄 Caching and Its Discrepancies
- 🔄 The web cache and server can have different mechanisms for handling the same requests.
- 🔄 Cache bypass is done by changing the query path to access uncached data on the server.

---

## 📋 Steps to Perform

### 🔐 1. Logging In
1. Log in using the provided credentials, then change your email address.
   ![image](https://github.com/user-attachments/assets/ee0498ee-2a13-42d1-bfbe-d7c09d95039a)
2. In the Proxy > HTTP History section, note that the form for submitting the email address change in the `/my-account` response contains a CSRF token as a hidden parameter.

---

### 🔍 2. Investigating Path Separator Discrepancies
1. Right-click on the GET `/my-account` request and select "Send to Repeater".
2. In Repeater, change the URL path to `/my-account/abc`, then send the request. Note the 404 Not Found response. This means that the origin server does not abstract the path to `/my-account`: 
   ![image](https://github.com/user-attachments/assets/d7c29692-e2e6-44de-9a1e-a5db82f14266)
3. Change the path to `/my-accountabc`, then send the request. Note that this returns a 404 Not Found response without any signs of caching: 
   ![image](https://github.com/user-attachments/assets/31e57b0f-8139-49db-8446-efdcbf5e04b4)

---

### 🔄 3. Using Delimiters in Intruder
Exploring how delimiters affect the site behavior.
1. Right-click on the request and choose "Send To Intruder" (or use Ctrl + I).
2. 📄 **Specify the Payload Location**:
   ![image](https://github.com/user-attachments/assets/f6128369-516e-44b4-a804-e145f593ff38).
3. Uncheck "URL-encode these characters".
4. 📊 **Add the List of Characters**: In the "Payload Configuration" section, add a list of characters that can be used as delimiters.
   Copy the list from here: [Delimiter List](https://portswigger.net/web-security/web-cache-deception/wcd-lab-delimiter-list).
   ![image](https://github.com/user-attachments/assets/f109678c-1d5f-4604-8970-97562d595b7c)
5. Click "Start attack". 
   In the output, you will see a 200 status code, indicating the processing of only two delimiters: `;`, `?`: 
   ![image](https://github.com/user-attachments/assets/4e216ecf-2138-4e0c-95ea-48fc88406419).
6. This indicates that they are used by the origin server as path delimiters.
7. Go to the Repeater tab containing the `/my-account/abc` request. Update the path to `/my-account?abc.js`, then send the request. Note that the response does not contain evidence of caching.
8. Repeat this test using the `;` symbol instead of `?`. Note that the response shows no signs of caching.

---

### 🛡️ 4. Exploit the Vulnerability to Find the Admin's CSRF Token
1. Use the `?` delimiter to attempt to create an exploit as follows: `/my-account?%2f%2e%2e%2frobots.txt`. Send the request. Note that it receives a 200 response but does not contain evidence of caching.
2. Repeat this test using the `;` delimiter instead of `?`. Note that it receives a 200 response with your API key and the `X-Cache: miss` header: ![image](https://github.com/user-attachments/assets/f2a08d85-3536-4a5c-967b-15722dd4b524). Resend and note that it updates to `X-Cache: hit`. This indicates that the cache normalized the path to `/robots.txt` and cached the response. You can use this payload for exploitation.
3. In the Burp browser, click “Go to the Exploit Server”.
4. In the Body section, create an exploit that redirects the victim user to the malicious URL you created. Be sure to add a random parameter as a cache buster:
<script>document.location="https://YOUR.LAB.LINK/my-account;%2f%2e%2e%2frobots.txt?eyes"</script>
6. Click “Deliver the exploit to the victim”.
7. Go to the URL you provided to the victim in your exploit: `https://YOUR.LAB.LINK/my-account;%2f%2e%2e%2frobots.txt?eyes`.
8. Note that the Burp browser redirects to the account login page. This may be due to the browser redirecting requests with invalid session data. Try to use the exploit in Burp.
9. Go to the Repeater tab containing the `/my-account` request. Change the path to reflect the URL you sent to the victim in your exploit. For example, `/my-account;%2f%2e%2e%2frobots.txt?eyes`.
10. Send the request. Ensure you do this within 30 seconds after delivering the exploit to the victim. If not, send the exploit again with a different cache buster.
11. Note that the response includes the CSRF token for the **administrator** user. Copy this.

---

### 🗝️ 5. Create Exploit
Determine the API key for the user **carlos**.
1. In the Proxy > HTTP History section, right-click on the POST `/my-account/change-email` request, and select "Send to Repeater".
2. In Repeater, replace the CSRF token with the admin's token.
3. Change the email address in your exploit so it does not match your own.
4. Right-click the request and select Interaction Tools > Generate CSRF PoC.
5. Click Copy HTML.
6. Paste the HTML code into the Body section of the exploit server.
7. Click “Deliver the exploit to the victim” to complete the lab.

---

## 💡 Conclusion
As a result of completing the lab assignment, you will be able to access protected resources using path mapping and cache bypassing skills. This knowledge will prove beneficial in your future projects and career. 🚀
