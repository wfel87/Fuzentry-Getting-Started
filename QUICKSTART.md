# Fuzentry Quick Start Guide

## 1. Subscribe on AWS Marketplace (2 minutes)

1. Go to the Fuzentry listing and click **Continue to Subscribe**
2. Accept terms and subscribe
3. AWS will send a confirmation email with API credentials

## 2. Get API Credentials

- API Endpoint: `https://api.tailoredtechworks.net/messages`
- Product Code: `7hg9wmc6aq5kh133mhntl8dz2`

## 3. Authenticate and Test

1. Use the Postman collection at `postman/fuzentry-marketplace-collection.json`
2. Call `GET {{baseUrl}}/health` to verify connectivity
3. Call `POST {{baseUrl}}/messages` with `Authorization: Bearer {{jwtToken}}` and `x-api-key: {{apiKey}}`

## 4. Verify Usage

Run:

```bash
curl -X GET https://api.tailoredtechworks.net/usage/current \
  -H "Authorization: Bearer {YOUR_JWT_TOKEN}" \
  -H "x-api-key: {YOUR_API_KEY}"
```

## Support

support@tailoredtechworks.net
