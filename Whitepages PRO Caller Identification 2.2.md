# Caller Identification

Identify, prioritize, and connect with callers. Get an incoming caller’s full name, demographics, address data, and phone intelligence in real-time. Using a RESTful GET API request, you’ll receive a comprehensive overview on the incoming caller including phone metadata attributes, who is calling in and from where. For a more detailed response including all the people associated with phone and their respective locations, use the Reverse Phone API.

## Request
**Request format**

```
http://proapi.whitepages.com/2.2/phone.json?phone_number=2069735100&api_key=KEYVAL
```

**Request Parameters**
All parameters are case sensitive.

| KEY     | Description | Examples |
| ------- | ---- | ---- |
| api_key | See here to acquire an API key. REQUIRED | |
| phone_number | Contains a raw unparsed or a formatted phone number. REQUIRED | 2069735184 or 12069735184 or 206-601-3561 |
| country_hint | Contains the ISO-3166-1 alpha-2 code for the phone_number | US or mx or Za |

## Response
A Caller Identification response is formatted as follows:

| Field     | Type | Description |
| ------- | ---- | ---- |
| id | ID | The entity id. REQUIRED |
| phone_number | string | Complete undecorated phone number without extension or country code. Example: “2065551212” |
| extension | string | The extension (numeric characters only) for the phone number. Example “143”  |
| is_valid | boolean | If true, indicates a valid phone number. If false, this is an invalid phone number. Reasons include invalid or missing area code, invalid country code, too short or too long number. If null, current validity of this number is not known |
| is_connected | boolean | If true, this is a connected number. If false, this number is currently disconnected and cannot make/receive calls. If null, we do not have the current status of this phone. |
| country_calling_code | string | International country code (Spec: ITU-T E.164). Example: “1” for USA & Canada |
| line_type | string | Line type can be any of the following: Landline: Traditional wired phone line, FixedVOIP: VOIP based fixed line phones, Mobile: Wireless phone line, Voicemail: Voicemail-only service, TollFree: Callee pays for call, Premium: Caller pays a premium for the call–e.g. 976 area code, NonFixedVOIP: Skype, for example, Other: Anything that does not match the previous categories |
| carrier | string | The company that provides voice and/or data services for this phone number. Example: “AT&T Wireless” |
| do_not_call | boolean | If true, phone number is on the National Do Not Call Registry. Access to this data is restricted to customers who have signed an agreement agreeing to usage terms and conditions. If false or null, phone number is not on the National Do Not Call Registry |
| is_prepaid | boolean | If true, phone number is associated to a prepaid phone account. If false, phone number is not associated with a prepaid phone account. If null, the prepaid status of the phone is not known. |
| belongs_to | array | A single person or business entity associated with this phone number. |
| associated_locations | array | A single location associated with this phone number. More details on the Location Id data structure can be found below. |


**Associated locations array**

| Field     | Type | Description |
| ------- | ---- | ---- |
| id | Object | key, type, uuid, durability |
| type | String | Type of the address |
| valid_for | Object | Start and End day, month, year when person was associated with address |
| Full address |  | To fully describe the location we return following strings and boolean values: legal_entities_at, city, postal_code, zip4, state_code, country_code, is_receiving_mail, not_receiving_mail_reason, usage, delivery_point, address_type, is_deliverable, standard_address_line1, standard_address_line2, is_historical  |
| lat_long | Object | latitude, longitude, and accuracy. Where accuracy could be: RoofTop, Street, PostalCode, City, State, Country |


## Example
**Request**
```
https://proapi.whitepages.com/2.2/phone.json?phone_number=2069735100&api_key=KEYVAL
```

**Response**
```json
{
  "results": [
    {
      "id": {
        "key": "Phone.4d796fef-a2df-4b08-cfe3-bc7128b6f6bb.Durable",
        "type": "Phone",
        "uuid": "4d796fef-a2df-4b08-cfe3-bc7128b6f6bb",
        "durability": "Durable"
      },
      "line_type": "NonFixedVOIP",
      "belongs_to": [
        {
          "id": {
            "key": "Business.7e29cf66-8bf2-4812-829d-356654b5b3d3.Durable",
            "type": "Business",
            "uuid": "7e29cf66-8bf2-4812-829d-356654b5b3d3",
            "durability": "Durable"
          },
          "name": "Whitepages",
          "locations": null,
          "phones": null
        }
      ],
      "associated_locations": [
        {
          "id": {
            "key": "Location.88e44955-805c-455a-99da-d7444ca5c484.Durable",
            "type": "Location",
            "uuid": "88e44955-805c-455a-99da-d7444ca5c484",
            "durability": "Durable"
          },
          "type": "Address",
          "valid_for": null,
          "legal_entities_at": null,
          "city": "Seattle",
          "postal_code": "98101",
          "zip4": "2625",
          "state_code": "WA",
          "country_code": "US",
          "is_receiving_mail": true,
          "not_receiving_mail_reason": null,
          "usage": "Business",
          "delivery_point": "MultiUnit",
          "address_type": "Highrise",
          "lat_long": {
            "latitude": 47.608624,
            "longitude": -122.334442,
            "accuracy": "RoofTop"
          },
          "is_deliverable": true,
          "standard_address_line1": "1301 5th Ave Ste 1600",
          "standard_address_line2": "",
          "is_historical": false
        }
      ],
      "is_valid": true,
      "phone_number": "2069735100",
      "country_calling_code": "1",
      "carrier": "Level 3 Communications",
      "do_not_call": null,
      "reputation": null,
      "is_prepaid": false,
      "is_connected": null
    }
  ],
  "messages": []
}
```


