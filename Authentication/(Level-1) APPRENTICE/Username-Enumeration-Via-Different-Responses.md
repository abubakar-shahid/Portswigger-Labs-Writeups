# Username Enumeration Via Different Responses

## Challenge Information
- **Category**: Authentication
- **Level**: Apprentice (Level 1)

## Initial Investigation
- Opened the lab which redirected me to a website.
- There was a page of 'my account'.
- On opening that page, it asked for `username` and `password` to login.
- Since I did not know both the credentials, so I entered random `username` and random `password`.
- On submitting the form, it indicated error `invalid username`. This indicated that the `username` is incorrect.
- So I used `burpsuite` to capture the request.

## Attack Method
- Using the `intruder` tab, I brute forced the `username` by using the list of usernames given as helping material in the lab manual.
- On brute forcing all the given usernames, I observed the correct `username` by the packet size of the response packet since response code for both correct and incorrect `username` was the same.
- After getting the correct `username`, now when I entered the correct `username` with a random `password`, now it gave me error `invalid password`.
- Now I again used the brute force from the `intruder` using the passwords list given in the lab manual along with the correct `username`.
- Again in the result, I observed the correct `password` through the packet size of the response packets.
- Now I had the correct `username` as well as the `password`, which allowed me to complete the lab.

## Vulnerability
**Result**: The problem was in the response message. The response message for incorrect `username` and `password` were different that gave hint to the attacker. Moreover, the difference in the response of correct and incorrect came to be due to the difference in the response size.
