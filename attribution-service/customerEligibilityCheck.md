# API Documentation: 

## Underwriting API - Customer Eligibility Check

### Description:
- This API endpoint is used to perform a customer eligibility check for underwriting. It checks the eligibility of a customer based on provided information such as PAN number, partner ID, and masked phone number.

### Endpoint:
```plaintext
POST /underwriting/customerEligibility
```

### Request Body:

- Method: POST
- Headers:
  * Content-Type: application/json
- Body:
  ```json
    {
    "panNumber": "Customer PAN Number",
    "partnerId": "Partner ID",
    "mobileNumber": "Masked Phone Number"
    }
  ```

### Response 
The API provides JSON responses with the following structure:

- Success Response (HTTP 200 OK):
  ```json
    {
    "status": "ACCEPT",
    "message": "Customer is eligible for the application."
    }
  ```

- Rejection Response (HTTP 200 OK):
  ```json
    {
    "status": "REJECT",
    "message": "Customer is not eligible for the application."
    }
  ```

- Bad Request Response (HTTP 400 Bad Request):
  ```json
  {
  "statusCode": 400,
  "message": "Specific error message"
  }
  ```
- Internal Server Error Response (HTTP 500 Internal Server Error):
  ```json
  {
  "statusCode": 500,
  "message": "Internal server error occurred."
  }
  ```

### Validation:
The API performs validation on the request parameters and returns appropriate error responses for missing or invalid inputs.

- If `panNumber`, `partnerId`, or `mobileNumber` is missing or empty:
  - Returns HTTP 400 with message: "Invalid request format. PAN number, Partner ID, and Masked Phone Number are required."

- If the `partnerId` is not registered:
  - Returns HTTP 400 with message: "Partner not registered."

- If the eligibility request is invalid:
  - Returns HTTP 400 with message: "Invalid request format."


### Processing Logic:
1. Check if the provided partner ID is registered.
   - If not registered, return a rejection response with message: "Partner not registered."

2. Validate the eligibility request.
    - If invalid, return a rejection response with message: "Invalid request format."

3. Extract information from the request payload.
    - PAN number (`panNumber`)
    - Partner ID (`partnerId`)
    - Masked Phone Number (`mobileNumber`)

4. Query the attribution collection in the MongoDB database to find matching applications.
   - Match either by PAN number or the last X digits of the masked phone number.

5. Check and process the existing application, if any:

   - If the same partner already applied with the lead PAN, check the application status.
     - If the application is rejected, return a response with status: "REJECT".
     - If the application is not rejected, return a response with status: "APPLICATION_ALREADY_EXIST".
   - If the application is from a different partner but with the same PAN, check attribution and reject if needed.
   - If no existing application, accept the new application.
