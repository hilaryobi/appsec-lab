## Fix 1: Login SQL injection (parameterized query)

**Original finding:** Attack 1 — login bypass via SQL injection

**Fix applied:**
Changed routes/login.ts to use a parameterized query instead of
building the SQL string with template literals. The email and
password are now passed as a `replacements` array, with `?`
placeholders in the query, instead of being inserted directly.

**Before:**
`SELECT * FROM Users WHERE email = '${req.body.email}' AND password = '...'`

**After:**
`SELECT * FROM Users WHERE email = ? AND password = ?`
(values passed separately via `replacements`)

**Re-test result:**
Repeated the original attack — {"email":"' OR 1=1--","password":"anything"}
Before: 200 OK, returned admin token
After: 401 Unauthorized, "Invalid email or password."

**Status:** Fixed and verified.
