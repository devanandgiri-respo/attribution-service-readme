# API Documentation: 
## Attribution Lock API Documentation

### Description
- The Attribution Lock API is used to lock attribution records for a given user ID. When called, it processes the first cash transfer for the specified user, updates the attribution status, and logs relevant information.

### Endpoint:
```plaintext
POST /attribution/lock
```

### Request Body:

- Method: POST
- Headers:
  * Content-Type: application/json
- Body:
  ```json
  {
  "userId": "123456"
  }
  ```

### Response 
The API provides JSON responses with the following structure:

- The API returns a simple HTTP status code as a response.
  - Success Response:
    - `200 OK`: The attribution was locked successfully.

  - Error Response:
    - `500 Internal Server Error`: An internal error occurred during the attribution locking process.

### Implementation Details:
- This function handles the incoming POST requests to the `/attribution/lock` endpoint. It logs information about the attribution locking process, creates an instance of the `SingularAttributionService`, and invokes the `processFirstCashTransfer` method.

`processFirstCashTransfer` Method
- This method processes the first cash transfer for a specified user ID, updates the attribution status, and logs relevant information.

### Processing Logic:
1. Retrieve the phone number associated with the user ID.

2. Check if the attribution exists for the user ID or phone number.

3. If the attribution is not locked, check if it is already attributed.

4. If attributed, update the `disbursedAt` date without modifying the attribution.

5. If not attributed, update the attribution status to "LOCKED," set the `disbursedAt` date, and specify the attribution source.

6. Log relevant information about the attribution locking process.

### Additional Methods: 
- `fetchPhoneNumberByCustomerId`: Retrieves the phone number associated with a given user ID.
- `isAttributionLocked`: Checks if the attribution is already locked.
- `isAttributed`: Checks if the attribution is already attributed.
- `updateHistory`: Updates the attribution history with relevant information.
