# Market Get All API Documentation

## Overview

The Market Get All API provides a comprehensive snapshot of property market data, consolidating multiple market metrics for a specific location. This endpoint offers a holistic view of rental prices, market trends, and property characteristics across different property types and bedroom configurations.

## Base URL

```
GET /api/member/proptrack/market
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

### Get comprehensive market data for Melbourne CBD

```bash
curl -H "Authorization: Bearer <token>" \
  "http://localhost:4000/api/member/proptrack/market?suburb=Melbourne&state=VIC&postcode=3000"
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
interface MarketGetAllResponse {
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

- **Rental Prices**: Whole numbers representing Australian dollars
- **Days on Market**: Whole number of days
- **Transaction Volumes**: Whole number of transactions
- **Rental Yields**: Decimal percentages (e.g., 0.0327 = 3.27%)

### Missing Data

- **Invalid/missing data**: Consistently returns `"N/A"`
- **Zero values**: Displayed as `0`

## Key Market Insights

### Rental Prices

#### Houses

- Combined median rental prices range from $380 to $650
- 3-bedroom properties consistently have the highest rental rates
- Significant price variations across different bedroom categories

#### Units

- Combined median rental prices range from $375 to $650
- 3-bedroom units show the most substantial price increases
- Consistent upward trend in rental prices

### Days on Market

#### Houses

- Median days on market varies between 15-69 days
- 2-bedroom properties typically have shorter market times
- Significant variations across bedroom categories

#### Units

- Consistently lower days on market, ranging from 19-32 days
- Minimal variation across different bedroom configurations

### Transaction Volumes

#### Houses

- Low transaction volumes, typically 2-32 transactions per category
- Total combined transactions around 98 per period

#### Units

- Significantly higher transaction volumes
- 1-2 bedroom units dominate with 3,463-4,326 transactions
- Total combined transactions around 8,581 per period

### Rental Yields

#### Houses

- Relatively low rental yields, ranging from 2.45% to 3.43%
- Smaller properties (0-1 bedrooms) show slightly higher yields

#### Units

- Substantially higher rental yields, ranging from 4.88% to 9.31%
- 0-bedroom units demonstrate the highest yield at 9.31%

## Location Details

- **Suburb**: Melbourne
- **State**: Victoria (VIC)
- **Postcode**: 3000

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
  "detail": "Market data is not available for the specified location"
}
```

### 500 - Internal Server Error

```json
{
  "error": "Internal server error",
  "detail": "Unable to retrieve market data"
}
```

## Methodology

- Data sourced from Hotspotting property tracking API
- Location: Melbourne, Victoria, Postcode 3000
- Time Period: October 2019 - September 2025
- Metrics: Median Rental Price, Days on Market, Transaction Volume, Rental Yield

**Note:** This data is for informational purposes and should not be considered financial advice.
