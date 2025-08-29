
<img width="200"  alt="pb_normify_grey" src="https://github.com/user-attachments/assets/b3777e1c-20a3-42e7-a7ff-c8af17c0ca94" />

<img width="200"  alt="pb_normify_white" src="https://github.com/user-attachments/assets/56a453c7-5417-4e73-bc45-857c1a6c9062" />

# Normify API Documentation

## Overview

The Normify API allows you to retrieve current version dates and changes for laws and standards. This API serves to update the version dates of standards and laws in your database.

## Base URL

```
https://app.normify.me/research/api/ultimate
```

## Headers

```
Content-Type: "application/json"
```

### Authentication

The API uses API keys for authentication. Include your API key in the header of each request:

```
Authorization: Bearer YOUR_API_KEY
```

#### How to get the bearer token

Send a post request to:

**Endpoint** `https://app.normify.me/api/token/`

**Content-Type** `application/json`

**Body (raw json)**
```json
{
    "email": "YOUREMAIL",
    "password": "YOURPASSWORD"
}
```

This returns a JSON with a refresh token and an access token. Use the access token for authorization as the bearer token. It has a limited lifetime so it should be fetched once you start a new API request process.

## Endpoints

### Query Laws and Standards

**POST** `https://app.normify.me/research/api/ultimate`

Checks the current version dates and changes for a law or standard.

#### Request Body

```json
{
  "id": "int",
  "identifier": "string",
  "type": "string",
  "short_title": "string", 
  "version_date": "YYYY-MM-DD",
  "last_change": "YYYY-MM-DD"
}
```

**Parameters:**
- `id` : Unique ID of the law or standard (required)
- `identifier` : Unique identifier of the law or standard (required)
- `short_title` : Title of the law or standard (not required but improves response quality)
- `version_date` : Current version date in your system (not required)
- `last_change` : Date of last change in your system (not required)

The version_date and last_change are not required. If they are not sent then has_newer_version will always be true. 
If both the version_date and last_change are sent the new version date is compared to both dates and the has_newer_version boolean is set accordingly:

```
if current_version_date > last_change OR current_version_date > version_date
  has_newer_version = True
else
  has_newer_version = False
```

#### Response

```json
{
  "success": "boolean",
  "data": {
    "id": "number/null",
    "identifier": "string",
    "short_title": "string",
    "current_version_date": "YYYY-MM-DD",
    "retracted": "boolean",
    "source": "string",
    "has_newer_version": "boolean",
    "changes": [
      {
        "change_note": "string",
        "retraction_note": "string",
        "effective_date": "YYYY-MM-DD",
        "link": "string"
      }
    ],
  }
}
```

**Response Fields:**
- `id`: Unique ID of the law/standard
- `identifier`: Unique identifier of the law/standard
- `title`: Title of the law/standard
- `current_version_date`: Current version date (Publishing date/Version date)
- `retracted`: Indicator if this law/standard was rectracted (not in effect anymore).
- `has_newer_version`: Boolean indicating whether a newer version than the provided date is available
- `source`: Link to standard/law url
- `changes`: Array of changes implemented since the provided date. The fields here are the change_note, the retraction_note, the effective date and the link for that change.

## Examples

Further examples can be found in the [Example folder](./Examples/).

### Example 1: Query with ID

**Request:**
```

  curl -v -X POST https://app.normify.me/research/api/ultimate/ \
  -H "Authorization: Bearer YOUR_BEARER_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "identifier": "DIN_EN_ISO_9001_2015",
    "version_date": "2023-01-15"
  }'
```

## Error Handling

### HTTP Status Codes

- `200 OK`: Request successful
- `400 Bad Request`: Invalid request (missing parameters, invalid date format)
- `401 Unauthorized`: Invalid or missing API key
- `404 Not Found`: Law or standard not found
- `500 Internal Server Error`: Server error

### Error Response Format

```json
{
  "success": false,
  "error": {
    "code": "string",
    "message": "string",
    "details": {}
  }
}
```

### Common Error Codes

- `MISSING_PARAMETERS`: At least one parameter is required
- `INVALID_DATE_FORMAT`: Invalid date format
- `STANDARD_NOT_FOUND`: Law or standard not found
