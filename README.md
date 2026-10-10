# FoodSprint — Food Delivery App (LLD + Working MVP)

FoodSprint is a beginner-friendly, end-to-end food delivery demo inspired by common customer flows in Swiggy and Zomato. It uses Node.js built-in modules and plain HTML/CSS/JavaScript, so no dependency installation is needed.

## Run it

Requirements: Node.js 18+.

```bash
npm start
```

Open http://localhost:3000. For automatic server restart during development, use `npm run dev`.

## Included features

- Restaurant discovery with sample menus, ratings, cuisines and open/closed status.
- Search restaurants and dishes; filter cuisine and open-now restaurants.
- Nearest restaurant sorting based on user coordinates (Haversine distance).
- Browser geolocation or manually entered coordinates.
- Menu, cart quantity controls and switching restaurants.
- Checkout with server-side item/price validation.
- Promo codes: `WELCOME10` gives 10% off up to ₹100; `YUMMY50` gives ₹50 off on a ₹299+ subtotal.
- Delivery fee ₹35; free delivery on subtotal ₹399+.
- Automatically assigns the nearest available delivery partner to the restaurant.
- Order history and delivery status demo: Order placed → Preparing food → Picked up → Delivered.
- Partner becomes available after delivery; customers can rate delivered orders.
- Responsive layout for mobile and desktop.

## Simple low-level design

| Component | Responsibility |
|---|---|
| Restaurant | Restaurant profile, cuisine, menu, opening status and location |
| MenuItem | Dish name, price and vegetarian flag |
| Customer | Checkout name, address and chosen location |
| Cart | Selected menu items and quantities |
| Order | Items, subtotal, coupon, total, status and assigned partner |
| DeliveryPartner | Location, vehicle and availability |
| Distance utility | Calculates straight-line distance with the Haversine formula |
| Order API | Validates checkout, calculates price and assigns a partner |

### Order flow

1. Customer sets location and browses nearest restaurants.
2. Customer selects an open restaurant and adds menu items.
3. Server validates the restaurant and every item/quantity; prices come from server-side menu data.
4. Server calculates coupon, delivery fee and total.
5. Server selects the nearest available delivery partner to the restaurant.
6. Customer advances the demo order through its statuses.
7. Once delivered, the partner becomes available again and the customer can rate the order.

## API

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/api/health` | Health check |
| GET | `/api/restaurants?lat=22.25&lng=84.9` | Restaurants sorted by distance |
| GET | `/api/orders` | Order history |
| POST | `/api/orders` | Place an order |
| PATCH | `/api/orders/:id/status` | Advance demo order status |
| POST | `/api/orders/:id/rating` | Rate a delivered order |

## Patterns and design choices

- **Separation of concerns:** browser UI and HTTP API are separate.
- **Single source of truth for prices:** the server resolves dish IDs to its own menu prices.
- **Strategy-like selection:** available partners are filtered and sorted by distance before assignment.
- **State transition:** orders move through a fixed ordered set of statuses.

## Important limitations

This is a working MVP/demo, not a production clone. Restaurants, partners and orders are sample data; orders and partner availability are stored in server memory and reset on restart. It does not have real customer/restaurant/driver accounts, database persistence, real payment gateway, live restaurant dashboards, push notifications, live GPS tracking, mapped road routes or real delivery estimates. Location works on localhost or an HTTPS deployment if browser permission is granted. Never use this demo for real payments or customer data.