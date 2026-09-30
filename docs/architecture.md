# Module Architecture

## Scope

This page documents the publishable architecture of the treatment-cycle quotation module. It intentionally excludes proprietary source code, internal endpoints, hostnames, infrastructure, credentials, and the physical database schema.

The solution is a full-stack module integrated into an existing hospital application. Its multi-page JavaScript interface communicates through REST/JSON with business services developed in C# and .NET, backed by SQL Server.

## System context

```mermaid
flowchart LR
    U[Authorized internal user] --> UI[Quotation web module]
    UI -->|REST / JSON| API[C# / .NET API]
    API --> DB[(SQL Server)]
    API --> HIS[Existing hospital services]
```

The exact service boundaries, hosting model, authentication infrastructure, and physical schema are deliberately not disclosed.

## Main components

```mermaid
flowchart TB
    NAV[Existing navigation] --> PAT[Patient workflow]
    PAT --> QUOTE[Quotation interface]
    QUOTE --> CAL[Schedule generation]
    QUOTE --> CAT[Billable-item selection]
    QUOTE --> TOT[Displayed amount calculation]
    QUOTE --> HTTP[Axios client]
    HTTP --> API[.NET REST controllers]
    API --> BL[Business services]
    BL --> DATA[Data access]
    DATA --> SQL[(SQL Server)]
    API --> VIEW[Quotation lookup]
```

- **Patient workflow:** selects or resumes a record in the existing application.
- **Quotation interface:** captures general information, practitioners, and treatment-cycle parameters.
- **Schedule generation:** calculates projected dates from duration, interval, and selected weekdays.
- **Billable-item selection:** loads procedures, examinations, services, products, and consumables.
- **Amount calculation:** provides immediate quantity, unit-price, and total feedback in the interface.
- **REST API:** validates requests and coordinates quotation business operations.
- **Persistence:** stores approved business data in SQL Server.
- **Quotation lookup:** retrieves a quotation summary by its identifier.

## Technology mapping

| Layer | Technologies |
| --- | --- |
| Interface | JavaScript, HTML5, CSS3, Handlebars |
| UI foundation | Bootstrap, jQuery |
| HTTP integration | Axios, REST/JSON |
| Front-end build | Gulp, npm |
| Backend | C#, .NET |
| Persistence | SQL Server |
| Version control | Git |

## Conceptual domain model

This model explains the business concepts without reproducing the company's physical database schema.

```mermaid
erDiagram
    PATIENT ||--o{ QUOTATION : concerns
    PRACTITIONER ||--o{ QUOTATION : associated_with
    QUOTATION ||--o{ TREATMENT_CYCLE : schedules
    QUOTATION ||--o{ QUOTATION_LINE : contains
    TREATMENT_CYCLE ||--o{ CYCLE_PRODUCT : uses
    CATALOG_ITEM ||--o{ QUOTATION_LINE : references
    CATALOG_ITEM ||--o{ CYCLE_PRODUCT : references
```

Cardinalities are illustrative and must not be treated as the proprietary physical schema.

## Quotation preparation sequence

```mermaid
sequenceDiagram
    actor User as Authorized user
    participant UI as Web interface
    participant API as .NET API
    participant DB as SQL Server

    User->>UI: Select patient record
    UI->>API: Request authorized information
    API->>DB: Retrieve required data
    DB-->>API: Business data
    API-->>UI: Authorized patient context
    User->>UI: Configure quotation and cycles
    UI->>UI: Generate projected dates
    User->>UI: Add items and quantities
    UI->>UI: Calculate displayed amounts
    User->>UI: Submit quotation
    UI->>API: Send validated request
    API->>DB: Persist quotation data
    API-->>UI: Return result or structured error
```

## Architectural boundaries

- The server must remain the source of truth for authorization, prices, final amounts, and consistency rules.
- Client-side calculations are usability aids and require server-side verification.
- Sensitive patient information must be minimized in browser state and logs.
- Multi-step persistence should be protected by an appropriate transaction boundary.
- Error responses must avoid exposing internal implementation or sensitive data.

## Recommended evolution

```mermaid
flowchart LR
    UI[Web interface] --> V[Client validation]
    V --> C[Centralized API client]
    C --> A[Quotation aggregate endpoint]
    A --> S[Server validation and authorization]
    S --> T[Business transaction]
    T --> DB[(SQL Server)]
```

Recommended next steps include a centralized API client, explicit DTO validation, transactional quotation submission, versioned database migrations, automated API and business-rule tests, structured observability, and documented rollback procedures.
