# Market Supply and Demand API Documentation

## Overview

The Market Supply and Demand API provides comprehensive insights into property market dynamics, offering detailed data on potential buyers, potential renters, supply, and demand across different property types and bedroom configurations.

## Base URL

```
GET /api/member/proptrack/market/supply-and-demand
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

### Get supply and demand data for Melbourne CBD

```bash
curl -H "Authorization: Bearer <token>" \
  "http://localhost:4000/api/member/proptrack/market/supply-and-demand?suburb=Melbourne&state=VIC&postcode=3000"
```

## Response Format

### Success Response Structure

```json
{
  "success": true,
  "data": {
    "PotentialBuyers": [
      {
        "propertyType": "house" | "unit",
        "dateRanges": [
          {
            "startDate": "YYYY-MM-DD",
            "endDate": "YYYY-MM-DD",
            "metricValues": [
              {
                "bedrooms": "0" | "1" | "2" | "3" | "4" | "5+" | "combined",
                "supply": number,
                "demand": number,
                "supplyChangePercentage": number | null,
                "demandChangePercentage": number | null
              }
            ]
          }
        ]
      }
    ],
    "PotentialRenters": [
      {
        "propertyType": "house" | "unit",
        "dateRanges": [
          {
            "startDate": "YYYY-MM-DD",
            "endDate": "YYYY-MM-DD",
            "metricValues": [
              {
                "bedrooms": "0" | "1" | "2" | "3" | "4" | "5+" | "combined",
                "supply": number,
                "demand": number,
                "supplyChangePercentage": number | null,
                "demandChangePercentage": number | null
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
interface MarketSupplyDemandResponse {
  success: boolean;
  data: {
    PotentialBuyers: SupplyDemandMetric[];
    PotentialRenters: SupplyDemandMetric[];
    Location: string;
  };
}

interface SupplyDemandMetric {
  propertyType: "house" | "unit";
  dateRanges: DateRange[];
}

interface DateRange {
  startDate: string;
  endDate: string;
  metricValues: SupplyDemandValue[];
}

interface SupplyDemandValue {
  bedrooms: "0" | "1" | "2" | "3" | "4" | "5+" | "combined";
  supply: number;
  demand: number;
  supplyChangePercentage: number | null;
  demandChangePercentage: number | null;
}
```

## Data Formatting Standards

### Numerical Values

- **Supply and Demand**: Whole number of properties
- **Change Percentages**: Decimal percentages representing market fluctuations
- **Null Values**: Indicates insufficient data or no change

### Interpretation Guidelines

- **Supply**: Number of available properties
- **Demand**: Number of potential buyers or renters
- **Supply Change Percentage**: Percentage change in available properties
- **Demand Change Percentage**: Percentage change in potential market interest

## Key Market Insights

### Potential Buyers

#### Houses

- Highly volatile market with significant demand fluctuations
- Combined supply ranges from 20-34 properties
- Combined demand varies dramatically, from 38 to 565 potential buyers
- Significant demand spikes in 1-3 bedroom categories

#### Units

- More stable market with consistent high demand
- Combined supply ranges from 724-1,645 properties
- Combined demand typically between 12,000-17,000 potential buyers
- 1-2 bedroom units dominate market interest

### Potential Renters

#### Houses

- Low supply, typically 20-36 properties
- Combined demand ranges from 216-637 potential renters
- Significant variations in 1-3 bedroom categories
- Highly unpredictable market dynamics

#### Units

- Large supply, ranging from 1,270-2,288 properties
- Combined demand between 12,000-20,000 potential renters
- Consistent high demand across all bedroom categories
- More stable rental market compared to house market

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
  "detail": "Supply and demand data is not available for the specified location"
}
```

### 500 - Internal Server Error

```json
{
  "error": "Internal server error",
  "detail": "Unable to retrieve supply and demand data"
}
```

## Methodology

- Data sourced from Hotspotting property tracking API
- Location: Melbourne, Victoria, Postcode 3000
- Time Period: October 2024 - September 2025
- Metrics: Property Supply, Potential Buyers, Potential Renters

**Note:** This data is for informational purposes and should not be considered financial advice.
