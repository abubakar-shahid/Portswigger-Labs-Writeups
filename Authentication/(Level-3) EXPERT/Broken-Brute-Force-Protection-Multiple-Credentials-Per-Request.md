-> here, we were given the username of the victim and we only had to brute force the password to login into the target account
-> i read the problem statement carefully and also took help from gpt to understand it
-> the main point was that multiple requests could be sent in a single http request
-> to understand this point in detail, i analyzed the request format in which i noticed that the parameters being sent in the request were in json format
-> it means that we could add as many passwords in the password field of the json as we wanted
-> i took help from cursor ai to make a new json containing all the passwords in the given password file
-> then i replaced the new json with the json present in the request and forwarded the request
-> as a result, i successfully logged in into the account
-> but, i did not knew that which was the correct password from the json
-> to find that out, i used sniper to brute force the password through simple method but did not get the correct password in the response table
-> when i checked the solution of the manual, it also did not give any method to identify the password. the target was only to login to the account that i already had done

Result: The main flaw was in the sending of data in the json format