# Username Enumeration Via Account Lock

## Challenge Information
- **Category**: Authentication
- **Level**: Practitioner (Level 2)

## Initial Testing
- After opening the target website, I tried to login with an incorrect username around 12 times with different incorrect passwords but the account or IP didn't get blocked.
- This shows that the login will not get blocked on any random incorrect username rather it will get locked if I try to attempt incorrect passwords with a correct username.
- This is logical because they will block a username that they know exists and can login, and with too many login tries, someone else is trying to login with their account.

## First Attempt
- I also brute forced username with the given list of the usernames and got a username against which it took too long to respond as compared to others.
- I brute forced passwords with this username but did not get any correct password that shows that the chosen username was also not a valid one.

## Attack Strategy
- Now I remember that I studied different types of attacks used by `intruder` in `burpsuite`. The type that we typically use is `Sniper` through which we can brute force a single payload.
- We had also used `pitchfork` to conduct attack with more than one payloads at a time but it takes the payload values in a parallel manner.
- Another attack `cluster bomb` is used that uses all the possible combinations of the given values of the payloads.
- After reviewing the pattern for this attack type, I used this attack for username and password that conducted an attack for 10100 combinations for 101 usernames and 100 passwords.
- But I did not let the attack complete because it seemed to be too much inefficient. So I studied a little solution of the lab.

## Refined Approach
- From the solution I got a little hint to conduct proper attack according to the present condition.
- It suggested to add null payloads in place of passwords rather than adding the complete list of the passwords, and add a value of 5 payloads for each username.
- This made 505 different combinations in which each username will try to login 5 times with an empty password.
- The purpose of this was to check that against which username it says too many login attempts and blocks the account.
- When the attack finished, I did not observe any change in the responses of any of the username. Some usernames took a bit long in their requests, but in short, the mission did not accomplish.
- I read complete instructions of the solution of the guide but none happened according to that. There was no response containing the error message as they told that should be.

## Final Solution
- So understanding the scenario, I prepared another logic. I decided to make a file of usernames with every username being repeated 5 times.
- I used this file for the brute force attack with a too long password so that if there is a valid username exists, all the five requests of it will take long.
- Even if it does not give the required error message, the username will be highlighted by its response time.
- This attack was now successful. Although the results were not as I thought, i.e. I got the username differentiated by the response error message rather than the response time. The response time was normal like all the other requests.
- Using this username, I conducted a brute force on the password using the given file for the passwords.
- The column of the error messages contained different messages. In start, it gave `invalid username and password` error, then after 3 failed attempts it gave the error to `try again after 1 minute`.
- But in the same attack when the correct password appeared, there was no error message indicating that it has logged in using that password.

## Vulnerability
**Result**: The flaw was that the error message changed for the valid username only that indicated the correct username.
