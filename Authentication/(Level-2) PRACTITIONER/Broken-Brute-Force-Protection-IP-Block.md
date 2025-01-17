                            ----------------------------------------------------------------------------------------------------Notes----------------------------------------------------------------------------------------------------
Logic Flaw: The protection mechanism likely has a flaw. For instance:
It could be blocking the IP after several failed attempts without distinguishing between different users.
It may also reset the block after a very short period or not count failed attempts across different IP addresses.
This means that it might be possible to rotate IPs or alternate between the user accounts (like wiener and carlos) to avoid getting blocked, allowing you to continue brute-forcing carlos's password.

                          ----------------------------------------------------------------------------------------------------Solution----------------------------------------------------------------------------------------------------
-> i opened the target machine. since the username was given, i entered that username and added a random password which gave me password incorrect message
-> i also gave an incorrect password and username both on which it gave me incorrect username error
-> after having a brute force on the password, after a few attempts, it gave me error to try again after 1 minute
-> then i conducted an attack with the list of ips that i used in the previous lab as well as with random ips through the number type payload set but both the methods did not work
-> i researched some on this and i came to know that we should manipulate other headers as well so i also brute forced random cookie similar to the original cookie
-> but this also did not work. then i got some hints from the solution and read the problem statement again carefully that suggested that the count to reset the login attempts resets when we enter a valid username and password
-> so to brute force the password again with this technique, i changed the file for username to valid username and invalid username 101 times each alternatively
-> similarly, i changed the passwords file with the valid password alternatively after each random password
-> through this technique, after each invalid attempt, there will be a valid attempt to reset the reset count which will prevent my ip from blocking
-> i removed all other filters and applied only on the username and password
-> by conducting this attack, i got the valid username and password for the target user

Result: The main flaw in this website was less number of counts for reseting the blocked ip.