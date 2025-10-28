# 2FA bypass using a brute-force attack

## Challenge Information
* **Category**: Authentication
* **Level**: Expert (Level 3)

## Initial Analysis
- Before starting the lab, i first read the details about the topic carefully in which i figured out that there would be a 4-6 digit code and we will have to brute force this code.
- The website on which we will try to login will automatically logout us if we try for too many unsuccessful attempts. For this purpose, we will be using advanced features of burp like macros or turbo intruder.
- After accessing the lab, in which we were given the username and the password of the vitim, we only have to figure out the authentication code that will be sent after entering the correct username and the password.

## Request Analysis
- Now starting with the lab, i started the burpsuite and captured the first login resquest. Analyzing this request, i observed that there was another field with the username and the password named as 'crsf'.
- I noted the value of this field as well as the host and the session cookie in a file so that if they are used in future, i already have them with me.

## Attack Execution
- Now, we had to use the macro tool of the burpsuite for this purpose. As we all know that in real world the 2FA code expires after a certain time or attempts. So, the main reason is to maintain the session validity:
  - Without a macro: If the session times out or becomes invalid, every subsequent brute-force attempt would fail because the session is no longer active, even if the correct 2FA code is guessed.
  - With a macro: The macro allows Burp Suite to perform the login process (or refresh the session) before every 2FA request. This ensures each 2FA code you submit is tied to a valid, active session.
- To set up the macro in our environment, follow the below steps:
  
  1. In Burp, click  Settings to open the Settings dialog, then click Sessions. In the Session Handling Rules panel, click Add. The Session handling rule editor dialog opens.

  2. In the dialog, go to the Scope tab. Under URL Scope, select the option Include all URLs.

  3. Go back to the Details tab and under Rule Actions, click Add > Run a macro.

  4. Under Select macro click Add to open the Macro Recorder. Select the following 3 requests:

  ```
  GET /login
  POST /login
  GET /login2
  ```
  Then click OK. The Macro Editor dialog opens.

  5. Click Test macro and check that the final response contains the page asking you to provide the 4-digit security code. This confirms that the macro is working correctly.

  6. Keep clicking OK to close the various dialogs until you get back to the main Burp window. The macro will now automatically log you back in as Carlos before each request is sent by Burp Intruder.

  7. Send the POST /login2 request to Burp Intruder.

  8. In Burp Intruder, add a payload position to the mfa-code parameter.

  9. In the Payloads side panel, select the Numbers payload type. Enter the range 0 - 9999 and set the step to 1. Set the min/max integer digits to 4 and max fraction digits to 0. This will create a payload for every possible 4-digit integer.

  10. Click on  Resource pool to open the Resource pool side panel. Add the attack to a resource pool with the Maximum concurrent requests set to 1.

  11. Start the attack. Eventually, one of the requests will return a 302 status code. Right-click on this request and select Show response in browser. Copy the URL and load it in the browser.

## Vulnerability
**Result**: The main objective was to maintain session validity while brute-forcing a short-lived MFA code by using Burp macros (or Turbo Intruder) to refresh the session/CSRF before each attempt.
