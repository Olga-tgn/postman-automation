# ⚡ Automation in Postman
Educational repository demonstrating API test automation workflow in Postman (Happy Path & dynamic data chaining).
## 🎯 Project Goal
This project presents automation of functional testing of web application API (Happy path).\
\
The collection covers the full CRUD lifecycle of a "Booking" entity, demonstrating integration between endpoints and dynamic data passing within a single execution session.
## 📈 Test Scenario (Happy Path Workflow)
The requests are executed in a strict chronological sequence using the Postman Collection Runner:
1. **POST /auth** — User authentication, generating an auth token and dynamically saving it to environment variables.
2. **POST /booking** — Creating a new booking record (the server automatically generates the ID), automatic variables generation in the request body, response parsing, and storing the extracted values in environment variables.
3. **GET /booking** — Fetching the entire list of booking IDs, verifying that the newly created booking is present in the list.
4. **GET /booking?firstname={{firstname}}&lastname={{lastname}}** — Verifying query parameters and data filtering logic. Filtering by specific customer (by first and last name). Verifying that the newly created booking is present in the list.
5. **GET /booking?checkin={{checkin}}&checkout={{checkout}}** — Verifying query parameters and data filtering logic. Filtering by checkin/checkout date. Verifying that the newly created booking is present in the list.
6. **GET /booking/{{bookingid}}** — Retrieving the newly created booking by its stored dynamic ID. Verifying that the response fields match the expected booking parameters saved in the environment variables.
7. **PUT /booking/{{bookingid}}** — Performing a full update of the booking details, passing the auth token in the request headers, automatic variables generation in the request body, response parsing, and storing the extracted values as new values ​​of environment variables.
8. **GET /booking/{{bookingid}}** — Verifying the applied changes from the PUT request.
9. **PATCH /booking/{{bookingid}}** — Executing a partial update on specific booking fields. Passing the auth token in the request headers.
10. **GET /booking/{{bookingid}}** — Verifying the applied changes from the PATCH request.
11. **DELETE /booking/{{bookingid}}** — Removing the booking entity from the system. Passing the auth token in the request headers.
12. **GET /booking/{{bookingid}}** — Verifying exception handling after deletion (expecting a `404 Not Found` response) and clearing the environment variables.
## ⚙️ Implemented QA Practices
* **Dynamic Data Management:** Handled runtime passing of the authorization token, capturing the server-generated booking ID and managing other parameters across requests via `pm.environment.set()`. No hardcoded parameters or static tokens are used in the core requests.
* **Environment Parameterization:** Fully parameterized the collection using a dedicated Postman Environment. Critical data, including the `{{base_url}}`, authentication tokens, and booking test data, are dynamically fetched from environment variables.
* **Contract Testing (JSON Schema Validation):** Implemented structural response validation using JSON Schemas for key endpoints. This ensures that the backend responses strictly comply with the expected data types and formats (e.g., verifying fields, objects, and nested elements).
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
## 🪲 Discovered API Defect (Known Issue)
During the execution of the test suite via Postman Collection Runner, a real backend defect was successfully identified:

* **Failing Step:** `Step 5: GET /booking?checkin={{checkin}}&checkout={{checkout}}`
* **Error Message:** `AssertionError: expected false to be true` (Created booking is present in the list).
* **Root Cause:** This training API environment (Restful-Booker) has a known, documented bug in its server-side filtering logic. The backend fails to process date query parameters correctly and does not include the newly created booking ID in the filtered response list.
* **QA Value:** The failure of this specific test proves the effectiveness and precision of the regression script, demonstrating its ability to catch real application bugs.
