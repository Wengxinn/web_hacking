# Authentication

- Authentication
  - process of verifying that a user really is who they claim to be
- Authorisation
  - process of verifying whether a user is allowed to do something

## Vulnerabilities in password-based login
### Brute-force attacks
- uses a system of trial-and-error to guess valid user credentials
- using wordlists of usernames and passwords

### Brute-forcing usernames/password
- recognisable pattern (e.g. email address)
- predictable usernames (e.g. admin, administrator)
- check whether discloses potential usernames publicly 
  - access user profiles without logging in
- check HTTP responses to see if any email addresses are disclosed

### Username enumeration
- observe changes in the website's behaviour to identify whether a given username is valid
- Pay attention to any differences in:
  - status code
  - error messages
  - response times

### Prevent brute-force attacks
1. Account lock
- Methods to bypass:
  - establish a list of candidate usernames that are likely to be valid 
    - e.g. username enumeration/wordlists
  - decide on a very small shortlist of passwords that ou think at least one user is likely to have
    - number of passwords selected < number of login attempts allowed
  - use tool such as `Burp Intruder`
    - attempt to brute-force without triggering the account lock
- Does not protect against **credential stuffing**
  - reuse the same username and password on multiple websites
2. User rate limiting - block IP address
- Methods to unblock IP
  - automatically after a certain period of time has elapsed
  - manually by an admin
  - manually by the user after successfully completing a CAPTCHA

##
