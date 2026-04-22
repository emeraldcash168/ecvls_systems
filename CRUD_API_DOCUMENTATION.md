# CRUD API Documentation

**Version**: 1.0  
**Base URL**: `/api/vehicles`  
**Content-Type**: `application/json`

---

## Overview

The Vehicle Management API provides comprehensive CRUD operations for managing vehicle inventory. It follows RESTful principles with consistent error handling, caching, and security measures.

---

## Authentication

All endpoints require a valid session cookie. Admin role is required for mutations (POST, PUT, DELETE).

```http
Cookie: ec-vms-session=<encrypted-session-data>
```

---

## Endpoints

### 1. List Vehicles (GET)

```http
GET /api/vehicles?limit=100&offset=0&category=Cars&brand=Toyota
```

**Query Parameters**:

| Parameter | Type | Description | Default |
|-----------|------|-------------|---------|
| `limit` | number | Max records to return (max 1000) | 100 |
| `offset` | number | Records to skip | 0 |
| `category` | string | Filter by category (case-insensitive) | - |
| `brand` | string | Filter by brand (partial match) | - |
| `model` | string | Filter by model (partial match) | - |
| `condition` | string | "New", "Used", or "Other" | - |
| `yearMin` | number | Minimum year (1900-2100) | - |
| `yearMax` | number | Maximum year (1900-2100) | - |
| `priceMin` | number | Minimum price | - |
| `priceMax` | number | Maximum price | - |
| `color` | string | Filter by color | - |
| `bodyType` | string | Filter by body type | - |
| `taxType` | string | Filter by tax type | - |
| `searchTerm` | string | Search brand, model, plate | - |
| `withoutImage` | boolean | Filter vehicles without images | - |
| `orderBy` | string | Sort column | "id" |
| `orderDirection` | string | "ASC" or "DESC" | "ASC" |

**Response** (200 OK):

```json
{
  "success": true,
  "data": [
    {
      "VehicleId": "123",
      "Category": "Cars",
      "Brand": "Toyota",
      "Model": "Camry",
      "Year": 2023,
      "Plate": "ABC-1234",
      "PriceNew": 35000,
      "Price40": 14000,
      "Price70": 24500,
      "TaxType": "VAT",
      "Condition": "New",
      "BodyType": "Sedan",
      "Color": "Silver",
      "Image": "https://res.cloudinary.com/.../image.jpg",
      "Time": "2024-01-15T10:30:00Z"
    }
  ],
  "meta": {
    "requestId": "req_abc123",
    "durationMs": 45,
    "total": 150,
    "limit": 100,
    "offset": 0,
    "totalPages": 2,
    "queryCount": 1,
    "cacheHit": false,
    "countsByCategory": {
      "Cars": 80,
      "Motorcycles": 40,
      "TukTuks": 30
    },
    "countsByCondition": {
      "New": 100,
      "Used": 50
    },
    "avgPrice": 28500,
    "noImageCount": 5
  }
}
```

---

### 2. Get Single Vehicle (GET)

```http
GET /api/vehicles/123
```

**Path Parameters**:

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | string | Vehicle ID |

**Response** (200 OK):

```json
{
  "success": true,
  "data": {
    "VehicleId": "123",
    "Category": "Cars",
    "Brand": "Toyota",
    "Model": "Camry",
    "Year": 2023,
    "Plate": "ABC-1234",
    "PriceNew": 35000,
    "Price40": 14000,
    "Price70": 24500,
    "TaxType": "VAT",
    "Condition": "New",
    "BodyType": "Sedan",
    "Color": "Silver",
    "Image": "https://res.cloudinary.com/.../image.jpg",
    "Time": "2024-01-15T10:30:00Z"
  },
  "meta": {
    "requestId": "req_def456",
    "durationMs": 12,
    "queryCount": 1,
    "cacheHit": true
  }
}
```

**Error Response** (404 Not Found):

```json
{
  "success": false,
  "error": "Vehicle with ID 123 not found",
  "meta": {
    "requestId": "req_def456",
    "durationMs": 8,
    "queryCount": 1
  }
}
```

---

### 3. Create Vehicle (POST)

```http
POST /api/vehicles
Content-Type: application/json
```

**Request Body**:

```json
{
  "category": "Cars",
  "brand": "Toyota",
  "model": "Camry",
  "year": 2023,
  "plate": "ABC-1234",
  "market_price": 35000,
  "tax_type": "VAT",
  "condition": "New",
  "body_type": "Sedan",
  "color": "Silver",
  "image_id": "cloudinary_public_id",
  "thumbnail_url": "https://res.cloudinary.com/.../thumb.jpg"
}
```

**Required Fields**: `category`, `brand`, `model`, `year`, `plate`

**Response** (201 Created):

```json
{
  "success": true,
  "data": {
    "VehicleId": "124",
    "Category": "Cars",
    "Brand": "Toyota",
    "Model": "Camry",
    "Year": 2023,
    "Plate": "ABC-1234",
    "PriceNew": 35000,
    "Price40": 14000,
    "Price70": 24500,
    "TaxType": "VAT",
    "Condition": "New",
    "BodyType": "Sedan",
    "Color": "Silver",
    "Image": "https://res.cloudinary.com/.../image.jpg",
    "Time": "2024-01-28T14:30:00Z"
  },
  "meta": {
    "requestId": "req_ghi789",
    "durationMs": 85,
    "queryCount": 2
  }
}
```

**Error Response** (400 Bad Request):

```json
{
  "success": false,
  "error": "Missing required fields: category, brand",
  "meta": {
    "requestId": "req_ghi789",
    "durationMs": 5,
    "queryCount": 0
  }
}
```

---

### 4. Update Vehicle (PUT)

```http
PUT /api/vehicles/123
Content-Type: application/json
```

**Path Parameters**:

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | string | Vehicle ID |

**Request Body** (partial update allowed):

```json
{
  "brand": "Honda",
  "model": "Accord",
  "color": "Blue",
  "market_price": 38000
}
```

**Response** (200 OK):

```json
{
  "success": true,
  "data": {
    "VehicleId": "123",
    "Category": "Cars",
    "Brand": "Honda",
    "Model": "Accord",
    "Year": 2023,
    "Plate": "ABC-1234",
    "PriceNew": 38000,
    "Price40": 15200,
    "Price70": 26600,
    "TaxType": "VAT",
    "Condition": "New",
    "BodyType": "Sedan",
    "Color": "Blue",
    "Image": "https://res.cloudinary.com/.../image.jpg",
    "Time": "2024-01-15T10:30:00Z"
  },
  "meta": {
    "requestId": "req_jkl012",
    "durationMs": 62,
    "queryCount": 1
  }
}
```

**Error Response** (404 Not Found):

```json
{
  "success": false,
  "error": "Vehicle with ID 123 not found",
  "meta": {
    "requestId": "req_jkl012",
    "durationMs": 15,
    "queryCount": 1
  }
}
```

---

### 5. Delete Vehicle (DELETE)

```http
DELETE /api/vehicles?id=123
```

**Query Parameters**:

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | number | Vehicle ID to delete |

**Response** (200 OK):

```json
{
  "success": true,
  "data": true,
  "meta": {
    "requestId": "req_mno345",
    "durationMs": 45,
    "queryCount": 1
  }
}
```

**Error Response** (404 Not Found):

```json
{
  "success": false,
  "error": "Vehicle with ID 123 not found",
  "meta": {
    "requestId": "req_mno345",
    "durationMs": 8,
    "queryCount": 1
  }
}
```

---

## Error Codes

| Status Code | Meaning | Description |
|-------------|---------|-------------|
| 200 | OK | Request successful |
| 201 | Created | Resource created successfully |
| 400 | Bad Request | Invalid input parameters |
| 401 | Unauthorized | Missing or invalid session |
| 403 | Forbidden | Insufficient permissions |
| 404 | Not Found | Resource not found |
| 500 | Internal Server Error | Server error occurred |
| 504 | Gateway Timeout | Request timeout |

---

## Data Types

### Vehicle Object

| Field | Type | Description |
|-------|------|-------------|
| `VehicleId` | string | Unique identifier |
| `Category` | string | Vehicle category (Cars, Motorcycles, etc.) |
| `Brand` | string | Vehicle brand |
| `Model` | string | Vehicle model |
| `Year` | number | Manufacturing year |
| `Plate` | string | License plate number |
| `PriceNew` | number | Market price (USD) |
| `Price40` | number | 40% depreciation price |
| `Price70` | number | 70% depreciation price |
| `TaxType` | string | Tax classification |
| `Condition` | string | "New", "Used", or "Other" |
| `BodyType` | string | Body style |
| `Color` | string | Vehicle color |
| `Image` | string | URL to vehicle image |
| `Time` | string | ISO 8601 timestamp |

### Categories

- `Cars`
- `Motorcycles`
- `TukTuks`
- `Trucks`
- `Vans`
- `Buses`
- `Other`

### Conditions

- `New`
- `Used`
- `Other`

---

## Caching

The API implements intelligent caching:

- **List queries**: 5-second TTL
- **Single records**: 10-second TTL
- **Statistics**: 30-second TTL
- **Cache invalidation**: Automatic on mutations

Cache headers are not exposed to clients; caching is server-side only.

---

## Rate Limiting

Currently no rate limiting is implemented. Recommended for production:

- 100 requests per minute per IP
- 1000 requests per minute per authenticated user

---

## Examples

### JavaScript/TypeScript

```typescript
// List vehicles
const response = await fetch('/api/vehicles?category=Cars&limit=10');
const { data, meta } = await response.json();

// Create vehicle
const newVehicle = await fetch('/api/vehicles', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    category: 'Cars',
    brand: 'Toyota',
    model: 'Camry',
    year: 2023,
    plate: 'ABC-1234',
    market_price: 35000
  })
});

// Update vehicle
const updated = await fetch('/api/vehicles/123', {
  method: 'PUT',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ color: 'Red' })
});

// Delete vehicle
const deleted = await fetch('/api/vehicles?id=123', {
  method: 'DELETE'
});
```

### cURL

```bash
# List vehicles
curl -X GET "https://api.example.com/api/vehicles?limit=10" \
  -H "Cookie: ec-vms-session=<session>"

# Create vehicle
curl -X POST "https://api.example.com/api/vehicles" \
  -H "Content-Type: application/json" \
  -H "Cookie: ec-vms-session=<session>" \
  -d '{
    "category": "Cars",
    "brand": "Toyota",
    "model": "Camry",
    "year": 2023,
    "plate": "ABC-1234",
    "market_price": 35000
  }'

# Update vehicle
curl -X PUT "https://api.example.com/api/vehicles/123" \
  -H "Content-Type: application/json" \
  -H "Cookie: ec-vms-session=<session>" \
  -d '{"color": "Blue"}'

# Delete vehicle
curl -X DELETE "https://api.example.com/api/vehicles?id=123" \
  -H "Cookie: ec-vms-session=<session>"
```

---

## Changelog

### v1.0 (2024-01-28)
- Initial API release
- Consolidated PUT endpoint to `/api/vehicles/[id]`
- Standardized error handling with `withErrorHandling` wrapper
- Added comprehensive documentation

---

**Support**: For issues or questions, contact the development team.
