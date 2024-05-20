# GMAIL Architecture

## Requirement List

1. Registering a user
2. Login (2-FA)
3. Profile Creation
4. Set Preference
5. Sending Emails
6. Searching Emails
7. Spam and Virus Detection
8. Contacts and groups

## Gateway

- Responsibility to handle external requests
  - lets say its gmail.com now thats gonna route the request to the gmail.com gateway and lets say the request looks like /auth?username=subham&pass=xyz than by looking into the /auth the gateway knows the request should be routed to auth service
  - gateway is kindof like DNS server auth service will be having an IP
  - register ==> AuthService
  - profile ==> ProfileService
    lets say if there is an enormous number of requests coming in than there will be rate limiters in the gateway and they will blacklist
  - the another thing that the gateway gonna do is translate the protocols
  - now the issue is the gateway has a lot of responsinbilities and which is not good
  - so what we gonna do we will setup a service mapper which will come betweent all the services and the gateway all the services will report to the service mapper service
  - this service mapper will store the information like which route endpoint will be routed to which service IP
  - now if the /auth comes first the gateway will ask the service mapper shich service this API should route to and the gateway will route that request to that service
  - and i will take the response and return the response
  - now in the DB i have the userID and pass and i have the phone number but to verify that the number belongs to you only i will send a verification code to your phone number
  - now instead of storing the code in the user table itself will store the userId and code in an another table
  - so now lets say will store the timestamps as well in that table and it will be the expiry time of the OTP
  - now we need the SMS service there will be having templates of all the messages example Thanks $firstname for registering your one time password
  - in teh user table will add another field called verified initially it will be false till the 2FA
  - now lets say with that username he has putten a wrong phone number and he havent got the OTP than he should try with another username
  - after recieving the code we will again send the code and the userID to the auth service and now auth service will check if the code is correct and the timestamp is under the time limit or not
  - now for that user i will make the user as verified true
  - now registration is complete now if the user wishes to cheange the profile he can do that
  - the endpoint will look like /update-profile this will be a post request it will hit the gateway and from gateway it will route that to the profile service
  - now the service manager will tell that the requests that comes from /createProfile will be routed to the profile service
  - the profile service itself is a distributed systems and there might be 3 to 4 IPs where the profile service is running
  - if lets sya one of the boxes crashes than what so here we need some heartbeat mechanism
  - now the service manager will be having all teh states of the services it will communicating with all the services whether its alive or dead
  - now instead of maintaining an another service mapper we can cache the mapping data every 1 hour and there wont be any need for the service mapper
  - [Map](https://d3ivphvzw9pbeo.cloudfront.net/GmailPart11-200623-122243.png?Expires=1716179182&Signature=Evhk-RKROdIPkf4t-e0tWgcK3Ws5byNihc8J1ZtfaWwPyyO41S3nwexPrOtXqTzhcUccEXW8pC0IUxfuPhK3we1enDNFFWpTLUJys2010kM6NN-XhO5klSeMukxaxO9eYjIhA6M-05F3Tj0yR6KSqXKxUsuTIbTidbojEtc4hAimKeSMpdsN66U6Q4eBTof3IP3in1HzcoA7WCZQSr9ieq-fzbGSNb3dn54orO7819itb7g0wtGOEg6kyX4WJCVyztPn7tI8IaPzPsiawFIWGsA9nIUvcLeBiKSALCiq3EGlMZLhWZKy7ChDdlYYYzIGv0iJytYOuunIMy6cCVqmEg__&Key-Pair-Id=K2UERR4YIMY56O)

## Login

- Every request i am sending that should be authenticated
- if i login and i send an un authenticated request
- when i login the server should give me a secret key and for every request i will give you the key and you will allow me to access everything
- i will make a table which will have the user_id and token and the time at which token was created
- will use a JWT token
- in every request we will send the token and on every request it will validate if the token is the same as the stored token
- the token and the userId table we will cache in the gateway
-
