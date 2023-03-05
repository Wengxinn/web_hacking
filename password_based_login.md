# Password-based Login

## Username enumeration via different responses
1. With Burp running, submit an invalid username: `admin` and password: `admin`.
2. In Burp, find the `POST /login` request on **Proxy > HTTP history**.
3. Send the request to **Intruder**.
4. On the **Positions** tab of **Intruder**, select `Sniper` as the attack type.
5. Clear `§` (payload).
6. Highlight the value of `username`, add `§`. e.g. `username=§admin§`.
7. On the **Payloads**tab, select `Simple list` as the payload type. 
8. Copy the username wordlist and start attack.
9. On the **Results** tab, examine the **Length** column. One of the entries will be different from others (e.g. `Invalid username`, `Incorrect password`). 
10. The odd result is the correct username.
11. Change the `username` value to the correct one and repeat the above steps to brute-force `password`. 
12. On the **Results** tab, examine the status code. The correct `username` and `password` results in `302` response - indicating that the login is successful. 

## Username enumeration via subtly different responses
1. With Burp running, submit an invalid username: `admin` and password: `admin`.
2. In Burp, find the `POST /login` request on **Proxy > HTTP history**.
3. Send the request to **Intruder**.
4. On the **Positions** tab of **Intruder**, select `Sniper` as the attack type.
5. Clear `§` (payload).
6. Highlight the value of `username`, add `§`. e.g. `username=§admin§`.
7. On the **Payloads**tab, select `Simple list` as the payload type. 
8. Copy the username wordlist.
9. On the **Settings** tab, under **Grep - Extract**, add the error message `Invalid username or password.` and start attack.
10.  The odd result is the correct username.
11. Change the `username` value to the correct one and repeat the above steps to brute-force `password`. 
12. On the **Results** tab, examine the status code. The correct `username` and `password` results in `302` response - indicating that the login is successful. 

## Username enumeration via response timing
1. With Burp running, submit an invalid username: `admin` and password: `admin`.
2. In Burp, find the `POST /login` request on **Proxy > HTTP history**.
3. Send the request to **Repeater**.
4. Add `X-Forwarded-For` as the header of HTTP request with random number. It is used to identify the client's IP address. Changing its value can spoof the IP address. 
5. Send the request to **Intruder**. 
6. On the **Positions** tab of **Intruder**, select `Pitchfork` as the attack type. `Sniper` uses single set of payloads and multiple payloads positions while `Pitchfork` uses multiple payloads. 
7. Clear `§` (payload).
8. Highlight the values of `username` and `X-Forwarded-For`, add `§`. e.g. `username=§admin§`, `X-Forwarded-For=§0§`. This allows **IP spoofing**.
9. Set a long password (e.g. 100-character password).
10. On the **Payloads**tab, select payload set 1 and `Numbers` as the payload type. 
11. Use numbers range from 1-100 with step = 1. Set `Max fraction digits` to 0.
12. Select payload set 2 and `Simple list` as the payload type.
13. Copy the username wordlist and start attack.
14. At the top of the **Results** tab, select **Response received** and **Response completed**.
15. Examine these columns and take note of the entry that took significantly longer response time than others. This works because when the username is invalid, the response time is roughly the same. However, when the username is valid (but the password is invalid), the response time depends on the length of password. 
16. Repeat these steps a few time to confirm the entry consistently takes the longest time. It is the correct username.
17. Change the `username` value to the correct one and repeat the above steps to brute-force `password`. 
18. On the **Results** tab, examine the status code. The correct `username` and `password` results in `302` response - indicating that the login is successful. 