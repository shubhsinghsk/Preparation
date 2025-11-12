🔌 APIs & REST — The Heart of Modern Backend Development
🧠 1. What Is an API?

API stands for Application Programming Interface.

Think of it as a menu in a restaurant 🍽️:

You (the client) don’t go into the kitchen.

You look at the menu (API documentation).

You place an order (send a request).

The kitchen (server) prepares your food (data).

The waiter (HTTP) delivers it back to you.

💡 In short:

API = A set of rules that lets software applications talk to each other.

🧩 2. Types of APIs
Type	Used For	Example
Web API	Communication over the web using HTTP	REST, GraphQL
Library API	Functions inside a programming library	NumPy, Pandas
OS API	System-level functions	Windows API, Android API
Remote API	Accessing services over a network	Twitter API, AWS API

As a Python backend developer, you’ll mostly work with Web APIs — especially RESTful APIs.

🌍 3. What Is a REST API?

REST stands for Representational State Transfer — a set of design principles for building APIs that are:
✅ Simple
✅ Scalable
✅ Stateless
✅ Resource-Oriented

REST is not a protocol — it’s a style of designing APIs over HTTP.

🧱 4. RESTful API — The Core Principles

Here are the 6 golden rules that make an API RESTful 👇

Principle	Description	Example
1. Client–Server Architecture	Keep client and server separate	Client (frontend) ↔ Server (backend)
2. Statelessness	Each request is independent	Server doesn’t store session data
3. Cacheability	Responses can be cached to improve speed	HTTP headers like Cache-Control
4. Uniform Interface	Consistent structure and naming	/users/1 always refers to same kind of resource
5. Layered System	Multiple layers (proxy, load balancer, etc.) shouldn’t affect communication	Client unaware of backend layers
6. Code on Demand (Optional)	Server can send executable code	e.g., JavaScript from server to browser
🧠 5. What Makes an API RESTful?

A RESTful API must:

Use HTTP methods properly (GET, POST, PUT, DELETE, PATCH)

Be resource-based, not action-based

Use stateless communication

Return representations (JSON, XML, etc.)

Follow clear, predictable URLs

Use HTTP status codes correctly

🗂️ 6. REST Resource and URL Design

A resource is any entity or object you want to expose — user, product, order, etc.

✅ Good RESTful URLs:
Action	HTTP Method	Endpoint	Description
Get all users	GET	/users	Returns list of users
Get one user	GET	/users/5	Returns user with ID=5
Create user	POST	/users	Creates a new user
Update user	PUT	/users/5	Replaces user data
Partial update	PATCH	/users/5	Updates a field
Delete user	DELETE	/users/5	Removes the user

💡 Don’t use verbs in URLs
❌ /getAllUsers
✅ /users

🧾 7. RESTful Request and Response Example
📨 Client Request (POST)
POST /users HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "name": "Shubham Singh",
  "role": "Developer"
}

💬 Server Response
HTTP/1.1 201 Created
Content-Type: application/json

{
  "id": 101,
  "name": "Shubham Singh",
  "role": "Developer"
}


👉 Clean, predictable, and uses HTTP methods correctly. That’s RESTful!

📊 8. Common HTTP Status Codes in REST APIs
Code	Meaning	When to Use
200 OK	Success	Data retrieved or updated
201 Created	New resource created	After POST
204 No Content	Successful but no data returned	After DELETE
400 Bad Request	Invalid input	Wrong parameters
401 Unauthorized	Missing/invalid credentials	Login required
403 Forbidden	Access denied	No permission
404 Not Found	Resource doesn’t exist	Invalid ID
500 Internal Server Error	Server failed	Bug or issue
⚙️ 9. RESTful vs Non-RESTful APIs
Feature	RESTful API	Non-RESTful API
Architecture	Follows REST principles	No strict rules
Communication	Stateless	May store session info
Data Format	JSON/XML	Any format
URL Design	Resource-oriented	Often verb-based
Scalability	High	Usually limited
🧑‍💻 10. Build a Mini REST API with Flask

Try this — it’s super fun 🎯

from flask import Flask, jsonify, request

app = Flask(__name__)

users = [{"id": 1, "name": "Shubham"}, {"id": 2, "name": "Vivek"}]

@app.route('/users', methods=['GET'])
def get_users():
    return jsonify(users)

@app.route('/users/<int:id>', methods=['GET'])
def get_user(id):
    user = next((u for u in users if u["id"] == id), None)
    return jsonify(user) if user else ("User not found", 404)

@app.route('/users', methods=['POST'])
def create_user():
    new_user = request.json
    users.append(new_user)
    return jsonify(new_user), 201

if __name__ == '__main__':
    app.run(debug=True)


Now run it and visit:

🔹 GET http://127.0.0.1:5000/users → Get all users

🔹 POST http://127.0.0.1:5000/users → Add a new one

Boom 💥 — you’ve made your own RESTful API!

🧠 11. RESTful API Best Practices

✅ Use nouns, not verbs, in URLs
✅ Always use plural for resources (/users, not /user)
✅ Send/receive JSON (most common format)
✅ Handle errors clearly with status codes
✅ Use versioning in your API (/api/v1/users)
✅ Secure your API using authentication tokens (JWT)
✅ Use pagination for large datasets (/users?page=2&limit=50)

🧩 12. Advanced REST Concepts (Beyond Basics)
Concept	Description
HATEOAS	Hypermedia as the Engine of Application State → API should return links to related resources
Pagination & Filtering	Divide large data into pages for performance
Rate Limiting	Restrict how many requests a client can send
Versioning	/api/v1/ → manage backward compatibility
Authentication	Use tokens (JWT, OAuth) to secure endpoints
🧾 13. REST vs GraphQL vs SOAP (Quick Compare)
Feature	REST	GraphQL	SOAP
Simplicity	✅ Easy	⚙️ Medium	❌ Complex
Flexibility	Limited	Very high	Limited
Data Format	JSON/XML	JSON only	XML
Over-fetching	Possible	❌ Avoided	Possible
Use Case	Web/Mobile APIs	Modern apps needing flexibility	Enterprise systems
🧩 14. Quick Recap
Concept	Key Point
API	Interface between two software components
REST	Architectural style for designing APIs
RESTful API	Follows REST rules — stateless, resource-based
HTTP Methods	GET, POST, PUT, PATCH, DELETE
Status Codes	Communicate result clearly
Best Practices	Use JSON, plural nouns, clear endpoints
Flask Demo	Easy way to test REST APIs locally
🎯 15. Fun Practice Tasks

Try these mini projects:

Build a Book Store API with CRUD operations.

Add search, pagination, and JWT authentication.

Create a /status endpoint that returns:

{"status": "Running", "uptime": "120s"}

🏁 16. Key Takeaway

A RESTful API is like a well-organized restaurant —
clear menu, consistent service, stateless orders, and fast delivery 🍕⚡
