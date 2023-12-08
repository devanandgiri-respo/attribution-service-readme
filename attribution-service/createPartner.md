# API Documentation: 

## Create Partner

### Description
- This API endpoint is used to create a new partner by providing necessary information such as partner name, preferred journey type, integration type, address, campaign name, and email.

### Endpoint:
```plaintext
POST /createPartner
```

### Request Body:

- Method: POST
- Headers:
  * Content-Type: application/json
- Body:
  ```json
    {
    "partnerName": "Partner Name",
    "preferredJourneyType": "Preferred Journey Type",
    "integrationType": "Integration Type",
    "address": "Partner Address",
    "campaignName": "Campaign Name",
    "email": "Partner Email"
    }
  ```

### Response 
The API provides JSON responses with the following structure:

- Success Response (HTTP 200 OK):
  ```json
  {
    "partnerId": "generated_partner_id"
  }
  ```
- Bad Request Response (HTTP 400 Bad Request):
  ```json
    {
    "statusCode": 400,
    "message": "Specific error message"
    }
    ```

### Validation :
The API performs validation on the request parameters and returns appropriate error responses for missing or invalid inputs.

- If `partnerName` is missing or empty:
  - Returns HTTP 400 with message: "Partner name is not present."

- If `preferredJourneyType` is missing:
    - Returns HTTP 400 with message: "Preferred journey type is not present."

- If `integrationType` is missing:
  - Returns HTTP 400 with message: "Integration type is not present."

- If `address` is missing:
  - Returns HTTP 400 with message: "Address is not present."

- If email is missing:
   - Returns HTTP 400 with message: "Email is not present."

- If `campaignName` is missing or empty:
   - Returns HTTP 400 with message: "Campaign name is not present."