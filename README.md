# CodePath Week 8 - Pentesting Live Targets
Time spent: **10** hours spent in total

> Objective: Identify vulnerabilities in three different versions of the Globitek website: blue, green, and red.

The six possible exploits are:
* Username Enumeration
* Insecure Direct Object Reference (IDOR)
* SQL Injection (SQLi)
* Cross-Site Scripting (XSS)
* Cross-Site Request Forgery (CSRF)
* Session Hijacking/Fixation

Each version of the site has been given two of the six vulnerabilities. (In other words, all six of the exploits should be assignable to one of the sites.)


## Blue
### SQL Injection + Bonus Objective 1
* Use the id parameter in the url to infiltrate the system: /blue/public/staff/salespeople/show.php?id=1 
* Explore the Database with sqlmap

![alt text](https://github.com/Mikhail-Kreytser/Cybersecurity-Week8/blob/master/Demo/SQLI.gif "SQLI Demo")

### Session Hijacking/Fixation
* Use the tool to change your PHPSESSID to a logged in user's PHPSESSID: public/hacktools/change_session_id.php
* Have a user open a malicious website that sets their PHPSESSID to the attacker's PHPSESSID. The user will have to login again. This makes the attacker's PHPSESSID valid.
![alt text](https://github.com/Mikhail-Kreytser/Cybersecurity-Week8/blob/master/Demo/SessionHijacking.gif "Session Demo")

## Red
### Insecure Direct Object Reference
* Use the public find a Salesperson tool to look for someone. After a Salesperson is found, use the id parameter in the url to look for other people in the system.
![alt text](https://github.com/Mikhail-Kreytser/Cybersecurity-Week8/blob/master/Demo/IDOR.gif "Insecure Direct Object Reference Demo")

### Cross-Site Request Forgery
* Create a website that has a form that posts to red/public/staff/salespeople/edit.php?id=1. When a logged in user goes the malicious website, the salesperson with id 1 has their information updated.
![alt text](https://github.com/Mikhail-Kreytser/Cybersecurity-Week8/blob/master/Demo/CSRF.gif "Cross-Site Request Forgery Demo")


## Green
### Username Enumeration
* Go to the main login. If the wrong password is entered and the user exists, the error is bold. If the wrong password is entered and the user does not exist, the error just plain text. 
![alt text](https://github.com/Mikhail-Kreytser/Cybersecurity-Week8/blob/master/Demo/UsernameEnumeration.gif "Username Enumeration Demo")

### Cross-Site Scripting + Bonus Objective 2
* Go leave some feedback and inject code. When a logged in user/admin visits the feedback page. The injected code is ran.
![alt text](https://github.com/Mikhail-Kreytser/Cybersecurity-Week8/blob/master/Demo/XSS.gif "Cross-Site Scripting Demo")

### Cross-Site Scripting + Session Hijacking/Fixation = Advanced Objective
* Hypothetically, the malicious code can change the logged in user's PHPSESSID to the attacker's. The code can then refresh the page or redirect to login. The user might think that it was a glitch and login again. By doing that, the attacker's PHPSESSID can now be used to login. The malicious code can also just send the current user's PHPSESSID to the attacker allowing the attacker to use it to login.

## Concept Review
* Which attacks were easiest to execute? Which were the most difficult?
  * SQLI was the easiest with the help of the sqlmap tool. The others attacks were on the same level of difficulty. None of these attacks were too difficult. 

* What is a good rule of thumb which would prevent accidentally username enumeration vulnerabilities like the one created here?
  * Return the same error string whether the user exists or not 
  
* Since you should be somewhat familiar with the CMS and how it was coded, can you think of another resource which could be made vulnerable to an Insecure Direct Object Reference? What code could be removed which would expose it? (Hint: It was also the answer to the first bonus objective to the Weekly Assignment for week 3.)
  * All the ids in the system follow a consecutive enumeration. If the ids were generated randomly, IDOR would me much harder.

* Many SQL Injections use OR as part of the injected code. (For example: ' OR 1=1 --'.) Could AND work just as well in place of OR? (For example: ' AND 1=1 --'.) Why or why not?
  * 'AND' is not as powerful as 'OR' when doing SQLI. When using 'AND' both conditions need to be true. On the other hand, with 'OR' only one condition needs to be true. Using something like **BOB'AND'1=1** will only return the row with BOB. However, using something like **BOB'OR'1=1** will everything no matter if the part before the 'OR' is true or not.

* A stored XSS attack requires patience because it could be stored for months before being triggered. Because of this, what important ingredient would an attacker most likely include in a stored XSS attack script?
  * It is important for the attacker to include a piece of code that can notify them when the xss is triggered. Otherwise, the attacker will never know if his attack worked or even if their code ran.

* Imagine that one of your classmates is an authorized admin for the site's CMS and you are not. How would you get them to visit the self-submitting, hidden form page you created in Objective #5 (CSRF)?
  * Social Engineering is the biggest flaw. I can always send them a malicious link to a game, book or even, my github. 

* Compare session hijacking and session fixation. Which attack do you think is easier for an attacker to execute? Why? One of them is much easier to defend against than the other. Which one and why?
  * I would say that session fixation is easier. I think it is easier to implant a cookie then it is to steal an existing one. Session hijacking is much easier to defend against. If an attacker wants to use a stolen cookie, they must use the same user-agent or ip address. Otherwise, the system can flag that login attempt.
