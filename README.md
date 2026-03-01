# Authentication-bypass-via-encryption-oracle
"Cybersecurity enthusiast and Bug Bounty learner. Documentation of my journey through PortSwigger Academy, focusing on advanced web vulnerabilities and manual exploitation.”
## 1. Analysis

The application uses a **block cipher** to encrypt user information. By analyzing the notification system, I discovered it acts as an **encryption oracle**, allowing me to encrypt custom strings and observe the resulting ciphertext.

## 2. Crafting the Payload

To exploit the block alignment, I constructed a specific string to target the administrative session:

- **Prefix**: `xxxxxxxxxx` (Used to align the target string to a new 16-byte block).
- **Target String**: `administrator:your-timestamp`.
- **Final Payload**: `xxxxxxxxxxadministrator:1740767055734`.

## 3. Byte Manipulation (Burp Decoder)

As shown in my progress, I manipulated the raw encrypted data:

- **Initial State**: Decoded the URL and Base64 string to reveal the hex data.
- **Removal**: I selected and deleted the first **32 bytes** (2 full blocks of 16 bytes each).
- **Result**: This left only the encrypted block containing my administrative credentials.

## 4. Privilege Escalation

After deleting the bytes, I performed the following:

1. **Encoded** the remaining data back to **Base64**.
2. **Encoded** it again as a **URL** string.
3. Replaced the `stay-logged-in` cookie in **Burp Repeater**.
4. Accessed the `/admin` panel and successfully deleted the user `carlos`.

## 🛠 Tools Used

- **Burp Suite Community Edition** (v2025.12.4).
- **Burp Decoder** for hex-level manipulation.
