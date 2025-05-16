# marigold-property
New version of Marigold property application

## Auth example tables

```mermaid
erDiagram
    auth.users ||--o{ profiles : "has one"
    profiles ||--|{ user_organization_roles : "has many"
    organizations ||--|{ user_organization_roles : "has many"
    roles ||--|{ user_organization_roles : "defines role for"

    organizations ||--o{ service_requests : "owns many"
    organizations ||--o{ images : "owns many"
    organizations ||--o{ locations : "owns many"
    organizations ||--o{ technicians_app_data : "owns many"

    profiles ||--o{ technicians_app_data : "can be (if technician is a user)"

    service_requests {
        uuid id PK
        string title
        string description
        uuid organization_id FK
        uuid assigned_technician_id FK "nullable, if technician is a user/profile"
        timestamp created_at
        uuid created_by_user_id FK "references profiles(id)"
        -- other fields
    }

    images {
        uuid id PK
        string url
        uuid organization_id FK
        uuid uploaded_by_user_id FK "references profiles(id)"
        -- other fields
    }

    locations {
        uuid id PK
        string address
        uuid organization_id FK
        -- other fields
    }

    technicians_app_data {
        uuid id PK "could be same as profile_id if all technicians are users"
        uuid profile_id FK "references profiles(id), if technician is a user"
        string name "if technician is not a direct user or for display"
        uuid organization_id FK "technician belongs to this org"
        -- other technician-specific fields not in profiles
    }

    profiles {
        uuid id PK "references auth.users(id)"
        string full_name
        string avatar_url
        -- other app-specific user data
    }

    organizations {
        uuid id PK
        string name
        string slug "unique, for URLs perhaps"
        -- other tenant-specific data (subscription, settings)
    }

    roles {
        text name PK "e.g., 'admin', 'member', 'technician_lead', 'viewer'"
        string description
    }

    user_organization_roles {
        uuid user_id PK FK "references profiles(id)"
        uuid organization_id PK FK "references organizations(id)"
        text role_name PK FK "references roles(name)"
        -- other metadata like 'joined_at'
    }

    %% Supabase's built-in auth table
    %% auth.users {
    %%     uuid id PK
    %%     string email
    %%     -- other auth fields
    %% }
```
