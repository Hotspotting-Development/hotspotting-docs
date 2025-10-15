# Market Rent API Documentation

## Overview

The Market Rent API provides comprehensive access to rental market data for Australian suburbs, offering detailed insights into rental prices, transaction volumes, and market dynamics across different property types and bedroom configurations.

## Base URL

```
GET /api/member/proptrack/market/rent-history
```

## Authentication

**Required**: Bearer token in Authorization header

```
Authorization: Bearer <your-access-token>
```

## Query Parameters

### Required Parameters

| Parameter  | Type   | Description                    | Example     |
| ---------- | ------ | ------------------------------ | ----------- |
| `suburb`   | string | Suburb name (case-insensitive) | `Melbourne` |
| `state`    | string | State abbreviation             | `VIC`       |
| `postcode` | string | Exact postcode match           | `3000`      |

## Request Examples

### Get rental data for Melbourne CBD

```bash
curl -H "Authorization: Bearer <token>" \
  "https://api.example.com/api/member/proptrack/market/rent-history?suburb=Melbourne&state=VIC&postcode=3000"
```

## Response Format

### Success Response Structure

```json
{
  "success": true,
  "data": {
    "MedianRentalPrice": [
      {
        "propertyType": "house" | "unit",
        "dateRanges": [
          {
            "startDate": "YYYY-MM-DD",
            "endDate": "YYYY-MM-DD",
            "metricValues": [
              {
                "bedrooms": "0" | "1" | "2" | "3" | "4+" | "combined",
                "value": number
              }
            ]
          }
        ]
      }
    ],
    "MedianDaysonMarketRent": [
      {
        "propertyType": "house" | "unit",
        "dateRanges": [
          {
            "startDate": "YYYY-MM-DD",
            "endDate": "YYYY-MM-DD",
            "metricValues": [
              {
                "bedrooms": "0" | "1" | "2" | "3" | "4+" | "5+" | "combined",
                "value": number
              }
            ]
          }
        ]
      }
    ],
    "RentalTransactionVolume": [
      {
        "propertyType": "house" | "unit",
        "dateRanges": [
          {
            "startDate": "YYYY-MM-DD",
            "endDate": "YYYY-MM-DD",
            "metricValues": [
              {
                "bedrooms": "0" | "1" | "2" | "3" | "4+" | "5+" | "combined",
                "value": number
              }
            ]
          }
        ]
      }
    ],
    "MedianRentalYield": [
      {
        "propertyType": "house" | "unit",
        "dateRanges": [
          {
            "startDate": "YYYY-MM-DD",
            "endDate": "YYYY-MM-DD",
            "metricValues": [
              {
                "bedrooms": "0" | "1" | "2" | "3" | "4+" | "5+" | "combined",
                "value": number
              }
            ]
          }
        ]
      }
    ],
    "MedianRentalPriceRequestYearly": [
      {
        "propertyType": "house" | "unit",
        "dateRanges": [
          {
            "startDate": "YYYY-MM-DD",
            "endDate": "YYYY-MM-DD",
            "metricValues": [
              {
                "bedrooms": "0" | "1" | "2" | "3" | "4+" | "combined",
                "value": number
              }
            ]
          }
        ]
      }
    ],
    "Location": "Melbourne VIC 3000"
  }
}
```

### Response Object Schema

```typescript
interface RentalMarketResponse {
  success: boolean;
  data: {
    MedianRentalPrice: RentalMetric[];
    MedianDaysonMarketRent: RentalMetric[];
    RentalTransactionVolume: RentalMetric[];
    MedianRentalYield: RentalMetric[];
    MedianRentalPriceRequestYearly: RentalMetric[];
    Location: string;
  };
}

interface RentalMetric {
  propertyType: "house" | "unit";
  dateRanges: DateRange[];
}

interface DateRange {
  startDate: string;
  endDate: string;
  metricValues: MetricValue[];
}

interface MetricValue {
  bedrooms: "0" | "1" | "2" | "3" | "4+" | "5+" | "combined";
  value: number;
}
```

## Data Formatting Standards

### Numerical Values

- **Rental Prices**: Formatted as whole numbers representing Australian dollars
- **Rental Yields**: Formatted as decimal percentages (e.g., 0.0327 = 3.27%)
- **Days on Market**: Whole number of days
- **Transaction Volumes**: Whole number of transactions

### Missing Data

- **Invalid/missing data**: Consistently returns `"N/A"`
- **Zero values**: Displayed as `0`

## Key Rental Market Insights for Melbourne (VIC 3000)

### Median Rental Prices

#### Houses

| Bedrooms | 2020-2021 | 2021-2022 | 2022-2023 | 2023-2024 | 2024-2025 |
| -------- | --------- | --------- | --------- | --------- | --------- |
| 0 BR     | $400      | $410      | N/A       | N/A       | $445      |
| 1 BR     | $350      | $365      | $525      | $520      | $525      |
| 2 BR     | $386      | $370      | $630      | $650      | $650      |
| 3 BR     | $535      | $750      | $850      | $900      | $800      |
| Combined | $390      | $400      | $630      | $650      | $638      |

#### Units

| Bedrooms | 2020-2021 | 2021-2022 | 2022-2023 | 2023-2024 | 2024-2025 |
| -------- | --------- | --------- | --------- | --------- | --------- |
| 0 BR     | $260      | $280      | $370      | $390      | $400      |
| 1 BR     | $320      | $380      | $500      | $525      | $540      |
| 2 BR     | $400      | $500      | $650      | $700      | $715      |
| 3 BR     | $600      | $700      | $1000     | $1050     | $1100     |
| 4+ BR    | $725      | $725      | $1000     | $1000     | $1050     |
| Combined | $375      | $450      | $600      | $630      | $650      |

### Rental Market Dynamics

#### Median Days on Market (Oct 2024 - Sep 2025)

##### Houses

| Bedrooms | Median Days |
| -------- | ----------- |
| 0 BR     | 45 days     |
| 1 BR     | 57 days     |
| 2 BR     | 29 days     |
| 3 BR     | 69 days     |
| 4+ BR    | 15-23 days  |
| Combined | 42 days     |

##### Units

| Bedrooms | Median Days |
| -------- | ----------- |
| 0 BR     | 19 days     |
| 1 BR     | 22 days     |
| 2 BR     | 29 days     |
| 3 BR     | 32 days     |
| 4+ BR    | 27 days     |
| Combined | 26 days     |

### Rental Transaction Volume (Oct 2024 - Sep 2025)

#### Houses

| Bedrooms | Transactions |
| -------- | ------------ |
| 0 BR     | 15           |
| 1 BR     | 32           |
| 2 BR     | 27           |
| 3 BR     | 19           |
| 4+ BR    | 2-3          |
| Combined | 98           |

#### Units

| Bedrooms | Transactions |
| -------- | ------------ |
| 0 BR     | 236          |
| 1 BR     | 3,463        |
| 2 BR     | 4,326        |
| 3 BR     | 535          |
| 4+ BR    | 19           |
| Combined | 8,581        |

### Rental Yield Analysis (Sep 2025)

#### Houses

| Bedrooms | Rental Yield |
| -------- | ------------ |
| 0 BR     | 3.27%        |
| 1 BR     | 3.43%        |
| 2 BR     | 2.96%        |
| 3 BR     | 2.80%        |
| 4+ BR    | 2.70%        |
| Combined | 2.45%        |

#### Units

| Bedrooms | Rental Yield |
| -------- | ------------ |
| 0 BR     | 9.31%        |
| 1 BR     | 8.00%        |
| 2 BR     | 6.78%        |
| 3 BR     | 5.46%        |
| 4+ BR    | 4.88%        |
| Combined | 7.19%        |

## Error Responses

### 400 - Bad Request

```json
{
  "error": "Missing required parameters",
  "detail": "Suburb, state, and postcode must be provided"
}
```

### 401 - Unauthorized

```json
{
  "error": "Unauthorized",
  "detail": "Invalid or missing authentication token"
}
```

### 404 - Not Found

```json
{
  "error": "Data source not found",
  "detail": "Rental market data is not available for the specified location"
}
```

### 500 - Internal Server Error

```json
{
  "error": "Internal server error",
  "detail": "Unable to retrieve rental market data"
}
```

## Methodology

- Data sourced from Hotspotting property tracking API
- Location: Melbourne, Victoria, Postcode 3000
- Time Period: October 2019 - September 2025
- Metrics: Median Rental Price, Days on Market, Transaction Volume, Rental Yield

**Note:** This data is for informational purposes and should not be considered financial advice.
