![Normify Logo](https://res.cloudinary.com/dvhsgzydx/image/upload/v1/static/images/favicons/normify_logo_cropped.33ab5f10799b.png)

# Normify API Documentation

## Overview

The Normify API allows you to retrieve current version dates and changes for laws and standards. This API serves to update the version dates of standards and laws in your database.

## Base URL

```
TBD
```

## Authentication

The API uses API keys for authentication. Include your API key in the header of each request:

```
Authorization: Bearer YOUR_API_KEY
```

## Endpoints

### Query Laws and Standards

**POST** `/standards/check`

Checks the current version dates and changes for a law or standard.

#### Request Body

```json
{
  "id": "string",
  "title": "string", 
  "version_date": "YYYY-MM-DD"
}
```

**Parameters:**
- `id` : Unique ID of the law or standard
- `title` : Title of the law or standard
- `version_date` (optional): Current version date in your system

#### Response

```json
{
  "success": true,
  "data": {
    "id": "string",
    "title": "string",
    "current_version_date": "YYYY-MM-DD",
    "has_newer_version": boolean,
    "changes": [
      {
        "description": "string",
        "section": "string",
        "effective_date": "YYYY-MM-DD"
      }
    ],
    "last_updated": "YYYY-MM-DDTHH:MM:SSZ"
  }
}
```

**Response Fields:**
- `id`: Unique ID of the law/standard
- `title`: Title of the law/standard
- `current_version_date`: Current version date
- `has_newer_version`: Boolean indicating whether a newer version than the provided date is available
- `changes`: Array of changes implemented since the provided date
- `last_updated`: Timestamp of the last update

<!-- #### Change Types

The API supports various change types:

- `ADDITION`: New sections or provisions added
- `MODIFICATION`: Existing provisions modified
- `DELETION`: Provisions removed
- `CLARIFICATION`: Clarifications or specifications
- `REORGANIZATION`: Restructuring of sections -->

<!-- ## Examples

### Example 1: Query with ID

**Request:**
```bash
curl -X POST https://api.normify.com/v1/standards/check \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "id": "DIN_EN_ISO_9001_2015",
    "version_date": "2023-01-15"
  }'
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "DIN_EN_ISO_9001_2015",
    "title": "DIN EN ISO 9001:2015 - Quality Management Systems",
    "current_version_date": "2023-12-01",
    "has_newer_version": true,
    "changes": [
      {
        "change_type": "MODIFICATION",
        "description": "Update of quality management requirements",
        "section": "Chapter 8.1",
        "effective_date": "2023-12-01"
      },
      {
        "change_type": "ADDITION",
        "description": "New requirements for digital documentation",
        "section": "Chapter 7.5",
        "effective_date": "2023-12-01"
      }
    ],
    "last_updated": "2023-12-01T10:30:00Z"
  }
}
```

### Example 2: Query with Title

**Request:**
```bash
curl -X POST https://api.normify.com/v1/standards/check \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "General Data Protection Regulation",
    "version_date": "2022-06-01"
  }'
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "EU_2016_679",
    "title": "General Data Protection Regulation (GDPR)",
    "current_version_date": "2022-06-01",
    "has_newer_version": false,
    "changes": [],
    "last_updated": "2022-06-01T00:00:00Z"
  }
}
``` -->

## Error Handling

### HTTP Status Codes

- `200 OK`: Request successful
- `400 Bad Request`: Invalid request (missing parameters, invalid date format)
- `401 Unauthorized`: Invalid or missing API key
- `404 Not Found`: Law or standard not found
- `429 Too Many Requests`: Rate limit exceeded
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
- `RATE_LIMIT_EXCEEDED`: Too many requests in a short time

## Rate Limiting

The API is limited to 100 requests per minute per API key. A 429 status code will be returned when exceeded.

## Versioning

The current API version is v1. All future changes will be provided through new versions to ensure backward compatibility.

## Support

For questions or issues, please contact:

- **Email:** prang@normify.com

## Changelog

### v1.0.0 (2024-01-01)
- Initial API documentation release