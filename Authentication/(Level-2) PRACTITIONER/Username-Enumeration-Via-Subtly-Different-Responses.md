# Username Enumeration Via Subtly Different Responses

## Challenge Information
- **Category**: Authentication
- **Level**: Practitioner (Level 2)

## Initial Testing
- First of all, I opened the target website.
- Now in this task, it was given that the website is subtly vulnerable to username enumeration and password brute-force attacks.
- So when I entered a random username and password, it gave me the error `Invalid username or password.`

## Investigation Steps
- Now I decided to brute force the username first to observe that would there be any change in the output string format.
- But there was no difference in the response code or packet length or any timing.
- Now after getting some hint from the solution, I came to know that we can highlight the error message in the responses that will indicate that in which response the error message is different.

## Attack Configuration
The setting was done in the following steps:
1. On the `Settings` tab, under `Grep - Extract`, click `Add`.
2. In the dialog that appears, scroll down through the response until you find the error message `Invalid username or password.`
3. Use the mouse to highlight the text content of the message. The other settings will be automatically adjusted.
4. Click `OK` and then start the attack.

## Attack Execution
- After giving the starting and the endpoint of the error message in `Grep - Extract`, I started the attack.
- Now in the attack result, there is an extra column for the warning message. I sorted this column and observed the change in the sequence of the responses arranged that gave me the valid username.
- The difference was that there was no full stop at the end of this warning message.
- Then I applied the same technique on the password. I applied the same settings for the attack and now using the latest warning message.
- Now this time, there was no warning message for the correct password and the cell in the warning message column was empty for it.
- Now I had the correct username as well as the password, which allowed me to complete the lab.

## Vulnerability
**Result**: The message for the incorrect username as well as password was the same to avoid giving any hint to the attacker. But, the difference came up in the full stop. On entering an incorrect username, there was a full stop at the end of the error message. While there was no full stop at the end when the username was correct and the password was incorrect only.
