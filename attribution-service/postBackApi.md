# API Documentation: 
## Singular Postback API Documentation

### Description
- The Singular Postback API is used to handle postback events from the Singular attribution service. It processes the incoming attribution data, performs necessary validations, and updates the relevant collections in the MongoDB database.

### Endpoint:
```plaintext
POST /singular/postback
```

### Request Body:

- Method: POST
- Headers:
  * Content-Type: application/json
- Body for android:
  ```json
    {
    "event_name": "SIGN_UP",
    "arguments": {
        "userId": "21"
    },
    "amount": 937656.73,
    "app_name": "Cardguard",
    "app_version": 54,
    "campaign": "zype",
    "city": "Salto",
    "click_ip": "212.121.53.188",
    "click_utc_timestamp": 61503107592,
    "country": "Argentina",
    "currency": "ARS",
    "device_brand": "samsung",
    "device_model": "SM-A127F",
    "event_utc_timestamp": 438393653799,
    "install_utc_timestamp": 183683427463,
    "is_first_event": false,
    "is_organic": false,
    "is_reengagement": false,
    "is_viewthrough": true,
    "limit_ad_tracking": false,
    "longname": "com.zype.mobile",
    "network": "Organic",
    "os_version": 22,
    "partner_site": "Organic",
    "platform": "Android",
    "site": "Organic",
    "tracker_name": "zype"
    }
  ```
- Body for iOS:
  ```json
    {
    "amount": 1000,
    "app_name": "Zype",
    "app_version": "2.0.1",
    "campaign": "zype",
    "city": "Delhi",
    "click_ip": "203.192.214.81",
    "click_utc_timestamp": 1695360414,
    "country": "IN",
    "currency": "INR",
    "device_brand": "Apple",
    "device_model": "iPhone12,8",
    "event_name": "SIGN_UP",
    "event_utc_timestamp": 1695360433,
    "idfa": "00000000-0000-0000-0000-000000000000",
    "idfv": "88BA0278-9EAE-4F9E-9260-F78CE4A29FFD",
    "install_utc_timestamp": 1695360414,
    "is_first_event": 1,
    "is_organic": 1,
    "is_reengagement": 0,
    "is_viewthrough": 0,
    "limit_ad_tracking": 1,
    "longname": "com.zype.mobile",
    "network": "Organic",
    "os_version": 17,
    "partner_site": "Organic",
    "platform": "iOS",
    "singular_click_id": "",
    "site": "Unknown",
    "tracker_name": "zype",
    "user_id": 536206
    }
  ```

### Response 
The API provides JSON responses with the following structure:

- Success Response (HTTP 200 OK):
  ```json
    {
    "status": "success",
    "message": "Attribution processed successfully"
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

- Internal Server Error Response (HTTP 500 Internal Server Error):
  ```json
  {
  "statusCode": 500,
  "message": "Internal server error occurred."
  }
  ```

### Implementation Details:
- This function handles the incoming POST requests coming from SINGULAR API using the `/singular/postback` endpoint. It logs the received attribution data, creates an instance of the `SingularAttributionService`, and processes the postback data using the `processSingularPostCallBack` method.

`processSingularPostCallBack` Method
- This method processes the Singular postback data, performs database operations, and returns an appropriate response.

### Processing Logic:
1. If the attribution has fraud status as `REJECT_ATTRIBUTION_EVENT`, it is stored in the `fraudCustomerDetailsCollection`, and a response with status `FRAUD_CUSTOMER_DETECTED` is sent.

2. If the event is not a `SIGN_UP_ATTRIBUTION_EVENT`, it is stored in the `singularEventsCollection`, and a response with status `SUCCESS` is sent.

3. If the device type is not found in the `deviceType` dictionary, a response with status `DEVICE_PLATFORM_NOT_FOUND` is sent.

4. Device metadata and other metadata are extracted from the attribution data.

5. Attribution processing is carried out based on the event type, device type, and user information.

6. If the user is already attributed, a message is sent, and the response indicates success.

7. If the user is not attributed, a new attribution record is created, and the response indicates success.

### Additional Methods: 
- `fetchPhoneNumberByCustomerId`: Retrieves the phone number associated with a given user ID.
- `createOrUpdateAttribution`: Creates or updates an attribution record based on the provided parameters.
- `isAttributionLocked`: Checks if the attribution is already locked.
- `isAttributed`: Checks if the attribution is already attributed.
