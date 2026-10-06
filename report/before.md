## Attack 1: Login bypass via SQL injection

**Threat model rank:** 1 (Spoofing) and 3 (SQL injection, App → Database)

**What I did:**
Sent a login request to /rest/user/login with:
{"email":"' OR 1=1--","password":"anything"}

**What happened:**
Received a 200 response with an auth token and user data for
admin@juice-sh.op. This logged me in as the site admin without
knowing any password. The app builds a database query using the
email field directly, and the injected `' OR 1=1--` closes the
string early, adds a condition that's always true, and comments
out the rest of the query — so the database returns the first
matching user (the admin) regardless of password.

**Screenshot:** ![Login bypass](screenshots/login-bypass.png)
