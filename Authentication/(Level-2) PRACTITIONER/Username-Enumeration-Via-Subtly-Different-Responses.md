-> first of all i opened the target website.
-> now in this task, it was given that the website is subtly vulnerable to username enumeration and password brute-force attacks.
-> so when i entered a random username and passoword, it gave me the error 'Invalid username or password.'
-> now i decided to brute force the username first to observe that would there be any change in the output string format.
-> but there was no difference in the response code or packet length or any timing.
-> now after getting some hint from the solution, i came to know that we can highlight the error message in the responses that will indicate that in which response the error message is diferent.
-> the setting was done in the following steps:
	=> On the Settings tab, under Grep - Extract, click Add. In the dialog that appears, scroll down through the response until you find the error message Invalid username or password.. Use the mouse to highlight the text content of the message. The other settings will be automatically adjusted. Click OK and then start the attack.
-> after giving the starting and the endpoint of the error message in Grep - Extract, I started the attack.
-> now in the attack result, there is an extra column for the warning message. i sorted this column and observed the change in the sequence o fthe responses arranged that gave me the valid username.
-> the difference was that there was no full stop at the end of this warning message.
-> then i appliend the same technique on the password. i applied the same settings for the attack and now using the latest warning message.
-> now this time, the there was no warning message for the correct password and the cell in the warning message column was empty for it.
-> now I had the correct username as well as the password, which allowed me to complete the lab.

Result: The message for the incorrect username as well password was the same to avoid giving any hint to the attacker. But, the difference came up in the fullstop. On entering incorrect username, there was a fullstop at the end of the errror message. While there was no fullstop at the end when the username was incorrect and the password was inorrect only.