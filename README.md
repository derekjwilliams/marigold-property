# marigold-property
New version of Marigold property application

## Auth example tables

```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
```

```mermaid
erDiagram
    users ||--o{ profiles : "has one (FK on profiles.id)"
    profiles ||--|{ user_organization_roles : "links to"
    organizations ||--|{ user_organization_roles : "links to"
    roles ||--|{ user_organization_roles : "defines role for"

    organizations ||--o{ service_requests : "owns many"
    organizations ||--o{ images : "owns many"
    organizations ||--o{ locations : "owns many"
    organizations ||--o{ technicians_app_data : "owns many"

    profiles ||--o{ technicians_app_data : "can be (if technician is a user)"
    profiles ||--o{ service_requests_created_by : "created by"
    profiles ||--o{ images_uploaded_by : "uploaded by"

    service_requests {
        uuid id PK
        string title
        string description
        uuid organization_id FK "to organizations.id"
        uuid assigned_technician_id FK "to technicians_app_data.id (nullable)"
        timestamp created_at
        uuid created_by_user_id FK "to profiles.id"
    }

    images {
        uuid id PK
        string url
        uuid organization_id FK "to organizations.id"
        uuid uploaded_by_user_id FK "to profiles.id"
    }

    locations {
        uuid id PK
        string address
        uuid organization_id FK "to organizations.id"
    }

    technicians_app_data {
        uuid id PK
        uuid profile_id FK "to profiles.id (nullable)"
        string name
        uuid organization_id FK "to organizations.id"
    }

    profiles {
        uuid id PK "FK to users.id"
        string full_name
        string avatar_url
    }

    organizations {
        uuid id PK
        string name
        string slug UK "unique"
    }

    roles {
        text name PK
        string description
    }

    user_organization_roles {
        uuid user_id PK "FK to profiles.id"
        uuid organization_id PK "FK to organizations.id"
        text role_name PK "FK to roles.name"
    }

    users {
        uuid id PK "This represents Supabase auth.users"
        string email
    }

    %% Explicitly showing some relationships that were implied or described in text
    service_requests }o--|| profiles as service_requests_created_by : "created_by_user_id"
    images }o--|| profiles as images_uploaded_by : "uploaded_by_user_id"
```
