# Data Hub API Documentation

## Overview

The Data Hub endpoint provides paginated access to the consolidated PPI monitoring dataset. It supports searching, filtering, and returns standardized, well-formatted fields similar to the PPI API documentation.

## Base URL

```
GET /api/data-hub
POST /api/data-hub
```

## Authentication

Required: Bearer token in Authorization header. The endpoint is protected and uses membership checks (e.g., enterprise-membership).

```
Authorization: Bearer <your-access-token>
```

## Query / Request Body Parameters

- `filters` (optional): JSON array of filter objects. Each filter should follow the format documented below.
- `search` (optional): string. Full-text search applied to suburb_name, lga, and state.
- `page` (optional): number. Defaults to 1.
- `pageSize` (optional): number. Defaults to 10.

Filter object schema (TypeScript):

```ts
interface Filter {
	id: number;
	column: string;
	operator: string;
	value: string;
	valueTo?: string;
	multiValues?: string[];
}
```

You can send filters either via query param (URL-encoded JSON) for GET requests or in the POST JSON body.

## Request Examples

GET with query params:

```bash
curl -G "https://api.example.com/api/data-hub" \
	-H "Authorization: Bearer <token>" \
	--data-urlencode "search=melbourne" \
	--data-urlencode "filters=[{\"id\":1,\"column\":\"state\",\"operator\":\"equals\",\"value\":\"VIC\"}]" \
	--data-urlencode "page=1" \
	--data-urlencode "pageSize=20"
```

POST examples:

Basic pagination request:
```bash
curl -X POST https://api.example.com/api/data-hub \
	-H "Authorization: Bearer <token>" \
	-H "Content-Type: application/json" \
	-d '{
		"search": "",
		"page": 2,
		"pageSize": 10
	}'
```

Request with multiple filters (high yield properties in populated areas):
```bash
curl -X POST https://api.example.com/api/data-hub \
	-H "Authorization: Bearer <token>" \
	-H "Content-Type: application/json" \
	-d '{
		"filters": [
			{ "column": "sales_volume_12mths", "operator": ">=", "value": "50" },
			{ "column": "rental_yield_avm", "operator": ">=", "value": "6" },
			{ "column": "rental_vacancy_1mth_dw", "operator": "<=", "value": "2" },
			{ "column": "LGA_population", "operator": ">=", "value": "15000" }
		],
		"search": "",
		"page": 2,
		"pageSize": 10
	}'
```


## Response Format

### Success Response (200)

```json
{
	"success": true,
	"data": [
		{
			"suburbName": "Sydney",
			"suburbCode": "12345",
			"postcode": "2000",
			"lga": "City of Sydney",
			"metroRegional": "Metro",
			"state": "NSW",
			"propertyType": "house",
			
			"salesVolume12mths": 245,
			"salesPriceMedian3mths": "$1,200,000.00",
			"salesPriceMedian12mths": "$1,250,000.00",
			"salesPriceMedian12mthsPctChange3mths": "4.17%",
			"salesPriceMedian12mthsPctChange12mths": "8.50%",
			"salesPriceMedian12mthsAvgpctChange5yrs": "6.25%",
			
			"askingRent12mths": "$650.00",
			"rentalYieldAvm": "2.70%",
			"rentalVacancy1mthDw": "1.50%",
			
			"lgaPopulation": 350000,
			"postcodePopulation": 25000,
			
			"daysOnMarket12mth": 28,
			"saleDemand1mth": 89,
			"rentalDemand1mth": 156,
			
			"gccsaName": "Greater Sydney",
			"salCode": "12345",
			"salName": "Sydney - Inner",
			"upliftRatio": "1.25",
			"postUplift20241231": "1,400,000.00",
			"postUpliftValue": "1,400,000.00",
			"ppi": "A+",
			"dates": {
				"March 2021": 1150000,
				"June 2021": 1180000,
				"Sept 2021": 1200000,
				"Dec 2021": 1220000,
				"March 2022": 1240000,
				"June 2022": 1260000,
				"Sept 2022": 1280000,
				"Dec 2022": 1300000,
				"March 2023": 1320000,
				"June 2023": 1340000,
				"Sept 2023": 1360000,
				"Dec 2023": 1380000,
				"March 2024": 1400000,
				"June 2024": 1420000,
				"Sept 2024": 1440000,
				"Dec 2024": 1460000,
				"March 2025": 1480000
			}
		}
	],
	"pagination": {
		"page": 1,
		"pageSize": 10,
		"total": 1234,
		"totalPages": 124
	}
}
```

### Error Responses

#### 400 - Bad Request
```json
{
	"error": "Invalid filter format",
	"message": "Filter values must match the expected type"
}
```

#### 401 - Unauthorized
```json
{
	"error": "Unauthorized",
	"message": "Invalid or expired token"
}
```

#### 403 - Forbidden
```json
{
	"error": "Forbidden",
	"message": "Enterprise membership required"
}
```

#### 500 - Internal Server Error
```json
{
	"error": "Server error",
	"message": "Error details if available"
}
```

## Data Formatting Standards

All data follows consistent formatting rules:

- **Currency values**: Formatted with dollar sign and thousands separators
  - Example: `"$1,250,000.00"`
- **Percentages**: Include the % symbol with 2 decimal places
  - Example: `"4.17%"`
- **Population figures**: Plain integers without formatting
  - Example: `350000`
- **Dates**: Historical and forecast quarterly values use consistent naming
  - Format: `"March/June/Sept/Dec YYYY"`
- **Missing data**: Returns `"N/A"` for missing strings, `0` for missing numbers
- **Identifiers**: Always returned as strings
  - Example: `"suburbCode": "12345"`
