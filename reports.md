# Reports API Documentation

## Overview

The Reports API endpoint provides access to various reports based on user membership levels (Premium or Enterprise). The endpoint returns filtered reports with appropriate fields based on the section type and user's membership status.

## Base URL

```
GET /api/reports
```

## Authentication

**Required**: Bearer token in Authorization header. The endpoint is protected and requires either Premium or Enterprise membership.

```
Authorization: Bearer <your-access-token>
```

## Membership Levels

The API supports two main membership tiers:

### Enterprise Memberships
- `enterprise-membership`
- `property-pro-membership`

### Premium Memberships
- `premium-membership-gold`
- `premium-membership-gold-monthly`
- `premium-membership-gold-resubscription`
- `premium-membership-gold-quarterly`
- `premium-membership-new`

## Response Format

### Success Response (200)

```json
{
  "success": true,
  "reports": {
    "NationalReports": [
      {
        "title": "Report Title",
        "link": "report-link-url",
        "image": "https://membership.hotspotting.com.au/image-path"  // Only for specific sections
      }
    ],
    "StateReports": [
      {
        "title": "Report Title",
        "link": "report-link-url",
        "image": "https://membership.hotspotting.com.au/image-path"  // Only for specific sections
      }
    ]
    // Additional report sections...
  }
}
```

### Report Object Schema

```typescript
interface Report {
  title: string;     // The title of the report
  link: string;      // URL to access the report
  image?: string;    // Optional image URL (only for NationalReports and StateReports)
}
```

### Image-Enabled Sections

The following report sections include image URLs in their response:
- `NationalReports`
- `StateReports`

For these sections, image URLs are prefixed with: `https://membership.hotspotting.com.au`

### Error Responses

#### 401 - Unauthorized
```json
{
  "success": false,
  "error": "Unauthorized",
  "message": "Invalid or expired token"
}
```

#### 403 - Forbidden
```json
{
  "success": false,
  "error": "Forbidden",
  "message": "Required membership not found"
}
```

#### 500 - Internal Server Error
```json
{
  "success": false,
  "error": "Server error",
  "message": "Error details if available"
}
```

## Example Requests

### Request Reports with Premium Membership

```bash
curl -X GET "https://api.example.com/api/reports" \
  -H "Authorization: Bearer your_access_token"
```

### Response Example

```json
{
  "success": true,
  "reports": {
    "NationalReports": [
      {
        "title": "National Market Overview",
        "link": "https://membership.hotspotting.com.au/reports/national-overview",
        "image": "https://membership.hotspotting.com.au/images/national-overview.jpg"
      }
    ],
    "StateReports": [
      {
        "title": "NSW Property Market Report",
        "link": "https://membership.hotspotting.com.au/reports/nsw-market",
        "image": "https://membership.hotspotting.com.au/images/nsw-report.jpg"
      }
    ]
  }
}
```

## Access Control

- Enterprise membership users have access to the complete set of reports (from `enterprisePropertyProReports`)
- Premium membership users have access to a subset of reports (from `premiumReports`)
- The API automatically filters and returns the appropriate report set based on the user's membership level

## Notes

1. Report fields are filtered based on the section type
2. All sections include basic report information (title and link)
3. Image URLs are only included for specified sections (NationalReports and StateReports)
4. The API handles membership validation through the `withAuth` middleware
5. Report data is sourced from JSON files based on membership level