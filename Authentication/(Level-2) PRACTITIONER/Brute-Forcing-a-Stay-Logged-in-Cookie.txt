# Brute-forcing a stay-logged-in cookie

## Challenge Information

* **Category**: Authentication
* **Level**: Practitioner (Level 2)

## Initial Analysis

I started by logging into my account and ticking the **Stay logged in** option. When Burp captured the response I noticed a `stay-logged-in` cookie had been set. I opened the Inspector, saw the cookie was Base64-encoded, and decoded it — the value was `wiener:51dc30ddc473d43a6011e9ebba6ca770`. The second part looked like an MD5 hash (based on its length and character set), and since the first part was my username I figured the second part was probably `md5(password)`. I hashed my own password with MD5 and it matched, so I concluded the cookie format was:

```
base64(username + ':' + md5(password))
```

## Request Analysis

I logged out so I could attempt to reproduce the flow from a fresh state. Then I found the most recent `GET /my-account?id=wiener` request in the proxy history. I highlighted the `stay-logged-in` cookie in that request and sent it to Intruder — Burp helpfully turned the cookie into a payload position automatically.

## Attack Execution

To confirm my transformation chain worked, I first added my own password as a single payload in Intruder. I then set up a sequence of **Payload Processing** rules so that each candidate password would be turned into a valid cookie before the request was sent:

1. I added a **Hash: MD5** rule so the password would be hashed.
2. I added an **Add prefix: `wiener:`** rule so the hash became `wiener:<md5>`.
3. I added an **Encode: Base64-encode** rule to produce the final cookie value.

I also added a grep match in the Settings panel for the string `Update email` — that button only appears when the My account page loads in an authenticated state, so it gave me a reliable way to tell which responses meant a successful login. I ran the attack and watched the results: one of the payloads triggered a response containing `Update email`, confirming the processed payload successfully recreated my stay-logged-in cookie.

### Retargeting the attack for Carlos

Once I confirmed the process, I swapped things to target another user:

* I removed my password from the payload list and loaded the candidate password wordlist instead.
* I changed the `id` parameter in the request URL from `wiener` to `carlos`.
* I updated the **Add prefix** rule to `carlos:` so the generated cookies would apply to Carlos’s account.
* And the most important one, I removed the value of **session** variable in the request content.

I ran the attack again and, when it finished, exactly one request returned a response containing `Update email`. That payload was the valid stay-logged-in cookie for Carlos’s account and the lab was solved.

## Vulnerability

**Result**: The cookie is essentially an unsalted MD5 of the password combined with the username, it’s possible to generate valid cookies for other users by hashing candidate passwords, prefixing the target username, and Base64-encoding the result.
