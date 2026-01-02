# Fuzentry - AWS Marketplace Getting Started

Fuzentry™ is an enterprise AI orchestration platform offered via AWS Marketplace. It provides single-point entry governance, folder-scoped memory isolation, and immutable audit trails for regulated workloads in healthcare, legal, and financial services.

Quick links
- Quickstart: QUICKSTART.md
- Troubleshooting: TROUBLESHOOTING.md
- Postman collection: postman/fuzentry-marketplace-collection.json

Why Fuzentry™ (brief)
- ✅ Router-level governance: central policy enforcement for all AI requests
- ✅ Folder-scoped memory isolation: data never crosses folder boundaries
- ✅ Immutable audit trails: tamper-evident logs retained for regulatory compliance

5-minute Quick Start (high level)
1. Subscribe on AWS Marketplace
2. Receive subscription email with API credentials
3. Use the Postman collection to call `/health`, then `/messages`
4. Monitor usage at `/usage/current`

Key features
- 🔐 Dual authentication: Cognito JWT + Marketplace API key
- 📁 Folder governance: per-folder policies and retention
- 📊 Usage metering: per-orchestration metering (AWS Marketplace)
- 🧾 Auditability: immutable, queryable audit records

Industry examples included
- Healthcare prior authorization
- Legal contract review (multi-expert synthesis)
- FinTech KYC/AML workflow

Resources
- Postman workspace: `postman/fuzentry-marketplace-collection.json`
- API docs: https://docs.fuzentry.ai
- Support: support@tailoredtechworks.net

Support & SLA
- Production support: enterprise SLA available; contact support@tailoredtechworks.net
# Fuzentry-getting-started
Customer onboarding resources for Fuzentry AWS Marketplace integration
