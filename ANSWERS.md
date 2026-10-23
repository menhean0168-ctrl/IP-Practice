# Week 4 Answers

## 1. In EventList, what goes wrong if you use the array index as :key and the list gets filtered while a card has local state (for example an open details panel)?
If you use the index as the key, Vue thinks the card in the same position is the same card. After filtering, a different event takes that position and keeps the old card's open panel. Using `e.id` fixes this because each event keeps its own card.

## 2. Why is visible a computed and not a function called from the template? What is the difference when you type in the search box?
A computed only runs again when `search`, `category` or `events` change, and it remembers the result. A function in the template runs on every re-render, even for unrelated changes. When you type in the search box both update, but the computed does less work the rest of the time.

## 3. EventCard emits book instead of changing event.booked. Give two reasons.
Props are read-only, so the card can't change `event.booked` itself, and Vue warns if you try. Also, the parent owns the data, so it can update everything in one place: the card, the header count and the saved bookings.

## 4.  You write const { booked } = reactive(event); booked++. The page does not change. Why?
Destructuring copies the number into a normal variable, so it is no longer connected to the reactive object. Changing it doesn't change the event, so Vue doesn't know anything changed and the page stays the same. Using `event.booked++` keeps it reactive.

## 5. Where does myBookings live, and why not in EventCard or EventList?
It lives in App.vue, because App owns the events and the header needs the count. EventCard and EventList are children, so they can't share it with the header. Keeping it in one place means one source of truth.