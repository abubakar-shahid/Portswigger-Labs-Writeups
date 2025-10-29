# Password Brute-Force via Password Change

## Challenge Information

* **Category**: Authentication
* **Level**: Practitioner (Level-2)

## Initial Analysis

Burp was left running while the password-change flow was explored. Submitting the form revealed the username is sent as a hidden input, and the server’s error messages vary depending on what’s submitted. Notably:

* Submitting a wrong current password with two matching new passwords results in the account being locked.
* Submitting a wrong current password with two different new passwords returns **Current password is incorrect**.
* Submitting a *valid* current password but two different new passwords returns **New passwords do not match**.

That differing behavior can be abused as an oracle to enumerate a correct current password for another account.

## Request Analysis

A normal password-change POST looks like:

```
POST /my-account/change-password
username=<username>&current-password=<current>&new-password-1=<p1>&new-password-2=<p2>
```

To turn the differing server messages into an enumeration channel, the plan is to submit requests for the target user where `current-password` is replaced with candidate passwords, while keeping the two new-password fields intentionally different. If the server responds with **New passwords do not match**, that indicates the supplied current password was accepted — and is therefore the correct password.

## Attack Execution

1. The password-change request was sent to Intruder after confirming the form submissions in the Proxy history.
2. The `username` parameter was changed to `carlos` in the request, and a payload position was placed around the `current-password` value. The two new-password fields were set to different static values, for example:

   ```
   username=carlos&current-password=§candidate-password§&new-password-1=123&new-password-2=abc
   ```
3. The password wordlist was loaded as the payload set in Intruder’s Payloads panel.
4. In Settings, a grep match rule for the string **New passwords do not match** was added so responses containing that text would be flagged.
5. The attack was started and monitored. When it finished, a single response contained **New passwords do not match** — that payload value was noted as the correct current password for Carlos.
6. Finally, logging out and then logging back in as `carlos` with the identified password confirmed access; visiting **My account** showed the lab was solved.

## Vulnerability

**Result**: The application leaks password validity through distinct error messages in the password-change flow. It also do not validate whether the password change request is from logged-in user or not.
