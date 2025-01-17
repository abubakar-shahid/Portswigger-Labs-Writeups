-> opened the lab which redirected me to a website.
-> there was a page of 'my account'.
-> on opening that page, it asked for username and password to login.
-> since i did not know both the credetials, so i eneterd random username and random password.
-> on submitting the form, it indicated error 'invalid username'. this indicated that the username is incorrect.
-> so i used burpsuite to capture the request.
-> using the intruder tab, i brute forced the username by using the list of usernames given as helping material in the lab manual.
-> on brute forcing all the given usernames, i observed the correct username by the paket size of the response packet since reponse code for both correct and incorrect username was the same.
-> after getting the correct username, now when i entered the correct username with a random password, no it gave me error 'invalid password'.
-> now i again used the brute force from the intruder using the passwords list given in the lab manual.
-> again in the result, i observed the correct password through the packet size of the response packets.
-> now I had the correct username as well as the password, which allowed me to complete the lab.

Result: The problem was in the response message. The reponse message for incorrect username and password were different that gave hint to the attacker. Moreover, the difference in the response of correct and incorrect came to be due to the difference in the response size.