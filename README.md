# Mooc Cyber Security Project 1
Course Project I for F-Secure Cyber Security Base MOOC

In the first course project, your task is to create a web application that has at least five different flaws from the OWASP top ten list (https://www.owasp.org/images/7/72/OWASP_Top_10-2017_%28en%29.pdf.pdf).

Starter code for the project is provided on Github at https://github.com/cybersecuritybase/cybersecuritybase-project.

----------------------------------------
**Flaws**
----------------------------------------

**A2 - Broken Authentication**

**Description:**

Password reset does not have a current password verification.

Application's password reset is not actually functional, but in real situation attackers could get access to users account by resetting password.
Problem can be reproduced in the following manner.

**Issue:**

Broken Authentication and Session Management

Steps to reproduce:
1. Run the project in for example Netbeans
2. Go to http://localhost:8080
3. Sign in with username "ted" and password "ted" and click Login button
4. In the "form" page click reset password
5. Enter new password and confirmation (original password not necessary)
6. Click submit

**Fix:**

At least the user's original password should be queried from the user while changing the password. Still, this is not a perfect solution by far, but its atleast better than having no authentication mechanisms at all. Another good way to make this better, would be a use of token's.

----------------------------------------

**A3 - Sensitive Data Exposure**

**Description:**

Re-entering person's name for event registration will show if that person has already registered and show his/her address.

**Issue:**

Sensitive Data Exposure

Steps to reproduce:
1. Run the project in for example Netbeans
2. Go to http://localhost:8080
3. Sign in with username 'ted' and password 'ted' and click Login button
4. In the "form" page type in "ted" as name and "address" as address and press submit
5. You will receive a message indicating that 'ted' has signed up to the event
6. go back to http://localhost:8080/form
7. insert "ted" as name and click submit
8. message displayed will show ted's address

**Fix**

add "httpSession.invalidate();" into beginning of defaultMapping() and loadForm() in SignupController.

----------------------------------------

**A5 - Broken Access Control**

**Description:**

If the attacker knows user's name, he or she can write "/duplicate?name=XXX" into address bar and see if the user has registered to the event and what address the user used.

**Issue:**

Insecure Direct Object References

Steps to reproduce:
1. Run the project in for example Netbeans or IntelliJ
2. Go to http://localhost:8080
3. Sign in with username "ted" and password "ted" and click Login button
4. In the "form" page type in "ted" as name and "address" as address and press submit
5. You will receive a message indicating that user has signed up to the event
6. Go to localhost:8080/duplicate?name=ted
7. Message will show that ted has already registered and his address

**Fix:**

You should have some sort of user control mechanism. Meaning the duplicate method would only allow you to view your own information, not other user's information.

----------------------------------------

**A7 - Cross-Site Scripting**

**Description:**

Attacker can upload malicious javascript code through text fields.
Owasp ZAP can be used to fuzz name and address fields, but tester would still need to manually find all pages that print stored scripts.

**Issue:**

Cross-Site Scripting

Steps to reproduce:
1. Run the project in for example Netbeans
2. Go to http://localhost:8080
3. Sign in with username "ted" and password "ted" and click Login button
4. In the "form" page type in "ted" as name and "<script>alert('BAZINGA!')</script>" as address and press submit
5. "user has been signed up to the event" appears. This stores the script as executable that will run when sent to the victim's browser
6. Go back to http://localhost:8080
7. If you are still logged in, type in "ted" as name and anything you want as an address
8. This sends you to duplicate page and browser runs the javascript that was previously loaded

**Fix:**

Validate all data inputs so they can't take characters "<" and ">", only the data they are supposed to take.

----------------------------------------

**A9 - Using Components with Known Vulnerabilities**

**Issue #1:**

Components with known vulnerabilities

Steps to reproduce:
1. Run the project in for example Netbeans or IntelliJ
2. Go to the address http://localhost:8080
3. Open developer tools network tab in your browser
4. Sign in as ted
5. You can see that script files are loaded when the form -page is loaded
6. Select file "jquery.min.js" and after that "Preview"-tab
7. You can see that the jQuery version is "jQuery v1.11.3"

That version of jQuery contains vulnerability which can be seen for example from the following link:
https://www.cvedetails.com/cve/CVE-2016-7103/


**Issue #2:**

Application uses old version of spring framework (1.4.2) which can be seen from pom.xml.

**Fix #1:**

The flaw can be fixed by updating jQuery to the newest version and at the same time updating Bootstrap to the latest version by downloading the newest minified files. Bootstrap CSS file should be copied to the directory src/main/resources/resources/styles/ and jQuery and Bootstrap JavaScript files should be copied to the directory src/main/resources/resources/scripts/. You should also remove the old minified files when you add the new files. After that you should update all script and style imports in form.html to point to the new files.

**Fix #2:**

This can be fixed by changing 1.4.2.RELEASE to a newer version on line 16 of pom.xml file.

----------------------------------------