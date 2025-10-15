# Market Sale API Documentation

## Overview

The Market Sale API provides comprehensive access to property sales market data for Australian suburbs, offering detailed insights into median sale prices, transaction volumes, and market dynamics across different property types and bedroom configurations.

## Base URL

```
GET /api/member/proptrack/market/sale-history
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
  "https://api.example.com/api/member/proptrack/market/sale-history?suburb=Melbourne&state=VIC&postcode=3000"
```

## Response Format

### Success Response Structure

```json
{
  "success": true,
  "data": {
    "MedianSalePrice": [
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
    "MedianDaysonMarket": [
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
    "SaleTransactionVolume": [
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
    "Location": "Melbourne VIC 3000"
  }
}
```

### Response Object Schema

```typescript
interface MarketSaleResponse {
  success: boolean;
  data: {
    MedianSalePrice: SaleMetric[];
    MedianDaysonMarket: SaleMetric[];
    SaleTransactionVolume: SaleMetric[];
    Location: string;
  };
}

interface SaleMetric {
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

- **Sale Prices**: Formatted as whole numbers representing Australian dollars
- **Days on Market**: Whole number of days
- **Transaction Volumes**: Whole number of transactions

### Missing Data

- **Invalid/missing data**: Consistently returns `"N/A"`
- **Zero values**: Displayed as `0`

## Key Market Sale Insights

### Median Sale Prices

#### Houses

- Combined median sale prices range from $1,271,000 (2021-2022) to $530,000 (2024-2025)
- Significant price fluctuations observed over the years

#### Units

- Combined median sale prices range from $505,000 (2020) to $440,000 (2022-2024)
- More stable pricing compared to houses

### Days on Market

#### Houses

- Median days on market varies between 62-334 days
- 3-bedroom properties typically have shorter market times

#### Units

- Consistently lower days on market, ranging from 49-85 days
- 0-3 bedroom units have similar market durations

### Transaction Volumes

#### Houses

- Low transaction volumes, typically 1-6 transactions per bedroom category
- Total combined transactions around 18 per period

#### Units

- Significantly higher transaction volumes
- 1-2 bedroom units dominate with 736-958 transactions
- Total combined transactions around 1,905 per period

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
  "detail": "Market sale data is not available for the specified location"
}
```

### 500 - Internal Server Error

```json
{
  "error": "Internal server error",
  "detail": "Unable to retrieve market sale data"
}
```

## Methodology

- Data sourced from Hotspotting property tracking API
- Location: Melbourne, Victoria, Postcode 3000
- Time Period: October 2019 - September 2025
- Metrics: Median Sale Price, Days on Market, Transaction Volume

**Note:** This data is for informational purposes and should not be considered financial advice.
