# Healthcare Prior Authorization - Fuzentry Example

## 1. Use Case
Hospital system automating prior authorization for total knee arthroplasty (TKA). The clinical team submits patient-specific clinical records and imaging summaries for rapid medical necessity assessment.

## 2. Scenario
A 68-year-old patient with advanced osteoarthritis, prior conservative therapy failure, and documented functional limitation requires prior authorization for TKA. The system evaluates history, medications, imaging findings (AP/lateral knee radiographs), and comorbidities.

## 3. API Request (curl)
```bash
curl -X POST https://api.tailoredtechworks.net/messages \
  -H "Authorization: Bearer ${JWT_TOKEN}" \
  -H "x-api-key: ${API_KEY}" \
  -H "Content-Type: application/json" \
  -d '{
    "message": "Assess medical necessity for total knee arthroplasty based on clinical notes and imaging summary.",
    "folders": [
      {
        "id": "clinical-records-orthopedics",
        "name": "Patient Clinical Records",
        "context": "HIPAA-compliant clinical documentation"
      }
    ],
    "tenantId": "{{tenantId}}",
    "metadata": {
      "userId": "dr.smith@hospital.com",
      "action": "prior_auth_submission"
    }
  }'
```

## 4. Expected Response
```json
{
  "response": "Findings support medical necessity for total knee arthroplasty. Radiographs show joint space narrowing, osteophytes, and subchondral sclerosis consistent with end-stage osteoarthritis. Conservative therapy trial documented: NSAIDs, PT, intra-articular corticosteroid—insufficient relief.",
  "recommendation": "Approve prior authorization for TKA",
  "auditId": "aud_20260102_orth_12345",
  "timestamp": "2026-01-02T15:30:00Z"
}
```

## 5. Compliance Features
- Folder isolation ensures only authorized clinical personnel can access `clinical-records-orthopedics`.
- Immutable audit trails retained for 7 years for regulatory compliance.
- Data encryption at rest and in transit; role-based access enforced by router-level policy.

## 6. Python Integration Code
```python
import requests
import os

API_BASE = os.getenv('API_BASE', 'https://api.tailoredtechworks.net')
JWT = os.getenv('JWT_TOKEN')
API_KEY = os.getenv('API_KEY')

def submit_prior_auth(patient_summary, tenant_id):
    url = f"{API_BASE}/messages"
    headers = {
        'Authorization': f'Bearer {JWT}',
        'x-api-key': API_KEY,
        'Content-Type': 'application/json'
    }
    payload = {
        'message': patient_summary,
        'folders': [
            {'id': 'clinical-records-orthopedics', 'name': 'Patient Clinical Records'}
        ],
        'tenantId': tenant_id,
        'metadata': {'action': 'prior_auth_submission'}
    }
    r = requests.post(url, headers=headers, json=payload)
    r.raise_for_status()
    return r.json()

if __name__ == '__main__':
    summary = '68yo M, progressive knee pain, failed conservative therapy, imaging: JSN, osteophytes.'
    print(submit_prior_auth(summary, 'YOUR_TENANT_ID'))
```

## 7. Cost Example
- Fuzentry automated prior auth: estimated $500/month (platform + metered usage) vs ~ $350,000 one-time to build an internal clinical rules engine and staff to operate it.

## 8. Audit Trail Retrieval
To fetch audit records for a given `auditId`:
```bash
curl -X GET "https://api.tailoredtechworks.net/audit/{auditId}" \
  -H "Authorization: Bearer ${JWT_TOKEN}" \
  -H "x-api-key: ${API_KEY}"
```

Compliance note: provide audit exports to regulators on request; all exports include tamper-evident signatures and timestamps.
