# Legal Contract Review - Multi-Expert Synthesis

## 1. Use Case
M&A legal team needs rapid review of a $50M stock purchase agreement (SPA) focusing on indemnification, non-compete, and earn-out provisions.

## 2. Scenario
Upload SPA excerpts and ask for combined legal, risk, and negotiation perspectives to highlight problematic clauses and produce redlines and negotiation playbook.

## 3. API Request (curl)
```bash
curl -X POST https://api.tailoredtechworks.net/messages \
  -H "Authorization: Bearer ${JWT_TOKEN}" \
  -H "x-api-key: ${API_KEY}" \
  -H "Content-Type: application/json" \
  -d '{
    "message": "Review attached SPA excerpts for indemnification caps, survival periods, and escrow structure. Provide redlines and negotiation points.",
    "folders": [
      {"id": "contract-expert","name":"Contract Law Expert"},
      {"id": "risk-expert","name":"Corporate Risk"},
      {"id": "negotiation-expert","name":"Deal Negotiation"}
    ],
    "tenantId": "{{tenantId}}"
  }'
```

## 4. Expected Response
```json
{
  "synthesizedAnswer": "Key risks: unlimited indemnity exposure for pre-closing liabilities; propose cap at 10% of purchase price; carve-outs for fraud and tax; shorten survival from 3 years to 18 months for general reps.",
  "geddeTrace": [
    {"expert":"Contract Law Expert","view":"We recommend adding specific definitions and limiting consequential damages."},
    {"expert":"Corporate Risk","view":"Escrow should be 5% for 18 months with periodic releases."},
    {"expert":"Deal Negotiation","view":"Push for seller escrow and milestone-based earn-out trigerring."}
  ],
  "arbitration": {"conflictResolution":"Weighted consensus with Contract Law Expert as tie-breaker"},
  "auditId": "aud_legal_20260102_789"
}
```

## 5. Compliance Features
- Attorney-client privileged folder isolation: folders marked `privileged` are access-restricted.
- 10-year audit retention and eDiscovery-ready exports.

## 6. TypeScript Integration Code
```ts
import fetch from 'node-fetch';

const API_BASE = process.env.API_BASE || 'https://api.tailoredtechworks.net';
const JWT = process.env.JWT_TOKEN;
const API_KEY = process.env.API_KEY;

export async function requestContractReview(tenantId: string, text: string) {
  const res = await fetch(`${API_BASE}/messages`, {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${JWT}`,
      'x-api-key': API_KEY,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      message: text,
      folders: [
        { id: 'contract-expert', name: 'Contract Law Expert' },
        { id: 'risk-expert', name: 'Corporate Risk' },
        { id: 'negotiation-expert', name: 'Deal Negotiation' }
      ],
      tenantId
    })
  });
  if (!res.ok) throw new Error(`Request failed ${res.status}`);
  return res.json();
}
```

## 7. Cost Comparison
- Multi-expert synthesis: $0.05 per orchestration (parallel expert queries + arbitration).
- Sequential prompts (one after another): typically $0.15+ per orchestration due to repeated context handling.

## 8. Competitive Advantage
- Architecturally, Fuzentry attaches governance, folder-scoped memory, and arbitration across experts — something off-the-shelf single-model chains (LangChain + LLM) do not provide without custom orchestration and persistent policy control.
