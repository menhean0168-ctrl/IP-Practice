Week 5 Answers

1. You refresh the browser on /events/2 and the page loads. Which program answered that request in development, and what must the production server do so it still works after deployment?
 The Vite dev server answered. It returns index.html for any path that is not a real file, so Vue Router can show the page. The production server must do the same: serve index.html for every route that is not a file.

2. The guard stops students from opening /events/new. Can a student still create an event? How, and  what must stop them?
 Yes. The guard only hides the page. A student can send POST /events directly (curl or DevTools) and skip the app. The server must check the user's role and reject anyone who is not an organiser.

3. EventDetailPage loads its event in a watch on the id with immediate: true, not in onMounted. What bug would onMounted alone cause?What bug would onMounted alone cause?
 Going from /events/1 to /events/2 reuses the same component, so onMounted does not run again and the page keeps showing event 1. A watch on the id with immediate: true loads the event on first load and every time the id changes.

4. Why does EventsPage use storeToRefs(store) instead of const { events } = store?
 Destructuring copies the current value, so events stops updating when the store changes. storeToRefs keeps each value as a ref, so the page stays reactive.

5. Booking makes two requests: POST the booking, then PATCH the seat count. What happens to the data if the second one fails? How could the server avoid this?
 The booking is saved but the event's seat count is not updated, so the data disagrees and the event can be overbooked. The server should do both steps in one transaction (all or nothing), or work out seats left from the bookings.