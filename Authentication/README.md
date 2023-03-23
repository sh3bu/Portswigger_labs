# Notes :

## 2FA

- Response manipulation [In response, if “success”:false, change it to “success”:true]
- Status code manipulation [If Status Code is 4xx, try to change it to 200 OK and see if it bypass restrictions]
- 2FA code leakage in response [Request for 2FA code and intercept the request & Analyze the response and see if the 2FA code is leaked or not]
- 2FA code Re-usability 
- Lack of Brute-force protection
- Direct request/Forceful browsing [This is the flaw of broken access control where the web application fails to check authorization, which allows the attacker to access resources that they should not be able to access just by giving the path of the exact resource]
- JS files analysis [Sometimes the application uses dynamic JavaScript files to store a copy of OTP, which is matched against the OTP received by the user to perform the check on the client-side and validate the user. While triggering the 2FA code request, analyze all the js files that are included in the response to see if any JS file includes information that can support bypass the 2FA code.]
- 2FA bypass by sending blank code
- Enabling 2FA Doesn’t expire the Previous session [Log in to the application in two different browsers and enable 2FA from 1st session. Use 2nd session and if it is not expired, it could be considered as vulnerability]











