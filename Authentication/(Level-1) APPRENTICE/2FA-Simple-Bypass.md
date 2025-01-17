-> First of all, i opened the lab in which we were given our username and password as well as the victim's username and password.
-> So, i used the given username and the password of the victim to login to the account.
-> After entering the credentials, it asked for a 4 digit security code.
-> I tried to capture the request format to understand what is happening when i enter any random 4 digit number.
-> I entered 1234 as a sample wrong input, but by chance, this was the correct code.
-> So the system got logged in, but i did not came learn any new thing from this lab since i got logged in by chance without using any proper implementation.
-> After a few days, when i tried to login using the same approach, the code '1234' did not work. So now, i was on the same untracked path that i wanted for the purpose of learning.
-> I reviewed the requests through burpsuite using the repeater as well thinking that may be i should do some brute force thing or someting like that.
-> There was no proper logic in my mind since the previous idea did not make any sense in guessing the code like that, so i decided to take some hints from the solutions.
-> There i learned that we had to go to the acounts page manually by manipulating the url of the web.
-> The solution was as follows:
	=> Log in to your own account. Your 2FA verification code will be sent to you by email. Click the Email client button to access your emails.
	=> Go to your account page and make a note of the URL.
	=> Log out of your account.
	=> Log in using the victim's credentials.
	=> When prompted for the verification code, manually change the URL to navigate to /my-account. The lab is solved when the page loads.y
-> I followed the same pathway i got logged into the user's account.
-> But in my approach, i missed a single thing that i did not get the emails button when i tried to login using my own correct credentials.
-> May be this was because i had already solved the lab or any else reason. but after all, i came to know the main logic that we had to guess the web page into which we will be taken when we login successfully.
-> That correct login page can be retrieved when we successfully login to our own account.

Result: The main flaw was that it should not take the user to any other page till the user gets authorized.
If the user is first prompted to enter a password, and then prompted to enter a verification code on a separate page, the user is effectively in a "logged in" state before they have entered the verification code. 
In this case, it is worth testing to see if you can directly skip to "logged-in only" pages after completing the first authentication step. 
Occasionally, you will find that a website doesn't actually check whether or not you completed the second step before loading the page.