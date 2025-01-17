# Broken Brute Force Protection IP Block

## Challenge Information
- **Category**: Authentication
- **Level**: Practitioner (Level 2)

## Initial Analysis
### Logic Flaw
The protection mechanism likely has a flaw. For instance:
- It could be blocking the IP after several failed attempts without distinguishing between different users.
- It may also reset the block after a very short period or not count failed attempts across different IP addresses.
- This means that it might be possible to rotate IPs or alternate between the user accounts (like `wiener` and `carlos`) to avoid getting blocked, allowing you to continue brute-forcing `carlos's` password.

## Investigation Steps
- I opened the target machine. Since the username was given, i entered that username and added a random password which gave me `password incorrect` message.
- I also gave an incorrect password and username both on which it gave me `incorrect username` error.
- After having a brute force on the password, after a few attempts, it gave me error to `try again after 1 minute`.
- Then i conducted an attack with the list of IPs that i used in the previous lab as well as with random IPs through the number type payload set but both the methods did not work.
- I researched some on this and i came to know that we should manipulate other headers as well so i also brute forced random cookie similar to the original cookie.
- But this also did not work. Then i got some hints from the solution and read the problem statement again carefully that suggested that the count to reset the login attempts resets when we enter a valid username and password.

## Attack Method
- So to brute force the password again with this technique, i changed the file for username to valid username and invalid username 101 times each alternatively.
- Similarly, i changed the passwords file with the valid password alternatively after each random password.
- Through this technique, after each invalid attempt, there will be a valid attempt to reset the reset count which will prevent my IP from blocking.
- I removed all other filters and applied only on the username and password.
- By conducting this attack, i got the valid username and password for the target user.

## Vulnerability
**Result**: The main flaw in this website was less number of counts for resetting the blocked IP.
