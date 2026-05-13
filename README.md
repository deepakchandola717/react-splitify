# react-splitify
Instructions for Splitlify(Splitwise like expense sharing application).

Updated version below, based on your original Splitify requirements. 

---

# Splitify — Full-Stack Web App Requirements

Build a full-stack web application called **Splitify**, similar to Splitwise, for managing shared expenses between friends, flatmates, families, couples, travel groups, and teams.

The application must be built using:

## Required Tech Stack

### Frontend

* React
* TypeScript
* Responsive web UI
* Component-based architecture
* Form validation
* Protected pages
* API integration with the backend

### Backend

* Node.js
* Express.js
* TypeScript
* PostgreSQL
* Sequelize ORM
* Sequelize migrations
* JWT-based authentication
* REST API design
* Input validation
* Secure backend architecture

### General Requirement

The frontend and backend should be separate applications.

All persistent data must be stored in PostgreSQL. Do not use Firebase, Supabase, SQLite, local-only storage, or browser-only persistence as the primary storage mechanism.

---

# 1. Project Goal

Splitify should help users manage shared expenses.

The application should clearly answer:

> Who owes whom, how much, and because of which expenses?

The three most important goals are:

1. Users should be able to easily add shared expenses.
2. The app should clearly show balances between users.
3. Users should be able to record settlements and maintain expense history.

The main priority is correctness, clean architecture, and reliable balance calculation. Fancy UI is secondary.

---

# 2. Authentication and User Accounts

Implement user account functionality through the backend.

Users should be able to:

* Sign up using email and password.
* Log in using email and password.
* Log out.
* Access protected pages only after authentication.
* View their own profile.
* Update their profile.
* Delete their account.
* Use forgot password and reset password flow.

User profile should include:

* Name
* Email
* Phone number
* Profile photo
* Default currency
* Account status

Authentication should use:

* Secure password hashing.
* JWT-based authentication.
* Auth middleware on protected backend APIs.
* Proper login validation.
* Rate limiting for sensitive authentication actions.
* Clear error handling for invalid credentials, expired sessions, and unauthorised access.

Optional login methods such as Google login, Apple login, or phone OTP login are not required for the main version.

---

# 3. Dashboard

Create a dashboard where the user can quickly understand their overall balance.

The dashboard should show:

* Total amount the user owes.
* Total amount others owe the user.
* Net balance.
* Recent groups with outstanding balances.
* Recent friends with outstanding balances.
* Recent activity.
* Quick action to add an expense.
* Quick action to settle up.

Example display:

```txt
You owe ₹1,250
You are owed ₹3,600
Overall, you are owed ₹2,350
```

The dashboard should make the user’s current financial position immediately clear.

---

# 4. Friends

Users should be able to manage friends.

A friend may be:

* A registered Splitify user.
* A placeholder contact who has not yet signed up.

Users should be able to:

* Add friends manually using name, email, or phone number.
* View a list of friends.
* Search friends.
* View balance with each friend.
* Open a friend detail page.
* See all expenses involving a friend.
* See all settlements involving a friend.
* Invite a friend to join Splitify.
* Remove or archive a friend where appropriate.

A friend detail page should clearly show:

* Whether the user owes the friend.
* Whether the friend owes the user.
* Total outstanding balance.
* Expense history.
* Settlement history.
* Related activity.

---

# 5. Groups

Users should be able to create and manage groups.

A group should have:

* Group name
* Image or icon
* Category
* Members
* Creator
* Status

Suggested group categories:

* Trip
* Home / Flatmates
* Couple
* Family
* Office
* Event
* Other

Users should be able to:

* Create groups.
* Edit group details.
* Add members.
* Remove members.
* View group expenses.
* View group balances.
* View who owes whom inside the group.
* Add expenses inside a group.
* Record settlements inside a group.
* Archive old groups.

A group detail page should clearly show:

* Group members.
* Total group spending.
* Individual balances.
* Simplified balances.
* Group expense history.
* Group settlement history.
* Group activity.

---

# 6. Expenses

Expense creation is the most important feature.

Users should be able to add an expense with:

* Title or description.
* Amount.
* Currency.
* Date.
* Paid by.
* One or more payers.
* Participants.
* Group or friend context.
* Category.
* Notes.
* Receipt or image attachment.

Expense categories may include:

* Food
* Travel
* Rent
* Groceries
* Utilities
* Entertainment
* Fuel
* Shopping
* Medical
* Subscription
* Other

Users should be able to:

* Add an expense.
* Edit an expense.
* Delete an expense.
* View expense details.
* Attach a receipt.
* Add notes or comments.
* See the full split calculation.

Expense creation should be easy to understand. A good UI may divide it into sections such as:

* Basic expense details.
* Payers.
* Participants.
* Split method.
* Calculation preview.
* Confirmation.

The exact layout is up to the developer.

---

# 7. Split Types

Support the following split methods.

| Split Type         | Requirement                                                          |
| ------------------ | -------------------------------------------------------------------- |
| Equal Split        | Divide the amount equally among selected people.                     |
| Exact Amount Split | User manually enters the exact amount for each participant.          |
| Percentage Split   | User assigns a percentage to each participant.                       |
| Share Split        | User assigns shares such as 2 shares, 1 share, etc.                  |
| Multiple Payers    | More than one person can pay different amounts for the same expense. |

Validation must ensure:

* Expense amount is greater than zero.
* Currency is selected.
* At least one payer is selected.
* At least one participant is selected.
* Exact split totals match the expense amount.
* Percentage split totals 100%.
* Share split values are valid.
* Multiple payer amounts match the total expense amount.
* Rounding is handled correctly.
* Users involved in a group expense actually belong to that group.

The frontend may show a calculation preview, but the backend must perform the final trusted calculation before saving the expense.

---

# 8. Balances

The app must correctly calculate:

* Who owes whom.
* Amount owed by each person.
* Amount each person is owed.
* Friend-wise balance.
* Group-wise balance.
* Overall user balance.
* Settlement-adjusted balance.
* Simplified debts.

Example:

Without simplification:

```txt
A owes B ₹500
B owes C ₹500
```

With simplification:

```txt
A owes C ₹500
```

Balances must always remain traceable to the underlying expenses and settlements.

The system should not simply store final balances in an opaque way. It should be possible to understand why a user owes a particular amount.

A good approach is to maintain a transaction or ledger-style record of balance movements so that every balance can be traced back to a source action such as:

* Expense creation.
* Expense edit.
* Expense deletion.
* Settlement creation.
* Settlement edit.
* Settlement deletion.

The developer is free to decide the exact implementation.

---

# 9. Settle Up

Users should be able to record payments made outside the app.

The application does not need to process real payments. It only records that a payment or settlement happened.

A settlement should include:

* Paid by.
* Paid to.
* Amount.
* Currency.
* Date.
* Payment method.
* Notes.
* Group or friend context.

Payment methods can include:

* Cash
* UPI
* Bank transfer
* Card
* Wallet
* Other

Users should be able to:

* Record a settlement.
* View settlement details.
* Edit a settlement.
* Delete a settlement.
* See settlement history with a friend.
* See settlement history inside a group.

Editing or deleting a settlement must update balances correctly.

---

# 10. Activity Feed

Maintain an activity history for important actions.

Track activities such as:

* Expense added.
* Expense edited.
* Expense deleted.
* Settlement recorded.
* Settlement edited.
* Settlement deleted.
* Group created.
* Group updated.
* Member added.
* Member removed.
* Friend added.
* Receipt attached.
* Comment added.
* Profile updated.

Activity should be visible:

* Globally.
* Inside relevant group pages.
* Inside relevant friend pages.
* Inside relevant expense pages where appropriate.

Each activity entry should be understandable to a normal user.

Example:

```txt
Deepak added “Dinner at Toit” in Bangalore Trip.
Rahul paid ₹500 to Deepak.
Priya was added to Goa Trip.
```

---

# 11. Notifications

Support basic in-app notifications.

Notifications should be created for:

* New expense involving the user.
* Expense edited.
* Expense deleted.
* User added to group.
* Settlement recorded.
* Someone paid the user.
* User owes someone.
* Friend invite received.

Users should be able to:

* View notifications.
* Mark one notification as read.
* Mark all notifications as read.
* See unread notification count.

Email, push, and reminder notifications are not required for the main version.

---

# 12. Search, Filters, and Export

Support search and filtering for:

* Expenses
* Groups
* Friends
* Settlements
* Activity

Useful filters include:

* Date range
* Group
* Friend
* Category
* Amount range
* Paid by
* Currency

List views should support:

* Pagination or infinite loading.
* Search.
* Sorting.
* Filtering.
* Empty states.
* Loading states.

Export features are not required, but CSV export may be added as an optional enhancement if the core application is complete.

---

# 13. Multi-Currency

Support adding expenses in different currencies.

Minimum requirement:

* User can choose currency while adding an expense.
* Currency is stored with the expense.
* Balances in different currencies remain separate.
* INR, USD, EUR, and other currencies should not be automatically merged.

Optional currency conversion is not required.

To avoid floating-point calculation issues, money should be stored carefully. A recommended approach is to store amounts in the smallest currency unit.

Example:

```txt
₹1,250.50 should be stored as 125050 paise
$100.25 should be stored as 10025 cents
```

---

# 14. Database Design Expectations

The developer must design the PostgreSQL database schema using Sequelize models and migrations.

The database design should be clean, normalised, and suitable for a real-world financial tracking application.

The design should ensure:

* Proper separation of users, groups, friends, expenses, settlements, balances, activity, and notifications.
* No unnecessary duplicate data.
* Clear relationships between entities.
* Proper foreign keys.
* Proper indexes for frequently queried data.
* Soft deletion where financial history should not be permanently removed.
* Auditability of important financial actions.
* Safe handling of money values.
* Support for group-based and friend-based expenses.
* Support for multiple payers.
* Support for different split types.
* Support for placeholder friends or invited users.
* Support for balance traceability.
* Support for multiple currencies.
* Consistent timestamps across records.

The schema should be designed by the developer based on the feature requirements. It should not be a random set of tables created only to make the UI work.

---

# 15. Backend Business Logic Expectations

The backend should contain the trusted business logic.

Important financial calculations must not depend only on the frontend.

## Expense Creation

When an expense is created, the backend should:

* Validate the request.
* Check user permissions.
* Check group membership if the expense belongs to a group.
* Validate payer details.
* Validate participant details.
* Validate the selected split method.
* Calculate the final split.
* Save the expense.
* Save payer and participant information.
* Update balance records or balance ledger.
* Create activity entries.
* Create notifications for affected users.

This should happen safely. If any important step fails, the database should not be left in a partially updated state.

## Expense Edit

When an expense is edited, the backend should:

* Fetch the existing expense.
* Check permission.
* Recalculate the split.
* Correctly update balances.
* Preserve traceability of what changed.
* Create activity entries.
* Notify affected users where appropriate.

## Expense Delete

Financial records should generally not be hard deleted.

When an expense is deleted, the backend should:

* Mark it as deleted or cancelled.
* Reverse or adjust its balance effect.
* Preserve enough history for auditability.
* Add an activity entry.

## Settlement Handling

When a settlement is recorded, edited, or deleted, the backend should:

* Validate the users involved.
* Validate the amount and currency.
* Update balances correctly.
* Preserve settlement history.
* Add activity entries.
* Notify affected users where appropriate.

---

# 16. API Design Expectations

The developer is free to design the API endpoints.

The API design must satisfy all feature requirements in this document.

The backend APIs should support:

* Authentication.
* Current user profile.
* Friends.
* Groups.
* Group members.
* Expenses.
* Expense split calculation.
* Settlements.
* Balances.
* Activity feed.
* Notifications.
* Search and filters.
* Pagination.
* File or receipt attachment metadata if implemented.

API responses should be consistent.

A good response format should clearly distinguish:

* Success response.
* Validation error.
* Authentication error.
* Permission error.
* Not found error.
* Server error.

The backend should not expose private data belonging to unrelated users.

---

# 17. API Security Requirements

The backend must include:

* JWT authentication.
* Protected APIs.
* Password hashing.
* Request validation.
* Centralised error handling.
* Rate limiting for sensitive actions.
* CORS configuration.
* Security middleware such as Helmet or equivalent.
* Permission checks for group resources.
* Ownership checks for expenses, friends, settlements, and groups.
* Safe Sequelize queries.

The user must not be able to:

* Access another user’s private data.
* Modify expenses in a group they do not belong to.
* Add settlements between unrelated users.
* Manipulate balances directly.
* Create duplicate expenses through repeated submissions.

---

# 18. Frontend Requirements

The frontend should be a responsive React web application.

The developer is free to design the layout, but the app should be clear and usable.

Suggested pages:

* Landing or welcome page
* Onboarding page
* Sign up page
* Login page
* Forgot password page
* Dashboard page
* Friends list page
* Friend detail page
* Groups list page
* Group detail page
* Add or edit group page
* Add or edit expense page
* Expense detail page
* Split configuration page or section
* Settle up page
* Activity feed page
* Notifications page
* Profile page
* Settings page

The frontend should include:

* Protected page handling.
* Auth state handling.
* Loading states.
* Empty states.
* Error states.
* Form validation.
* Toast messages.
* Confirmation modals for destructive actions.
* Search and filters.
* Pagination or infinite loading.
* Responsive behaviour for desktop and mobile browsers.

The UI should prioritise:

* Clear balance cards.
* Simple expense creation.
* Clear friend balances.
* Clear group balances.
* Easy settlement recording.
* Understandable transaction history.
* Helpful validation messages.

---

# 19. Calculation Logic

The calculation logic should be separated from controllers and route handlers.

A better approach is to keep split and balance calculations in dedicated reusable utility or service functions.

This is important because:

* Expense calculations can become complex.
* The same logic may be needed in multiple places.
* It keeps controllers clean.
* It makes debugging easier.
* It makes future testing easier.
* It reduces the chance of inconsistent balance calculations.

The developer can decide the exact structure, but calculation logic should not be scattered randomly across the codebase.

The system should correctly handle:

* Equal splits.
* Exact amount splits.
* Percentage splits.
* Share-based splits.
* Multiple payers.
* Settlement adjustments.
* Expense edits.
* Expense deletions.
* Rounding differences.
* Multi-currency balances.

---

# 20. Testing

Automated testing is optional.

However, the developer should manually verify all critical flows, especially:

* Signup and login.
* Creating friends.
* Creating groups.
* Adding group members.
* Creating expenses.
* Editing expenses.
* Deleting expenses.
* Recording settlements.
* Editing settlements.
* Deleting settlements.
* Equal split calculation.
* Exact split calculation.
* Percentage split calculation.
* Share split calculation.
* Multiple payer calculation.
* Friend-wise balances.
* Group-wise balances.
* Overall balances.
* Multi-currency separation.
* Permission checks.
* Protected API access.

If automated tests are added, calculation logic and backend business rules should be prioritised first.

---

# 21. Quality Expectations

Ensure that:

* Balance calculations are correct.
* Data persists in PostgreSQL.
* Editing expenses updates balances correctly.
* Deleting expenses updates balances correctly.
* Settlements update balances correctly.
* The app does not create duplicate expenses due to repeated clicks.
* Backend APIs are protected.
* Invalid users cannot access other users’ data.
* Empty states are handled properly.
* Validation messages are clear.
* Errors are handled gracefully.
* Code is modular and maintainable.
* Sequelize migrations are used.
* API responses are consistent.
* Important financial actions remain traceable.
* The application remains usable on both desktop and mobile browsers.
