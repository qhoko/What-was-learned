# Lab Work: Using Path Matching to Bypass Web Cache 😎

---

## Description 📝
In this lab work, you will learn to exploit discrepancies between the web cache and the origin server to obtain the API key for the user **carlos**.

---

## Objectives 🎯
- 👉 Learn how to log into the account using the provided credentials.
- 👉 Familiarize yourself with path matching and its use for accessing resources.
- 👉 Study the technical aspects of web caching and how to bypass it.

---

## Credentials 🔑
To log into the account, use the following credentials:
- **Username:** `wiener`
- **Password:** `peter`

---

## Theoretical Background 📚

### URL Path Matching 🛤️
- 🔍 **API Endpoints** can use regular expressions to match URL paths with resources.
- 🔍 It's important to understand how different paths may be handled differently depending on the cache and server configurations.

### Caching and Its Discrepancies 🔄
- 🔄 The web cache and the server may have different mechanisms for handling the same requests.
- 🔄 Cache bypassing is done by changing the request path to access non-cached data on the server.

---

## Steps to Perform 📋

### 1. Logging In 🔐
After logging in using the provided credentials, you will see your **API key**:
![API Key](https://github.com/user-attachments/assets/aaa6811d-e722-4a6c-8d55-b6607a0ecd1d)

---

### 2. Analyzing Endpoints 🔍
We explore the available API endpoints to determine how they are processed by the server.
- 🔍 **Pay attention to paths** that may be vulnerable to bypassing. Run **Burp Suite** and capture requests by refreshing the page at `/my-account`.
- ✉️ **Send the request to Repeater** and change the path to an arbitrary one:
![Change Path](https://github.com/user-attachments/assets/3712184f-56d3-47ca-b684-0b541227e483).  
  _We can also see our API, and the length of the request remains the same, indicating that the server abstracts the URL path to `/my-account`._

---

### 3. Using Static Extensions 🔄
We check how static extensions affect the website's behavior.
- 📄 **Add a static extension**, for example, `eyes.js`:
![Static Extension](https://github.com/user-attachments/assets/79cc4583-9554-4137-9d00-9b246c8d04d5).
- 📊 **Note the response headers**:
  - `X-Cache: miss` — indicates that the response was not served from the cache.
  - `Cache-Control: max-age=30` — implies that if the response was cached, it should be stored for 30 seconds.

---

### 4. Testing Cache Bypass 🛡️
Try modifying the request path so that it does not match cached data, thereby accessing the required information.
- 🔄 **Send the repeat request**, and the value of the `X-Cache` header will change to `hit`:
![Cache Bypass](https://github.com/user-attachments/assets/a8af7be3-e0ae-4a3b-b929-c6df93a62220).  
  _This shows that the response was returned from the cache._
- 📈 **Conclusion**: The cache interprets the URL path as `/my-account/eyes.js`, and caching occurs correctly based on the `.js` static extension.
- 💻 Use this payload for the exploit.

---

### 5. Obtaining the API Key 🗝️
Determine the API key for the user **carlos**.
- 🛠️ On the exploit server, proceed to create an exploit.
- 📜 In the **Body** section, create an exploit that redirects the victim `carlos` to a malicious URL:
- 🚀 Click "Deliver the exploit to the victim". When the victim views the exploit, the received response gets cached.
- 🌐 Go to the URL you specified in your exploit: `https://YOUR.LAB.LINK/my-account/eyes.js`.
- 🔑 The response will include the API key for **carlos**:
![API Key for carlos](https://github.com/user-attachments/assets/1199ea79-842b-4668-8567-06ace1af1137).

---

## Conclusion 💡
As a result of completing this lab work, you will be able to access protected resources using path matching and cache bypassing skills. This knowledge will be useful in your future projects and career. 🚀
