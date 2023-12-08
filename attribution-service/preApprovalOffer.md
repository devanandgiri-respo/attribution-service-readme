# API Documentation: 

## Underwriting API - Pre-Approval Offer

### Description:
- This API endpoint is used to generate a pre-approval offer for a customer and attribute the application. It checks for existing applications, generates an offer, and attributes the application accordingly.

### Endpoint:
```plaintext
POST /underwriting/preApprovalOffer
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
    "mobileNumber": "Customer Mobile Number",
    "dob": "Customer Date of Birth",
    "employmentType": "Customer Employment Type",
    "income": "Customer Monthly Income",
    "bureauType": "Bureau Type",
    "monthlyObligation": "Customer Monthly Obligation",
    "bureauData": "Bureau Data",
    "bureauName": "Bureau Name",
    "name": "Customer Name",
    "email": "Customer Email"
    }
  ```

### Response 
The API provides JSON responses with the following structure:

- Success Response (HTTP 200 OK):
  ```json
    {
    "status": "ACCEPT",
    "message": "Customer is eligible for the pre-approval offer.",
    "offer": 5000
    }
  ```

- Rejection Response (HTTP 200 OK):
  ```json
    {
    "status": "REJECT",
    "message": "Customer is not eligible for the pre-approval offer.",
    "offer": null
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

- If required fields (`panNumber`, `partnerId`, `mobileNumber`, `dob`, `employmentType`, `income`, `bureauType`, `monthlyObligation`, `bureauData`, `bureauName`, `name`, `email`) are missing or empty:
  - Returns HTTP 400 with message: "Invalid request format. All required fields must be provided."

- If the `partnerId` is not registered:
  - Returns HTTP 400 with message: "Partner not registered."

- If the mobile number format is invalid:
  - Returns HTTP 400 with message: "Invalid request format. Mobile number format is not valid."

- If the application for the provided PAN and mobile number is not found:
  - Returns HTTP 400 with message: "Success, but dedupe not found."

### Processing Logic:
1. Check if the provided partner ID is registered.
   - If not registered, return a rejection response with message: "Partner not registered."

2. Validate the request payload for required fields.
    - If invalid, return a rejection response with message: "Invalid request format."

3. Normalize the mobile number format to ensure consistency.

4. Query the attribution collection in the MongoDB database to find matching applications.
   - Match either by PAN number or the phone number.

5. Check and process the existing application, if any:

   - If an application is found, check for attribution and generate an offer accordingly.
   - If no application is found, return a rejection response with message: "Success, but dedupe not found."

6. Generate a pre-approval offer for the customer.
   - if the offer is accepted, update the application with offer details and attribution.
