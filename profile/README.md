# Reekhav Engineers

> Precision engineering software for high-voltage overhead transmission line design, conductor sag-tension analysis, and tower clearance verification.

---

## About Reekhav Engineers

**Reekhav Engineers** builds next-generation utility and electrical grid engineering platforms. Our flagship product, **LineDesign Studio**, is an enterprise-grade cloud system that transforms complex transmission line design workflows—from catenary curve physics and environmental wind loading to statutory clearance spotting—into an interactive, high-precision web interface.

---

## System Architecture

```text
+-----------------------------------------------------------------------------------+
|                                Client & Edge Layer                                |
|                                                                                   |
|   Production Frontend: designer.reekhav.com (Cloudflare Worker: designer-frontend) |
|   UAT Frontend:        uat.designer.reekhav.com (Worker: designer-frontend-uat)   |
|   Security Gateway:    Cloudflare Turnstile Bot Defense                           |
+-----------------------------------------------------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|                               Application Services                                |
|                                                                                   |
|   - Authentication & RBAC Service (JWT, Microsoft Graph API, Impersonation)       |
|   - Sag-Tension Calculation Engine (IS 802 / IEC Catenary Physics)                |
|   - Multi-Sheet State & Provenance Engine (Interactive Excel-grade Workbook)      |
|   - Wire & Conductor Catalog Service (Master library & custom project wires)      |
|   - Project Delegation & Review Audit Logging                                     |
+-----------------------------------------------------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|                               Storage Infrastructure                              |
|                                                                                   |
|   Production: Azure Cosmos DB / DocumentDB vCore (30 GB Cluster)                  |
|   UAT:        MongoDB Atlas QA Cluster (designer_projects_qa)                      |
+-----------------------------------------------------------------------------------+
```

---

## Live Environments & Infrastructure

### 1. Production Environment

| Component | Identifier / URL | Details |
| :--- | :--- | :--- |
| **Frontend Application** | [designer.reekhav.com](https://designer.reekhav.com) | Cloudflare Worker (`designer-frontend`) delivering React 18 / TypeScript single-page application. |
| **Backend API Gateway** | [backend.designer.reekhav.com](https://backend.designer.reekhav.com) | Azure Linux App Service (`designer`), hosted in India South Central. |
| **Direct Azure Endpoint**| [designer-accwhadabggmd2cj...azurewebsites.net](https://designer-accwhadabggmd2cj.indiasouthcentral-01.azurewebsites.net) | Direct HTTPS host for backend service operations. |
| **API Documentation** | [/api-docs](https://designer-accwhadabggmd2cj.indiasouthcentral-01.azurewebsites.net/api-docs) | Interactive OpenAPI 3.0 (Swagger UI) documentation. |
| **API Specification** | [/apispec.json](https://designer-accwhadabggmd2cj.indiasouthcentral-01.azurewebsites.net/apispec.json) | Raw OpenAPI 3.0 JSON specification. |
| **Database Cluster** | Azure Cosmos DB (DocumentDB vCore) | `designer-mongodb.mongocluster.cosmos.azure.com` (30 GB storage tier, database `designer_projects`). |

### 2. User Acceptance Testing (UAT) Environment

| Component | Identifier / URL | Details |
| :--- | :--- | :--- |
| **UAT Frontend** | [uat.designer.reekhav.com](https://uat.designer.reekhav.com) | Cloudflare Worker (`designer-frontend-uat`) isolated for staging validation. |
| **UAT Backend API** | [designer-backend-node-uat...azurewebsites.net](https://designer-backend-node-uat-dzh0auf8ercffkbv.westus3-01.azurewebsites.net) | Azure Linux App Service (`designer-backend-node-uat`), hosted in West US 3. |
| **UAT API Documentation** | [/api-docs](https://designer-backend-node-uat-dzh0auf8ercffkbv.westus3-01.azurewebsites.net/api-docs) | Live staging OpenAPI 3.0 documentation. |
| **UAT Database** | MongoDB Atlas QA Cluster | Isolated staging cluster with database `designer_projects_qa`. |

---

## Repositories

| Repository | Scope | Stack |
| :--- | :--- | :--- |
| [`designer-frontend`](https://github.com/ReekhavEngineers/designer-frontend) | Single-page spreadsheet interface & engineering HUD | React 18, TypeScript, Redux Toolkit, Vite, Cloudflare Workers |
| [`designer-backend-node`](https://github.com/ReekhavEngineers/designer-backend-node) | REST API calculation engine & data governance | Node.js, Express, TypeScript, Azure Cosmos DB, OpenAPI 3.0 |
| [`.github`](https://github.com/ReekhavEngineers/.github) | Organization profile, community health, and workflows | Markdown, GitHub Actions |

---

## Core Capabilities

1. **Sag-Tension Physics Engine**:
   - High-precision calculation of conductor catenary curves across operational temperatures (0°C to 85°C).
   - Ruling span state-change cubic equation solving for tension and sag.
   - Tension-Sag-Distance (TSD) tables for field erection and stringing charts.
2. **Environmental & Wind Loading (IS 802 / IEC)**:
   - Dynamic wind pressure and resultant unit weight derivations under combined wind and ice loading.
   - Terrain categories, gust factors, and reliability return periods.
3. **Tower Height & Ground Clearance Verification**:
   - Automated mid-span ground clearance checks against statutory codes.
   - Obstacle elevation profiling and vertical clearance spotting.
4. **Enterprise Multi-Project Collaboration**:
   - Role-Based Access Control (RBAC): Administrators, Line Managers, Design Engineers.
   - Review workflow lifecycle: Draft &rarr; Submitted &rarr; Approved / Modification Requested.
   - Full audit logging and administrator session impersonation.

---

## Engineering Standards

- **Strict TypeScript**: 100% typed with zero `any` usage, discriminated unions, and Zod boundary validation.
- **Single Source of Truth**: Backend handles 100% of physical and engineering formulas; computed values are returned directly on save.
- **Security & Bot Defense**: Universal Cloudflare Turnstile token validation and strict CORS origin enforcement.
