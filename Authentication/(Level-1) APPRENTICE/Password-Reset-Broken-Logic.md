# Password Reset Broken Logic

## Challenge Information

* **Category**: Authentication
* **Level**: Apprentice (Level-1)

## Initial Analysis

I started by exploring the password reset flow while Burp was running. I clicked **Forgot your password?**, entered my username and used the **Email client** button to view the reset email. The email contained a link with a `temp-forgot-password-token` query parameter — a classic reset token pattern. My initial thought was that the token would be required to authorize the password change, so I wanted to confirm how strictly the application validated it.

## Request Analysis

I followed the reset link in the email and changed my password to something I controlled. Then I opened **Proxy → HTTP history** in Burp and examined the requests involved in the reset flow. I noticed the POST that submits the new password was made to:

```
POST /forgot-password?temp-forgot-password-token=<token>
```

and that the request body also included a hidden `username` field and the new password. That suggested the server was receiving both the token (in the URL) and the username (in the form). I sent that POST request to Repeater so I could test variations.

## Attack Execution

I wanted to see whether the token was actually required on submission:

1. In Repeater, I removed the value of the `temp-forgot-password-token` parameter from both the URL and the request body and then submitted the request. To my surprise, the password reset still worked — the server accepted the new password even without the token. That told me the token was only being used in the link generation step (email delivery), but not validated when the password was actually changed.

2. To further verify the flaw, I went back to the browser and requested a fresh password reset for my account so I had a new valid flow captured in Burp. I sent the new POST `/forgot-password?temp-forgot-password-token=<token>` request to Repeater again.

3. In Repeater, I stripped the `temp-forgot-password-token` value from the URL and body once more, changed the `username` parameter to `carlos`, and set the new password to a value I chose. I submitted that request.

4. Finally, I opened the browser and logged in as Carlos using the new password I had set. I navigated to **My account** and confirmed I was authenticated as Carlos — the lab was solved.

## Vulnerability

**Result**: The application do not validate the password reset token when the new password is submitted.
