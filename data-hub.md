# Data Hub API Documentation

## Overview

The Data Hub endpoint provides paginated access to the consolidated PPI monitoring dataset. It supports searching, filtering, and returns standardized, well-formatted fields similar to the PPI API documentation.

## Base URL

```
GET /api/member/data-hub
POST /api/member/data-hub
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
curl -G "https://api.example.com/api/member/data-hub" \
	-H "Authorization: Bearer <token>" \
	--data-urlencode "search=melbourne" \
	--data-urlencode "filters=[{\"id\":1,\"column\":\"state\",\"operator\":\"equals\",\"value\":\"VIC\"}]" \
	--data-urlencode "page=1" \
	--data-urlencode "pageSize=20"
```

POST examples:

Basic pagination request:

```bash
curl -X POST https://api.example.com/api/member/data-hub \
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
curl -X POST https://api.example.com/api/member/data-hub \
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
      "suburbName": "Alexandria",
      "suburbCode": "10030",
      "postcode": "2015",
      "lga": "Sydney",
      "metroRegional": "Metropolitan",
      "state": "New South Wales",
      "endDate": {
        "value": "2025-08-31"
      },
      "propertyType": "house",
      "salesVolume12mths": 83,
      "salesPriceMedian3mths": "$1,870,000.00",
      "salesPriceMedian12mths": "$2,032,500.00",
      "salesPriceMedian12mthsPctChange3mths": "0.37%",
      "salesPriceMedian12mthsPctChange12mths": "-0.37%",
      "salesPriceMedian12mthsAvgpctChange5yrs": "5.10%",
      "askingRent12mths": "$1,100.00",
      "askingRent12mthsPctChange3mths": "0.00%",
      "askingRent12mthsPctChange12mths": "4.76%",
      "rentalYieldAvm": "2.60%",
      "rentalVacancy1mth": "1.75%",
      "rentalVacancy1mthDw": "1.53%",
      "daysOnMarket12mth": 25,
      "saleDemand1mth": 1442,
      "rentalDemand1mth": 540,
      "gccsaName": "Greater Sydney",
      "salCode": 10030,
      "salName": "Alexandria",
      "upliftRatio": "1.24",
      "postUplift20241231": "N/A",
      "postUpliftValue": "14.00",
      "variable": "",
      "ppi": "Plateau",
      "lgaPopulation": "211,632",
      "postcodePopulation": "11,422",
      "lgaSeifaIrsadDecile": 10,
      "familyIncomeUnder1000": 4638,
      "familyIncomeUnder1000Percent": "0.10%",
      "familyIncome1000To3000": 17598,
      "familyIncome1000To3000Percent": "0.38%",
      "familyIncome3000To8000": 21419,
      "familyIncome3000To8000Percent": "0.46%",
      "familyIncomeNilIncome": 886,
      "familyIncomeNilIncomePercent": "0.02%",
      "familyIncome8000OrMore": 1714,
      "familyIncome8000OrMorePercent": "0.04%",
      "familyIncomeNegative": 76,
      "familyIncomeNegativePercent": "0.00%",
      "totalFamilyIncome": 46331,
      "homeowner": 32959,
      "homeownerPercent": "0.34%",
      "rented": 63620,
      "rentedPercent": "0.65%",
      "other": 1185,
      "otherPercent": "0.01%",
      "totalTenure": 97764,
      "industryEmploymentAgricultureForestryAndFishing": 208,
      "industryEmploymentAgricultureForestryAndFishingPercent": "0.00%",
      "industryEmploymentMining": 191,
      "industryEmploymentMiningPercent": "0.00%",
      "industryEmploymentManufacturing": 3159,
      "industryEmploymentManufacturingPercent": "0.03%",
      "industryEmploymentElectricityGasWaterAndWasteServices": 640,
      "industryEmploymentElectricityGasWaterAndWasteServicesPercent": "0.01%",
      "industryEmploymentConstruction": 5444,
      "industryEmploymentConstructionPercent": "0.04%",
      "industryEmploymentWholesaleTrade": 2841,
      "industryEmploymentWholesaleTradePercent": "0.02%",
      "industryEmploymentRetailTrade": 8927,
      "industryEmploymentRetailTradePercent": "0.07%",
      "industryEmploymentAccommodationAndFoodServices": 10438,
      "industryEmploymentAccommodationAndFoodServicesPercent": "0.08%",
      "industryEmploymentTransportPostalAndWarehousing": 4095,
      "industryEmploymentTransportPostalAndWarehousingPercent": "0.03%",
      "industryEmploymentInformationMediaAndTelecommunications": 5294,
      "industryEmploymentInformationMediaAndTelecommunicationsPercent": "0.04%",
      "industryEmploymentFinancialAndInsuranceServices": 13026,
      "industryEmploymentFinancialAndInsuranceServicesPercent": "0.10%",
      "industryEmploymentRentalHiringAndRealEstateServices": 2751,
      "industryEmploymentRentalHiringAndRealEstateServicesPercent": "0.02%",
      "industryEmploymentProfessionalScientificAndTechnicalServices": 23981,
      "industryEmploymentProfessionalScientificAndTechnicalServicesPercent": "0.19%",
      "industryEmploymentAdministrativeAndSupportServices": 4571,
      "industryEmploymentAdministrativeAndSupportServicesPercent": "0.04%",
      "industryEmploymentPublicAdministrationAndSafety": 7049,
      "industryEmploymentPublicAdministrationAndSafetyPercent": "0.06%",
      "industryEmploymentEducationAndTraining": 9602,
      "industryEmploymentEducationAndTrainingPercent": "0.08%",
      "industryEmploymentHealthCareAndSocialAssistance": 11906,
      "industryEmploymentHealthCareAndSocialAssistancePercent": "0.09%",
      "industryEmploymentArtsAndRecreationServices": 3580,
      "industryEmploymentArtsAndRecreationServicesPercent": "0.03%",
      "industryEmploymentOther": 7839,
      "industryEmploymentOtherPercent": "0.06%",
      "totalIndustryEmployment": 125542,
      "labourForceEmployedWorkedFullTime": 82867,
      "labourForceEmployedWorkedFullTimePercent": "0.62%",
      "labourForceEmployedWorkedPartTime": 31499,
      "labourForceEmployedWorkedPartTimePercent": "0.23%",
      "labourForceEmployedAwayFromWork": 12521,
      "labourForceEmployedAwayFromWorkPercent": "0.09%",
      "labourForceUnemployedLookingForFullTimeWork": 4225,
      "labourForceUnemployedLookingForFullTimeWorkPercent": "0.03%",
      "labourForceUnemployedLookingForPartTimeWork": 3248,
      "labourForceUnemployedLookingForPartTimeWorkPercent": "0.02%",
      "totalLabourForce": 134360,
      "mortgageMonthlyUnder1000": 1166,
      "mortgageMonthlyUnder1000Percent": "0.07%",
      "mortgageMonthly1000To1199": 532,
      "mortgageMonthly1000To1199Percent": "0.03%",
      "mortgageMonthly1200To1399": 619,
      "mortgageMonthly1200To1399Percent": "0.04%",
      "mortgageMonthly1400To1599": 626,
      "mortgageMonthly1400To1599Percent": "0.04%",
      "mortgageMonthly1600To1799": 686,
      "mortgageMonthly1600To1799Percent": "0.04%",
      "mortgageMonthly1800To1999": 623,
      "mortgageMonthly1800To1999Percent": "0.04%",
      "mortgageMonthly2000To2199": 1544,
      "mortgageMonthly2000To2199Percent": "0.09%",
      "mortgageMonthly2200To2399": 740,
      "mortgageMonthly2200To2399Percent": "0.04%",
      "mortgageMonthly2400To2599": 938,
      "mortgageMonthly2400To2599Percent": "0.06%",
      "mortgageMonthly2600To2799": 953,
      "mortgageMonthly2600To2799Percent": "0.06%",
      "mortgageMonthly2800To2999": 683,
      "mortgageMonthly2800To2999Percent": "0.04%",
      "mortgageMonthly3000To3499": 2363,
      "mortgageMonthly3000To3499Percent": "0.14%",
      "mortgageMonthly3500To3999": 1125,
      "mortgageMonthly3500To3999Percent": "0.07%",
      "mortgageMonthly4000To4999": 1909,
      "mortgageMonthly4000To4999Percent": "0.11%",
      "mortgageMonthly5000AndOver": 2521,
      "mortgageMonthly5000AndOverPercent": "0.15%",
      "totalMortgage": 17028,
      "occupationManagers": 23719,
      "occupationManagersPercent": "0.19%",
      "occupationProfessionals": 52808,
      "occupationProfessionalsPercent": "0.42%",
      "occupationTechniciansAndTradesWorkers": 8953,
      "occupationTechniciansAndTradesWorkersPercent": "0.07%",
      "occupationCommunityAndPersonalServiceWorkers": 9739,
      "occupationCommunityAndPersonalServiceWorkersPercent": "0.08%",
      "occupationClericalAndAdministrativeWorkers": 13496,
      "occupationClericalAndAdministrativeWorkersPercent": "0.11%",
      "occupationSalesWorkers": 7916,
      "occupationSalesWorkersPercent": "0.06%",
      "occupationMachineryOperatorsAndDrivers": 2505,
      "occupationMachineryOperatorsAndDriversPercent": "0.02%",
      "occupationLabourers": 5464,
      "occupationLabourersPercent": "0.04%",
      "occupationOther": 1393,
      "occupationOtherPercent": "0.01%",
      "totalOccupation": 125993,
      "dates": {
        "March 2021": 25,
        "June 2021": 26,
        "Sept 2021": 29,
        "Dec 2021": 24,
        "March 2022": 23,
        "June 2022": 22,
        "Sept 2022": 15,
        "Dec 2022": 20,
        "March 2023": 16,
        "June 2023": 16,
        "Sept 2023": 18,
        "Dec 2023": 24,
        "March 2024": 18,
        "June 2024": 20,
        "Sept 2024": 25,
        "Dec 2024": 28,
        "March 2025": 19
      }
    },
    {
      "suburbName": "Alexandria",
      "suburbCode": "10030",
      "postcode": "2015",
      "lga": "Sydney",
      "metroRegional": "Metropolitan",
      "state": "New South Wales",
      "endDate": {
        "value": "2025-08-31"
      },
      "propertyType": "unit",
      "salesVolume12mths": 208,
      "salesPriceMedian3mths": "$935,000.00",
      "salesPriceMedian12mths": "$910,000.00",
      "salesPriceMedian12mthsPctChange3mths": "0.78%",
      "salesPriceMedian12mthsPctChange12mths": "7.06%",
      "salesPriceMedian12mthsAvgpctChange5yrs": "4.72%",
      "askingRent12mths": "$800.00",
      "askingRent12mthsPctChange3mths": "0.63%",
      "askingRent12mthsPctChange12mths": "6.67%",
      "rentalYieldAvm": "4.60%",
      "rentalVacancy1mth": "1.46%",
      "rentalVacancy1mthDw": "1.53%",
      "daysOnMarket12mth": 26,
      "saleDemand1mth": 2548,
      "rentalDemand1mth": 2596,
      "gccsaName": "Greater Sydney",
      "salCode": 10030,
      "salName": "Alexandria",
      "upliftRatio": "1.41",
      "postUplift20241231": "N/A",
      "postUpliftValue": "71.00",
      "variable": "",
      "ppi": "Rising",
      "lgaPopulation": "211,632",
      "postcodePopulation": "11,422",
      "lgaSeifaIrsadDecile": 10,
      "familyIncomeUnder1000": 4638,
      "familyIncomeUnder1000Percent": "0.10%",
      "familyIncome1000To3000": 17598,
      "familyIncome1000To3000Percent": "0.38%",
      "familyIncome3000To8000": 21419,
      "familyIncome3000To8000Percent": "0.46%",
      "familyIncomeNilIncome": 886,
      "familyIncomeNilIncomePercent": "0.02%",
      "familyIncome8000OrMore": 1714,
      "familyIncome8000OrMorePercent": "0.04%",
      "familyIncomeNegative": 76,
      "familyIncomeNegativePercent": "0.00%",
      "totalFamilyIncome": 46331,
      "homeowner": 32959,
      "homeownerPercent": "0.34%",
      "rented": 63620,
      "rentedPercent": "0.65%",
      "other": 1185,
      "otherPercent": "0.01%",
      "totalTenure": 97764,
      "industryEmploymentAgricultureForestryAndFishing": 208,
      "industryEmploymentAgricultureForestryAndFishingPercent": "0.00%",
      "industryEmploymentMining": 191,
      "industryEmploymentMiningPercent": "0.00%",
      "industryEmploymentManufacturing": 3159,
      "industryEmploymentManufacturingPercent": "0.03%",
      "industryEmploymentElectricityGasWaterAndWasteServices": 640,
      "industryEmploymentElectricityGasWaterAndWasteServicesPercent": "0.01%",
      "industryEmploymentConstruction": 5444,
      "industryEmploymentConstructionPercent": "0.04%",
      "industryEmploymentWholesaleTrade": 2841,
      "industryEmploymentWholesaleTradePercent": "0.02%",
      "industryEmploymentRetailTrade": 8927,
      "industryEmploymentRetailTradePercent": "0.07%",
      "industryEmploymentAccommodationAndFoodServices": 10438,
      "industryEmploymentAccommodationAndFoodServicesPercent": "0.08%",
      "industryEmploymentTransportPostalAndWarehousing": 4095,
      "industryEmploymentTransportPostalAndWarehousingPercent": "0.03%",
      "industryEmploymentInformationMediaAndTelecommunications": 5294,
      "industryEmploymentInformationMediaAndTelecommunicationsPercent": "0.04%",
      "industryEmploymentFinancialAndInsuranceServices": 13026,
      "industryEmploymentFinancialAndInsuranceServicesPercent": "0.10%",
      "industryEmploymentRentalHiringAndRealEstateServices": 2751,
      "industryEmploymentRentalHiringAndRealEstateServicesPercent": "0.02%",
      "industryEmploymentProfessionalScientificAndTechnicalServices": 23981,
      "industryEmploymentProfessionalScientificAndTechnicalServicesPercent": "0.19%",
      "industryEmploymentAdministrativeAndSupportServices": 4571,
      "industryEmploymentAdministrativeAndSupportServicesPercent": "0.04%",
      "industryEmploymentPublicAdministrationAndSafety": 7049,
      "industryEmploymentPublicAdministrationAndSafetyPercent": "0.06%",
      "industryEmploymentEducationAndTraining": 9602,
      "industryEmploymentEducationAndTrainingPercent": "0.08%",
      "industryEmploymentHealthCareAndSocialAssistance": 11906,
      "industryEmploymentHealthCareAndSocialAssistancePercent": "0.09%",
      "industryEmploymentArtsAndRecreationServices": 3580,
      "industryEmploymentArtsAndRecreationServicesPercent": "0.03%",
      "industryEmploymentOther": 7839,
      "industryEmploymentOtherPercent": "0.06%",
      "totalIndustryEmployment": 125542,
      "labourForceEmployedWorkedFullTime": 82867,
      "labourForceEmployedWorkedFullTimePercent": "0.62%",
      "labourForceEmployedWorkedPartTime": 31499,
      "labourForceEmployedWorkedPartTimePercent": "0.23%",
      "labourForceEmployedAwayFromWork": 12521,
      "labourForceEmployedAwayFromWorkPercent": "0.09%",
      "labourForceUnemployedLookingForFullTimeWork": 4225,
      "labourForceUnemployedLookingForFullTimeWorkPercent": "0.03%",
      "labourForceUnemployedLookingForPartTimeWork": 3248,
      "labourForceUnemployedLookingForPartTimeWorkPercent": "0.02%",
      "totalLabourForce": 134360,
      "mortgageMonthlyUnder1000": 1166,
      "mortgageMonthlyUnder1000Percent": "0.07%",
      "mortgageMonthly1000To1199": 532,
      "mortgageMonthly1000To1199Percent": "0.03%",
      "mortgageMonthly1200To1399": 619,
      "mortgageMonthly1200To1399Percent": "0.04%",
      "mortgageMonthly1400To1599": 626,
      "mortgageMonthly1400To1599Percent": "0.04%",
      "mortgageMonthly1600To1799": 686,
      "mortgageMonthly1600To1799Percent": "0.04%",
      "mortgageMonthly1800To1999": 623,
      "mortgageMonthly1800To1999Percent": "0.04%",
      "mortgageMonthly2000To2199": 1544,
      "mortgageMonthly2000To2199Percent": "0.09%",
      "mortgageMonthly2200To2399": 740,
      "mortgageMonthly2200To2399Percent": "0.04%",
      "mortgageMonthly2400To2599": 938,
      "mortgageMonthly2400To2599Percent": "0.06%",
      "mortgageMonthly2600To2799": 953,
      "mortgageMonthly2600To2799Percent": "0.06%",
      "mortgageMonthly2800To2999": 683,
      "mortgageMonthly2800To2999Percent": "0.04%",
      "mortgageMonthly3000To3499": 2363,
      "mortgageMonthly3000To3499Percent": "0.14%",
      "mortgageMonthly3500To3999": 1125,
      "mortgageMonthly3500To3999Percent": "0.07%",
      "mortgageMonthly4000To4999": 1909,
      "mortgageMonthly4000To4999Percent": "0.11%",
      "mortgageMonthly5000AndOver": 2521,
      "mortgageMonthly5000AndOverPercent": "0.15%",
      "totalMortgage": 17028,
      "occupationManagers": 23719,
      "occupationManagersPercent": "0.19%",
      "occupationProfessionals": 52808,
      "occupationProfessionalsPercent": "0.42%",
      "occupationTechniciansAndTradesWorkers": 8953,
      "occupationTechniciansAndTradesWorkersPercent": "0.07%",
      "occupationCommunityAndPersonalServiceWorkers": 9739,
      "occupationCommunityAndPersonalServiceWorkersPercent": "0.08%",
      "occupationClericalAndAdministrativeWorkers": 13496,
      "occupationClericalAndAdministrativeWorkersPercent": "0.11%",
      "occupationSalesWorkers": 7916,
      "occupationSalesWorkersPercent": "0.06%",
      "occupationMachineryOperatorsAndDrivers": 2505,
      "occupationMachineryOperatorsAndDriversPercent": "0.02%",
      "occupationLabourers": 5464,
      "occupationLabourersPercent": "0.04%",
      "occupationOther": 1393,
      "occupationOtherPercent": "0.01%",
      "totalOccupation": 125993,
      "dates": {
        "March 2021": 73,
        "June 2021": 57,
        "Sept 2021": 56,
        "Dec 2021": 53,
        "March 2022": 52,
        "June 2022": 31,
        "Sept 2022": 43,
        "Dec 2022": 41,
        "March 2023": 41,
        "June 2023": 48,
        "Sept 2023": 55,
        "Dec 2023": 40,
        "March 2024": 51,
        "June 2024": 50,
        "Sept 2024": 56,
        "Dec 2024": 39,
        "March 2025": 64
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
