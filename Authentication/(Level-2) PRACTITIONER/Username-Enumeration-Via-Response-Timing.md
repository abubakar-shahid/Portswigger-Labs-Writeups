# Username Enumeration Via Response Timing

## Challenge Information
- **Category**: Authentication
- **Level**: Practitioner (Level 2)

## Initial Testing
- According to problem statement, the lab is vulnerable to username enumeration using its response times.
- When i entered a random username and password, it gave me the error message `Invalid username or password.`

## IP Blocking Challenge
- So i decided to brute force the username to compare the responses of different input values.
- Now the problem was that my IP was being blocked after each 5 attempts. The page displayed a message to `try again after 30 minutes`.
- To tackle this issue, i came to know that we can spoof our IP by adding a `X-Forwarded-For: 123.45.67.89` header.
- But the issue was that we had to change the IP after each 5 failed attempts.

## Attack Strategy
- So i decided to use a list of unique random IP addresses, one for each request.
- For this, i created a list of 101 random IP addresses since our usernames were also 101.
- Then i added `X-Forwarded-For: §123.45.67.89§` at the line number 3.
- I also added this for username to create multiple payloads for a single attack.
- I changed the attack type from `sniper` to `pitchfork` to support multiple payloads.

## First Attempt
- After adding lists for both IP addresses and the usernames, i started the attack again.
- An important point to be noted is that whenever you try to brute force through `intruder` in `burpsuite`, you should have your proxy server on.
- However, i performed the attack and found that there was one username against which the response time was very large as compared to the others.
- Considering this username as valid username, now i started a brute force on the passwords list.
- But the response of this attack was same. There was no difference in any attribute of the response.
- So i decided to brute force the username again since the previous username did not work.
- This attack also did not give any significant response.

## Refined Approach
- I did the same attack after taking some hints from the solution. The hint was that enter the password too long i.e. min 100 characters. This will increase the response time of valid username because the server will start checking that long password after validating the valid username.
- Perform the attack 3-4 times to verify that the username that is taking long than others, every time takes more time than others.
- After performing the username attack 3 times, i got a username that was taking long everytime. So i started the password attack on that username.
- On the password attack, i got a `302` response and no error message from one password against that guessed username which meant that i have got the correct credentials.

## Vulnerability
**Result**: Now the error message was also exactly same for both the username as well as password and the login was also getting blocked after each 5 attempts. But, we came out with a solution by spoofing the IP and checking the correct username by entering long password field which created a good difference in the response time of correct username. For the password, it was simple change of status code to `302`.
