![Normify Logo](https://res.cloudinary.com/dvhsgzydx/image/upload/v1/static/images/favicons/normify_logo_cropped.33ab5f10799b.png)

# Normify API Documentation

## Overview

The Normify API allows you to retrieve current version dates and changes for laws and standards. This API serves to update the version dates of standards and laws in your database.

## Base URL

```
TBD
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
Endpoint: https://app.normify.me/api/token/
Content-Type: application/json
Body (raw json):
{
    "email": "YOUREMAIL",
    "password": YOURPASSWORD"
}

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
- `id` : Unique ID of the law or standard
- `identifier` : Unique identifier of the law or standard
- `short_title` : Title of the law or standard
- `version_date` : Current version date in your system
- `last_change` : Date of last change in your system


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
- `current_version_date`: Current version date
- `retracted`: Indicator if this law/standard was rectracted (not in effect anymore).
- `has_newer_version`: Boolean indicating whether a newer version than the provided date is available
- `source`: Link to standard/law url
- `changes`: Array of changes implemented since the provided date

## Examples

Further examples can be found in the [Example folder](./Examples/).

### Example 1: Query with ID

**Request:**
```

  curl -v -X POST https://app.normify.me/research/api/ultimate/ \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNzU0NDg0MjY1LCJpYXQiOjE3NTE4OTIyNjUsImp0aSI6IjBhMDhmNGQ0NGExYzQ3YmE5NDM2OTczZTEwYWUwYjNlIiwidXNlcl9pZCI6NDl9.6yPf6x1A6gHxTc-hITgF2pFrOrzXag5aE9_cNvf0hJk" \
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


## Versioning

The current API version is v1. All future changes will be provided through new versions to ensure backward compatibility.
