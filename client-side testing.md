**Parameter level bypass testing**

This checks whether relationships/dependencies *between* different input parameters are properly validated — not just each field in isolation, but the constraints that tie multiple fields together.

Here's how all four compare, with examples:

**1. Value level bypass testing**
Checks whether a *single* parameter's value constraints (type, range, format) are enforced server-side, even after client-side JS validation is bypassed.
- Example: A form limits "Age" to 18–60 via JavaScript. You bypass the JS (e.g., using browser dev tools or a direct HTTP request) and submit Age = -5 or Age = "abc". This tests if the server independently rejects it.

**2. Parameter level bypass testing** ✅ (the answer)
Checks whether *relationships among multiple parameters* are validated — constraints that only make sense when parameters are considered together.
- Example: A hotel booking form requires "Check-out date" > "Check-in date." Client-side JS enforces this, but you bypass it and submit check-in = 20 Sep, check-out = 15 Sep directly to the server. This tests whether the server catches the invalid *relationship* between the two fields, not just whether each date individually is well-formed.
- Another example: "Discount amount" should never exceed "Total price" — you submit a request where discount > total.

**3. Control flow level bypass testing**
Checks whether the application enforces the correct *sequence/order* of operations, rather than checking parameter values at all.
- Example: An e-commerce checkout has steps: Cart → Shipping Info → Payment → Confirm. You directly submit a request to the "Confirm Order" endpoint, skipping the Payment step entirely, to see if the server enforces that prior steps were actually completed.

**4. User-session data based testing**
Checks whether session-related data (cookies, tokens, hidden session variables) can be tampered with to alter behavior meant to be controlled server-side.
- Example: A shopping cart stores the item price in a session variable that's also sent as a hidden form field. You modify that hidden field's value before submission to see if the server trusts the client-supplied price instead of re-verifying it from the database.

The key distinguishing idea for your question: **parameter level** = testing *cross-field* dependency logic, while **value level** = testing a *single field's* own constraints.
