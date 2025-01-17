# Broken Brute Force Protection - Multiple Credentials Per Request

## Challenge Information
- **Category**: Authentication
- **Level**: Expert (Level 3)

## Initial Analysis
- Here, we were given the username of the victim and we only had to brute force the password to login into the target account.
- I read the problem statement carefully and also took help from `GPT` to understand it.
- The main point was that multiple requests could be sent in a single `HTTP` request.

## Request Analysis
- To understand this point in detail, I analyzed the request format in which I noticed that the parameters being sent in the request were in `JSON` format.
- It means that we could add as many passwords in the password field of the `JSON` as we wanted.
- I took help from `GPT` to make a new `JSON` containing all the passwords in the given password file.

## Attack Execution
- Then I replaced the new `JSON` with the `JSON` present in the request and forwarded the request.
- As a result, I successfully logged in into the account.
- But, I did not know which was the correct password from the `JSON`.

## Password Identification Attempt
- To find that out, I used `sniper` to brute force the password through simple method but did not get the correct password in the response table.
- When I checked the solution of the manual, it also did not give any method to identify the password. The target was only to login to the account that I already had done.

## Vulnerability
**Result**: The main flaw was in the sending of data in the `JSON` format.
