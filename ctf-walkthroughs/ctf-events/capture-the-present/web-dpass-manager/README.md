# Description

The coolest password manager in town, DPass Manager, is soon going into production. They have provided you with a mock build of their web application and asked you to review it. Can you take a look to see if it's secure?

# Recon

After going through source code I found the app is using `"jsonwebtoken":"^8.5.1"` which is vulnerable to [CVE-2022-23529](https://unit42.paloaltonetworks.com/jsonwebtoken-vulnerability-cve-2022-23529/)

