Pagination is the art of "slicing" your data into manageable chunks. Here is an in-depth breakdown of the three main strategies, explained in simple terms.

---

## 1. Offset-based Pagination

This is the "old school" and most common method. It works exactly like a book: you tell the database which page you want and how many items are on a page.

* **The Logic:** You use two parameters: `limit` (how many items) and `offset` (how many items to skip).
* **The Math:** To get page 3 with 10 items per page:
* `Limit = 10`
* `Offset = (3 - 1) * 10 = 20`


* **API Example:** `GET /products?limit=10&offset=20`

### Pros:

* **Jumping:** Users can skip directly to page 500.
* **Simple:** Very easy to implement in SQL (`LIMIT 10 OFFSET 20`).

### Cons (The "Silent Killers"):

* **Performance:** As the offset gets higher, the database gets slower. To skip 1,000,000 rows, the DB still has to read them all into memory and then throw them away. Complexity is $O(N)$.
* **Data Drift:** If a new item is added to page 1 while a user is moving to page 2, the last item from page 1 "pushes" into page 2. The user sees the same item twice.

---

## 2. Keyset Pagination (Search After)

Instead of saying "skip 20 rows," you say "give me the next 10 items after the last ID I saw."

* **The Logic:** You use a marker (usually a unique ID or a timestamp) from the last item of your current result.
* **API Example:** `GET /products?limit=10&after_id=105`
* **SQL Logic:** `SELECT * FROM products WHERE id > 105 ORDER BY id ASC LIMIT 10`

### Pros:

* **Consistent:** If new items are added, the "marker" stays the same, so no duplicate items.
* **Fast:** The database uses an index to jump straight to ID 105. Performance is $O(1)$ or $O(\log N)$.

### Cons:

* **No "Page 5":** You can't jump to a specific page because you don't know what the ID is for the start of page 5 without fetching pages 1-4 first.
* **Coupling:** The API user needs to know about your internal ID structure.

---

## 3. Cursor-based Pagination

This is the "Gold Standard" used by Facebook, Slack, and Twitter. It’s an evolution of Keyset pagination but highly abstracted.

* **The Logic:** The server sends a "Cursor"—a long, encoded string (usually Base64)—that contains the position of the last item. The client doesn't need to know what's inside the string; they just pass it back.
* **API Example:** `GET /posts?limit=10&cursor=ZXhhbXBsZTEyMw==`

### Pros:

* **Perfect for Infinite Scroll:** Ideal for social media feeds where data changes every second.
* **Encapsulation:** You can change your sorting logic (e.g., sort by "trending score") without breaking the client's code; you just change how you encode the cursor.

### Cons:

* **Complexity:** Harder to implement on the backend.
* **No Random Access:** Again, you can't jump to "Page 10."

---

## The "Minute Details": Metadata & HATEOAS

A professional API doesn't just send a list of items. It sends **metadata** so the frontend knows what to do.

> **HATEOAS (Hypermedia as the Engine of Application State):** This is a fancy way of saying your API should provide the "Next" and "Previous" links directly in the response.

### Real-Time Example: A Grocery App API

If you were building a "Maya’s Mart" app, your response for `GET /items?size=2` might look like this:

```json
{
  "metadata": {
    "total_count": 500,
    "has_next": true,
    "next_cursor": "YmFuYW5hXzEw",
    "links": {
      "next": "https://api.mayasmart.com/v1/items?limit=2&cursor=YmFuYW5hXzEw",
      "self": "https://api.mayasmart.com/v1/items?limit=2"
    }
  },
  "data": [
    {"id": 1, "name": "Apples"},
    {"id": 2, "name": "Bananas"}
  ]
}

```

---

## Strategy Comparison Table

| Feature | Offset-Based | Keyset | Cursor-based |
| --- | --- | --- | --- |
| **Random Access (Jump to Page)** | Yes | No | No |
| **Performance (High Volumes)** | Poor ($O(N)$) | Excellent ($O(1)$) | Excellent ($O(1)$) |
| **Handles Frequent Writes** | No (Duplicates) | Yes | Yes |
| **Ease of Implementation** | Easy | Medium | Hard |
| **Best For** | Admin Dashboards | Search Results | Social Media Feeds |

---

### Which one should you choose?

* If you are building a **back-office tool** where people need to jump to "Page 15," stick with **Offset**.
* If you are building a **high-traffic mobile app** with infinite scroll, go with **Cursor**.

---
[![VIEW Topic](https://img.shields.io/badge/🚀-Preview-blue?style=for-the-badge)](https://htmlpreview.github.io/?https://github.com/Tushar-Nagdive/StackBlueprint/blob/StackTech/System-Design/visual-content/restful-pagination-strategies.html)