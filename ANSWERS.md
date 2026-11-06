# Week 6 Answers

## 1. DELETE /events/2 returns 204 and the second call returns 404. Is DELETE still idempotent? Explain with what idempotent means.
Yes. Idempotent means repeating the request leaves the server in the same state. Event 2 is gone after the first call and still gone after the second. Only the response differs (204, then 404), not the state.

## 2. The Vue app on port 5173 calls the API on port 3000. Which program blocks the response when CORS is not configured, and why can curl read it anyway?
The browser blocks it, because port 5173 and port 3000 are different origins and the server sent no allow header. curl is not a browser and does not check CORS, so it just prints the response.

## 3. EventsController never writes new EventsService(). Who creates the service, when, and how many are created?
Nest's dependency injection creates it when the app starts, and passes it into the controller's constructor. Only one instance is created, and everyone shares it.

## 4. A client sends { "title": "Pitch Night", ..., "booked": 50 }. Which line of code refuses it, and what would happen to booked without it?
`forbidNonWhitelisted: true` in `src/setup.ts` refuses it with 400. Without it, the field is silently stripped (or, without `whitelist` too, a client could set `booked` itself).

## 5. Why is the “not enough seats” rule in BookingsService and not in BookingsController or only in the Vue form?
The service holds the rules and data, and the controller only maps HTTP. The Vue form can be bypassed (for example with curl) and its seat count can be old. Only the server knows the real `booked` value.
