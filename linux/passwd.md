
# PASSWORD ROTATION

It will only affect new users that are created on the server

New required values in ````/etc/login.defs````
````
PASS_MAX_DAYS 90  
PASS_MIN_DAYS 1    
PASS_WARN_AGE 14
````

To change in existing user:

* PASS_MIN_DAYS ````chage -m NEWNUMBER  USERNAME ````
* PASS_MAX_DAYS ````chage -M NEWNUMBER  USERNAME ````
* PASS_WARN_AGE ````chage -W NEWNUMBER  USERNAME ````


# PASSWORD STRENGTH

It will affect all users when they need to change their password.

New values in ````/etc/pam.d/common-password-pc````
* minlen=8 – password must be at least 8 characters
* remember=5 - You cannot use any of the 5 last password
* minclass=4 - It must contain at least: one uppercase, one lowercase, one number and one special character (.-$"!()-_.......)

````
password        requisite       pam_cracklib.so  minlen=8 minclass=4
password        required        pam_unix.so     minlen=8 remember=5 use_authtok shadow try_first_pass
````
*IMPORTANT!:* you need to create opassword file if it does not exist for store old password
````
touch /etc/security/opasswd 
chmod 600 /etc/security/opasswd
````
