# ⚡ Automation in Postman
Educational repository demonstrating API test automation workflow in Postman (Happy Path & dynamic data chaining).
## 🎯 Project Goal
This project presents automation of functional testing of web application API (Happy path).\
\
The collection covers the full CRUD lifecycle of a "Book" entity, demonstrating integration between endpoints and dynamic data passing within a single execution session.
## 📈 Test Scenario (Happy Path Workflow)
The requests are executed in a strict chronological sequence using the Postman Collection Runner:
1. **POST /auth** — User authentication, generating an auth token and dynamically saving it to environment variables.
2. **POST /booking** — Creating a new book record, automatic variables generation in the request body, response parsing, and storing the extracted values into environment variables.
3. **GET /booking** — Fetching the entire book catalog, verifying that the newly created book is present in the list.
4. **GET /books?firstname={{firstname}}&lastname={{lastname}}** — Verifying query parameters and data filtering logic. Filtering by specific author (by first and last name). Verifying that the newly created book is present in the list.
5. **GET /books?checkin={{checkin}}&checkout={{checkout}}** — Verifying query parameters and data filtering logic. Filtering by checkin/checkout date. Verifying that the newly created book is present in the list.
6. **GET /booking/{id}** — Retrieving the newly created book by its stored dynamic ID. Verifying that the response fields match the expected book parameters saved in the environment variables.
7. **PUT /booking/{id}** — Performing a full update of the book's details, passing the auth token in the request headers, automatic variables generation in the request body, response parsing, and storing the extracted values as a new values ​​of environment variables.
8. **GET /booking/{id}** — Verifying the applied changes from the PUT request.
9. **PATCH /booking/{id}** — Executing a partial update on specific book fields. Passing the auth token in the request headers.
10. **GET /booking/{id}** — Verifying the applied changes from the PATCH request.
11. **DELETE /booking/{id}** — Removing the book entity from the system. Passing the auth token in the request headers.
12. **GET /booking/{id}** — Verifying exception handling after deletion (expecting a `404 Not Found` response) and clearing the environment variables.
## ⚙️ Implemented QA Practices
* **Dynamic Data Management:** Handled runtime passing of the authorization token, generated book ID and other parameters across requests via `pm.environment.set()`. No hardcoded parameters or static tokens are used in the core requests.
* **Environment Parameterization:** Fully parameterized the collection using a dedicated Postman Environment. Critical data, including the `{{base_url}}`, authentication tokens, and book test data, are dynamically fetched from environment variables.
* **Header-Based Authorization:** The authentication token is dynamically passed within the request headers across endpoints, ensuring secure access control.

## ▶️ How to Run the Project

1. Clone this repository.
2. Import the `Booker.postman_collection.json` and `Booker_Environment.postman_environment.json` files into Postman.
3. Ensure that the correct environment is selected and the `base_url` variable in your active Postman Environment is set to the correct target server.
4. Right-click the collection -> select **Run**.
5. Click **Start Run**.

To run via CLI (**Newman**) use the following command:
```bash
   newman run Booker.postman_collection.json -e Booker_Environment.postman_environment.json
   ```
