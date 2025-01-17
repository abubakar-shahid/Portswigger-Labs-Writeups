-> First of all, i opened the website after reading the detailed infromation about the topic before starting the lab.
-> The information highlighted the bypassing the username password area and then bypass the second login page using the session cookies of the correct login credentials and use that to create the 2FA code for the victim account.
-> Reading the information in detail gave me so much benefit that I found half of the lab solution in that information.
-> Anyhow, we were given correct username, correct password and a victim username. first i used correct credentials to login and check the overall process.
-> After that, i analyzed the request in the burpsuite to check the request pattern by entering corret username and correct password. this generated a code that was sent to the email of correct account.
-> The email could be accessed through a button that opened the emails of the account to be logged in.
-> I tried to login to the victims account by using this code since there is a field in the header of the request named 'verify' that contains usename of the user whose account is being logged in.
-> I changed the username to the victim's username and forwarded the request.
-> But this did not work because the code that i was trying to enter did not belonged to the victim.
-> Now i studied the solution of the lab that suggested that i had to first login with my own credentials to check the procedure.
-> Then again try to login to the account with correct credetials but when it tries to GET the second login page that actually generates the code and sends it to the main, we have to change the name in the verify field to the victims name so that it generates the code for the victims account.
-> This technique bypassed the password authentication procedure for the victim and generated the code for victim.
-> Now during the posting of the second login page where we have to submit the code, here we have to change the verfiy field again to the victim username and brute force the code since now there exists a code for the victims account.
-> This will get the code and you will be able to login to the victim account.

-> To conduct a brute force using external tools, first i used hydra in which i tried multiple commands by using gpt but unfortunately none of them was working. I was facing different types of errors for different commands. One of them is also given below:
	hydra -l carlos -P 4-digit-list.txt -s 443 -f -V -t 16 -w 1 -m /login2 0a9a00c304e6082d83d05f4800d500b5.web-security-academy.net https-post-form "/login2:mfa-code=^PASS^:Invalid security code"
-> Now i searched for more tools rather than using hydra. There i came to know about many tools like ffuf, patator, gobuster, wfuz, etc.
-> Among these, i used ffuf. I used the following command. The brute force was done a file named 4-digit-list.txt that contained the 10000 possible 4-digit combinations. This file was generated using the command 'seq -w 0000 9999 > 4-digit-list.txt':
        ffuf -u https://0a71008004bc4ce18172ff4e00c600bd.web-security-academy.net/login2 \
        -X POST -d "mfa-code=FUZZ" \
        -H "Cookie: session=AoC39Yuo2b7Oi2BDr3KSYgC4e1ubQcH7; verify=carlos" \
        -H "Content-Type: application/x-www-form-urlencoded" \
        -w 4-digit-list.txt -fc 302 \
        > logs.txt

-> In this command, Using -fc (Filter by Status Code): If the successful response returns a different HTTP status code (like 200 or 302 for success and 401/403 for failure), you can filter to only show those successful status codes.
-> using '> logs.txt' will save all the data in this text file which will help sorting the logs and search for our dersired result rather than searching for it in the terminal.
-> After a successful attack, the logs file contained unsorted data. To sort it, i used the following command to sort the data: 'sort logs.txt -o output.txt'. This saved the sorted data in output.txt.
-> After observing the data there was 9999 data input informatioin rather than 10000 which means that there is one input missing with the 302 status code because we added '-fc 302' in our attack command. Hence, this missing input is the correct verification code.
-> I used a python script to search for the missing input since it was toooo much difficult to search for a missing field in a data of 10000 lines. The python script is present in the project directory.
-> Running the python script using following command, it gave that one missing input: 'python3 code.py'

Result: The session cookie of a valid user got used for an unknown account to generate the code without the verification of the password.

