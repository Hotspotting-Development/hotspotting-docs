# PPI API Documentation

## Overview

The PPI (Property Price Index) API provides comprehensive access to property market data, demographics, and socioeconomic information for Australian suburbs. All data is consistently formatted with proper error handling and standardized field mappings.

## Base URL

```
GET /api/ppi
```

## Authentication

**Required**: Bearer token in Authorization header

```
Authorization: Bearer <your-access-token>
```

## Query Parameters

### Required Parameters

At least one of the following must be provided:

| Parameter     | Type   | Description                                 |
| ------------- | ------ | ------------------------------------------- |
| `suburb_code` | number | Exact suburb code match                     |
| `suburb_name` | string | Suburb name prefix match (case-insensitive) |

### Optional Parameters

| Parameter              | Type   | Description                                       | Example  |
| ---------------------- | ------ | ------------------------------------------------- | -------- |
| `postcode`             | string | Exact postcode match                              | `2000`   |
| `suburb`               | string | Partial suburb name match (case-insensitive)      | `Syd`    |
| `property_type`        | string | Property type filter: `"house"` or `"unit"`       | `house`  |
| `categorization_house` | string | House categorization (when property_type="house") | `luxury` |
| `categorization_unit`  | string | Unit categorization (when property_type="unit")   | `studio` |

## Request Examples

### Get data for a specific suburb by name

```bash
curl -H "Authorization: Bearer <token>" \
  "https://api.example.com/api/ppi?suburb_name=Sydney&property_type=house"
```

### Get data for a specific postcode

```bash
curl -H "Authorization: Bearer <token>" \
  "https://api.example.com/api/ppi?postcode=2000"
```

### Get data for a suburb code with property type

```bash
curl -H "Authorization: Bearer <token>" \
  "https://api.example.com/api/ppi?suburb_code=12345&property_type=unit"
```

### Search for suburbs containing a term

```bash
curl -H "Authorization: Bearer <token>" \
  "https://api.example.com/api/ppi?suburb=Melbourne&property_type=house"
```

## Response Format

### Success Response Structure

```json
{
  "success": true,
  "count": 1,
  "data": [
    /* Array of PPI data objects */
  ],
  "timestamp": "2024-01-15T10:30:00.000Z",
  "query_parameters": {
    "suburb_name": "Sydney",
    "property_type": "house"
  }
}
```

### PPI Data Object Schema

#### Basic Information

```typescript
{
  // Suburb identification
  suburbName: string,           // "Sydney"
  suburbCode: string,           // "12345"
  postcode: string,             // "2000"
  lga: string,                  // "City of Sydney"
  state: string,                // "NSW"
  metroRegional: string,        // "Metro"
  propertyType: string,         // "house" | "unit"
  endDate: string,              // "2024-01-01"

  // Market Stability and Growth (formatted)
  salesPriceMedian12mths: string,              // "$1,250,000.00"
  salesPriceMedian3mths: string,               // "$1,200,000.00"
  salesPriceMedian12mthsPctChange3mths: string,  // "4.17%" | "N/A"
  salesPriceMedian12mthsPctChange12mths: string, // "8.50%" | "N/A"
  salesPriceMedian12mthsAvgpctChange5yrs: string, // "6.25%" | "N/A"

  // Rental Market (formatted)
  askingRent12mths: string,                    // "$650.00" | "N/A"
  askingRent12mthsPctChange3mths: string,      // "3.20%" | "N/A"
  askingRent12mthsPctChange12mths: string,     // "7.80%" | "N/A"
  rentalYieldAvm: string,                      // "2.70%" | "N/A"
  rentalVacancy1mth: string,                   // "1.50%" | "N/A"
  rentalVacancy1mthDw: string,                 // "1.50%" | "N/A"

  // Demand Dynamics
  salesVolume12mths: number,                   // 245
  daysOnMarket12mth: number,                   // 28
  saleDemand1mth: number,                      // 89
  rentalDemand1mth: number,                    // 156

  // Demographics and Socioeconomic Factors (formatted)
  lgaPopulation: number,                       // 350000
  postcodePopulation: number,                  // 25000
  lgaSeifaIrsadDecile: string,                // "8" | "N/A"

  // Housing Tenure (formatted percentages)
  homeowner: number,                           // 15000
  homeownerPercent: string,                    // "60.25%" | "N/A"
  rented: number,                              // 8000
  rentedPercent: string,                       // "32.10%" | "N/A"
  other: number,                               // 2000
  otherPercent: string,                        // "7.65%" | "N/A"
  totalTenure: number,                         // 25000

  // Family Income Distribution (formatted)
  familyIncomeUnder1000: number,               // 1200
  familyIncomeUnder1000Percent: string,        // "4.80%" | "N/A"
  familyIncome1000To3000: number,              // 3500
  familyIncome1000To3000Percent: string,       // "14.00%" | "N/A"
  familyIncome3000To8000: number,              // 12000
  familyIncome3000To8000Percent: string,       // "48.00%" | "N/A"
  familyIncome8000OrMore: number,              // 8300
  familyIncome8000OrMorePercent: string,       // "33.20%" | "N/A"
  familyIncomeNilIncome: number,               // 0
  familyIncomeNilIncomePercent: string,        // "0.00%" | "N/A"
  familyIncomeNegative: number,                // 0
  familyIncomeNegativePercent: string,         // "0.00%" | "N/A"
  totalFamilyIncome: number,                   // 25000

  // Employment by Industry (formatted)
  industryEmploymentMining: number,                        // 250
  industryEmploymentMiningPercent: string,                 // "1.25%" | "N/A"
  industryEmploymentManufacturing: number,                 // 1800
  industryEmploymentManufacturingPercent: string,          // "9.00%" | "N/A"
  industryEmploymentConstruction: number,                  // 2200
  industryEmploymentConstructionPercent: string,           // "11.00%" | "N/A"
  industryEmploymentRetailTrade: number,                   // 1600
  industryEmploymentRetailTradePercent: string,            // "8.00%" | "N/A"
  industryEmploymentFinancialAndInsuranceServices: number, // 3400
  industryEmploymentFinancialAndInsuranceServicesPercent: string, // "17.00%" | "N/A"
  industryEmploymentEducationAndTraining: number,          // 1800
  industryEmploymentEducationAndTrainingPercent: string,   // "9.00%" | "N/A"
  industryEmploymentHealthCareAndSocialAssistance: number, // 2800
  industryEmploymentHealthCareAndSocialAssistancePercent: string, // "14.00%" | "N/A"
  // ... (additional industry fields)
  totalIndustryEmployment: number,                         // 20000

  // Labour Force (formatted)
  labourForceEmployedWorkedFullTime: number,               // 12000
  labourForceEmployedWorkedFullTimePercent: string,        // "60.00%" | "N/A"
  labourForceEmployedWorkedPartTime: number,               // 5000
  labourForceEmployedWorkedPartTimePercent: string,        // "25.00%" | "N/A"
  labourForceEmployedAwayFromWork: number,                 // 800
  labourForceEmployedAwayFromWorkPercent: string,          // "4.00%" | "N/A"
  labourForceUnemployedLookingForFullTimeWork: number,     // 1500
  labourForceUnemployedLookingForFullTimeWorkPercent: string, // "7.50%" | "N/A"
  labourForceUnemployedLookingForPartTimeWork: number,     // 700
  labourForceUnemployedLookingForPartTimeWorkPercent: string, // "3.50%" | "N/A"
  totalLabourForce: number,                                // 20000

  // Mortgage Repayment Ranges (formatted)
  mortgageMonthlyUnder1000: number,                        // 500
  mortgageMonthlyUnder1000Percent: string,                 // "5.00%" | "N/A"
  mortgageMonthly1000To1199: number,                       // 800
  mortgageMonthly1000To1199Percent: string,                // "8.00%" | "N/A"
  mortgageMonthly2000To2199: number,                       // 1200
  mortgageMonthly2000To2199Percent: string,                // "12.00%" | "N/A"
  mortgageMonthly5000AndOver: number,                      // 300
  mortgageMonthly5000AndOverPercent: string,               // "3.00%" | "N/A"
  // ... (additional mortgage ranges)
  totalMortgage: number,                                   // 10000

  // Occupation Categories (formatted)
  occupationManagers: number,                              // 2500
  occupationManagersPercent: string,                       // "12.50%" | "N/A"
  occupationProfessionals: number,                         // 5000
  occupationProfessionalsPercent: string,                  // "25.00%" | "N/A"
  occupationTechniciansAndTradesWorkers: number,           // 3000
  occupationTechniciansAndTradesWorkersPercent: string,    // "15.00%" | "N/A"
  // ... (additional occupation fields)
  totalOccupation: number,                                 // 20000

  // Geographic Classification
  gccsaName: string,                                       // "Greater Sydney"
  salCode: string,                                         // "12345"
  salName: string,                                         // "Sydney - Inner"

  // Price Index and Forecasting
  ppi: string,                                            // "A+" | "B" | "C"
  upliftRatio: string,                                    // "1.25" | "N/A"
  postUplift20241231: string,                             // "1,400,000.00" | "N/A"
  postUpliftValue: string,                                // "1,400,000.00" | "N/A"
  variable: string,                                       // "property_value"

  // Quarterly Historical Data
  dates: {
    "March 2021": number,    // 1150000
    "June 2021": number,     // 1180000
    "Sept 2021": number,     // 1200000
    "Dec 2021": number,      // 1220000
    // ... continues through quarters to March 2025
  }
}
```

## Data Formatting Standards

### Numerical Values

- **All numerical values**: Formatted to 2 decimal places using `.toFixed(2)`
- **Currency values**: Include proper locale formatting with thousand separators
  - Example: `"$1,250,000.00"`
- **Percentage values**: Formatted as `"XX.XX%"` strings
  - Example: `"4.17%"`
- **Population figures**: Locale-formatted integers without decimals
  - Example: `350000` (number) or `"350,000"` (when displayed)

### Missing Data

- **Invalid/missing data**: Consistently returns `"N/A"`
- **Zero values**: Displayed as `"0.00"` for currencies, `"0.00%"` for percentages

## Error Responses

### 400 - Bad Request

```json
{
  "error": "Missing required parameters",
  "detail": "Either suburb_code or suburb_name must be provided",
  "example": "/api/ppi?suburb_name=Sydney or /api/ppi?suburb_code=12345"
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
  "detail": "PPI data source is not accessible",
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

### 500 - Internal Server Error

```json
{
  "error": "Internal server error",
  "detail": "Specific error message",
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

Available on our API

- Total Population
- Average Age

Median Weekly income

- Personal
- Family
- Household

Tenure type

- Rented
- Owned outright
- Owned with a mortgage
- Other/not Stated

Median monthy mortgage Repayments

PPI Ranking

Market Stability Growth

- 12M Median Sales Price
- 3M Price Change
- 5yr Avg Growth

Demand Dyanmics

- 12 Sales Volume
- Days on Market

Housing Tenure

- Homeownership Rate

Affordability and Entry Point

- Median Sales Price

Strong Rental Market

- 12 Median Asking Rent
- Rental Yield
- Rental Vacancy Rate

Demographic and Socioeconomic Factors

- LGA Population
- Postcode population
- SEIFA Decile

Employment and Labour Force

- Employed Full Time
- Employed Part Time

N/A on our api yet

Property Type

- Separate house
- Semi Detached house
- Unit/Apartment

Family composition

- Couple Family withou children
- One parent family
- Couple family with Children
