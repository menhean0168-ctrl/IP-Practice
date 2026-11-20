# Week 7 Answers

## 1. Why does Booking have an eventId column but Event has no bookingIds column? Where is the “many” side of a 1-N relation stored?
The “many” side holds the foreign key. A column can hold one eventId, but it can’t hold a list of booking ids.

## 2. booked repeats information we could compute with COUNT and SUM over bookings. Why keep it, and what must the code do so it never becomes wrong?
booked makes reads fast, with no COUNT/SUM over bookings. It stays correct only if every booking create or delete updates it in the same transaction.

## 3. What is the difference between prisma migrate dev and prisma generate? Which one changes the database?
migrate dev writes and runs SQL, so it changes the database. generate only rebuilds the TypeScript client.

## 4. A page lists 20 events with their bookings. One version runs 1 query, another runs 21. Name the problem and show how include fixes it.
The N+1 problem happens when you run 1 query to get the 20 events, then 1 more query per event to get its bookings, so 1 + 20 = 21 queries. include fixes it because Prisma fetches all the bookings for those events in one extra query, instead of one per event:

## 5. Two students book the last seat at the same moment. Explain step by step why the read-check-update version can sell two seats and why the conditional updateMany cannot.
Read-check-update (can sell two seats):
- Student A reads the event: booked = capacity - 1, so 1 seat is left.
- Student B reads the event at the same moment and sees the same thing.
- Both pass the JavaScript check “is there a seat?”
- Both run the update, booked + 1, so booked goes over capacity and two seats are sold.
The problem is that the check and the update are separate steps, and the other student can slip in between them.

Conditional updateMany (cannot):
-The check is part of the update itself: UPDATE ... WHERE booked <= capacity - seats.
-   The database runs these updates one at a time on the same row, so A’s update runs first and booked becomes full.
- When B’s update runs, the WHERE no longer matches, so 0 rows are updated.
- count === 0, so the code throws 409 and only one seat is sold.