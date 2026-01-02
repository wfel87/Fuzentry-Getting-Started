# FinTech KYC/AML Workflow - Fuzentry Example

## 1. Use Case
Digital bank automates KYC for new account opening including identity verification, document checks, and sanctions screening.

## 2. Scenario
Applicant submits passport, utility bill, and bank statement. The system extracts PII, cross-checks names/dates, and screens against OFAC/EU/UN sanctions lists.

## 3. API Request (curl)
```bash
curl -X POST https://api.tailoredtechworks.net/messages \
  -H "Authorization: Bearer ${JWT_TOKEN}" \
  -H "x-api-key: ${API_KEY}" \
  -H "Content-Type: application/json" \
  -d '{
    "message": "Verify identity and screen for sanctions. Documents: passport, utility bill, bank statement.",
    "folders": [
      {"id":"kyc-customer-docs","name":"Customer KYC Documents"}
    ],
    "tenantId": "{{tenantId}}",
    "metadata": {"workflow":"kyc_onboarding"}
  }'
```

## 4. Expected Response
```json
{
  "verification": {
    "nameMatch": true,
    "idDocumentValid": true,
    "addressMatch": true
  },
  "sanctionsScreening": {
    "ofac": "clear",
    "eu": "clear",
    "un": "clear"
  },
  "riskScore": 12,
  "recommendation": "Approve",
  "auditId": "aud_kyc_20260102_456"
}
```

## 5. Compliance Features
- FinCEN 7-year retention for KYC artifacts.
- OFAC/EU/UN sanctions lists integrated; alerts for matches.
- Folder isolation ensures only compliance team can access `kyc-customer-docs`.

## 6. Python Integration Code
```python
import requests
import os

API_BASE = os.getenv('API_BASE', 'https://api.tailoredtechworks.net')
JWT = os.getenv('JWT_TOKEN')
API_KEY = os.getenv('API_KEY')

def run_kyc_workflow(tenant_id, description):
    url = f"{API_BASE}/messages"
    headers = {
        'Authorization': f'Bearer {JWT}',
        'x-api-key': API_KEY,
        'Content-Type': 'application/json'
    }
    payload = {
        'message': description,
        'folders': [{'id':'kyc-customer-docs','name':'Customer KYC Documents'}],
        'tenantId': tenant_id,
        'metadata': {'workflow':'kyc_onboarding'}
    }
    r = requests.post(url, headers=headers, json=payload)
    r.raise_for_status()
    return r.json()

if __name__ == '__main__':
    print(run_kyc_workflow('YOUR_TENANT_ID', 'Verify passport 12345678; utility bill Jan 2026; bank stmt Feb 2026'))
```

## 7. Regulatory Compliance Checklist
- FinCEN recordkeeping: store KYC results for 5–7 years
- OFAC screening: automated, with manual escalation for potential hits
- SOC 2: access controls and folder isolation enforced

## 8. Cost Analysis
- Automated KYC via Fuzentry: $250/month (platform + usage) vs $62,500/month manual (10 analysts)

## 9. Audit Trail Retrieval
```bash
curl -X GET "https://api.tailoredtechworks.net/audit/{auditId}" \
  -H "Authorization: Bearer ${JWT_TOKEN}" \
  -H "x-api-key: ${API_KEY}"
```
