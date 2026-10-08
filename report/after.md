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


## Fix 2: Search bar SQL injection (parameterized query)

**Original finding:** Attack 3 — SQL injection via search bar, full user data dump

**Fix applied:**
Changed routes/search.ts to use a parameterized query instead of
building the SQL string with template literals. The search term
is now passed as a `replacements` array, with `?` placeholders in
the query.

**Before:**
`WHERE ((name LIKE '%${criteria}%' OR description LIKE '%${criteria}%') ...)`

**After:**
`WHERE ((name LIKE ? OR description LIKE ?) ...)`
(values passed separately via `replacements`)

**Re-test result:**
Repeated the original attack via direct GET request:
/rest/products/search?q=')) UNION SELECT id,email,password,'4','5','6','7','8','9' FROM Users--
Before: 200 OK, returned full Users table (emails + password hashes)
After: 200 OK, data: [] — no injection, no data leaked

**Status:** Fixed and verified.
