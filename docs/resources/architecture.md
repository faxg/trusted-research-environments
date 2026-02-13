# Architecture

This page contains architectural diagrams, patterns, and design references for Trusted Research Environments.

## High-Level Architecture

### Basic TRE Architecture

```mermaid
graph TB
    subgraph External
        R[Researcher]
        D[Data Provider]
    end
    
    subgraph "TRE Boundary"
        subgraph "Access Layer"
            A[Authentication]
            G[Gateway]
        end
        
        subgraph "Workspace Layer"
            W1[Workspace 1]
            W2[Workspace 2]
            W3[Workspace N]
        end
        
        subgraph "Data Layer"
            DS[Data Store]
            DC[Data Catalog]
        end
        
        subgraph "Management Layer"
            IAM[Identity & Access]
            LOG[Logging & Audit]
            MON[Monitoring]
        end
    end
    
    R -->|Request Access| A
    A -->|Authenticated| G
    G -->|Provision| W1
    G -->|Provision| W2
    G -->|Provision| W3
    W1 -->|Read| DS
    W2 -->|Read| DS
    W3 -->|Read| DS
    W1 -.->|Query| DC
    D -->|Ingest| DS
    IAM -.->|Control| G
    LOG -.->|Track| W1
    MON -.->|Monitor| W1
```

This diagram shows the fundamental components of a TRE:

- **Access Layer**: Authentication and gateway for researcher access
- **Workspace Layer**: Isolated environments for different projects
- **Data Layer**: Secure storage and cataloging of sensitive data
- **Management Layer**: Cross-cutting concerns like identity, logging, and monitoring

## Detailed Component Architectures

### Data Flow Architecture

```mermaid
flowchart LR
    subgraph Ingress
        DP[Data Provider]
        DV[Data Validation]
        DC[Data Classification]
    end
    
    subgraph Storage
        RAW[Raw Data]
        CUR[Curated Data]
        DER[Derived Data]
    end
    
    subgraph Processing
        W[Workspace]
        AN[Analysis Tools]
    end
    
    subgraph Egress
        REV[Review Process]
        APP[Approval]
        OUT[Output Release]
    end
    
    DP --> DV
    DV --> DC
    DC --> RAW
    RAW --> CUR
    CUR --> W
    W --> AN
    AN --> DER
    DER --> REV
    REV --> APP
    APP --> OUT
```

### Security Architecture

```mermaid
graph TB
    subgraph "Security Zones"
        subgraph "DMZ"
            LB[Load Balancer]
            WAF[Web Application Firewall]
        end
        
        subgraph "Application Zone"
            APP[Application Servers]
            API[API Gateway]
        end
        
        subgraph "Data Zone"
            DB[(Database)]
            FS[File Storage]
        end
        
        subgraph "Management Zone"
            IAM[IAM Service]
            LOG[Log Aggregation]
            SIEM[SIEM]
        end
    end
    
    Internet[Internet] --> WAF
    WAF --> LB
    LB --> APP
    APP --> API
    API --> DB
    API --> FS
    IAM -.->|Authorize| API
    APP -.->|Log| LOG
    LOG -.->|Analyze| SIEM
```

## Network Architecture

### Network Segmentation

```mermaid
graph LR
    subgraph "Public Network"
        INT[Internet]
    end
    
    subgraph "Perimeter Network"
        FW[Firewall]
        VPN[VPN Gateway]
    end
    
    subgraph "Internal Network"
        subgraph "Workspace VLAN"
            WS[Workspaces]
        end
        
        subgraph "Data VLAN"
            DATA[Data Services]
        end
        
        subgraph "Management VLAN"
            MGMT[Management Services]
        end
    end
    
    INT --> FW
    INT --> VPN
    FW --> WS
    VPN --> WS
    WS --> DATA
    MGMT -.->|Monitor| WS
    MGMT -.->|Monitor| DATA
```

## Deployment Patterns

### Cloud-Based TRE

```mermaid
graph TB
    subgraph "Cloud Provider"
        subgraph "Region A"
            subgraph "VNet"
                SUB1[Subnet: Gateway]
                SUB2[Subnet: Workspaces]
                SUB3[Subnet: Data]
            end
            
            LB[Load Balancer]
            VM1[VM: Workspace 1]
            VM2[VM: Workspace 2]
            STOR[(Blob Storage)]
        end
        
        subgraph "Region B - DR"
            BACKUP[(Backup Storage)]
        end
    end
    
    USER[Users] --> LB
    LB --> SUB1
    SUB1 --> SUB2
    SUB2 --> VM1
    SUB2 --> VM2
    VM1 --> SUB3
    VM2 --> SUB3
    SUB3 --> STOR
    STOR -.->|Replicate| BACKUP
```

### Hybrid TRE

```mermaid
graph LR
    subgraph "On-Premises"
        LOCAL[(Local Data)]
        DC[Data Center]
    end
    
    subgraph "Cloud"
        CLOUD[Cloud TRE]
        COMPUTE[Compute Resources]
    end
    
    subgraph "Researchers"
        R1[Remote Users]
        R2[On-Site Users]
    end
    
    LOCAL -->|Secure Sync| CLOUD
    DC -->|VPN| CLOUD
    R1 -->|Internet| CLOUD
    R2 -->|VPN| DC
    CLOUD --> COMPUTE
```

## Identity & Access Management

```mermaid
sequenceDiagram
    participant U as User
    participant IDP as Identity Provider
    participant TRE as TRE Gateway
    participant WS as Workspace
    participant DATA as Data Store
    
    U->>IDP: 1. Authenticate
    IDP->>U: 2. Issue Token
    U->>TRE: 3. Request Access + Token
    TRE->>IDP: 4. Validate Token
    IDP->>TRE: 5. Token Valid
    TRE->>TRE: 6. Check Permissions
    TRE->>WS: 7. Provision Workspace
    WS->>U: 8. Access Granted
    U->>WS: 9. Request Data
    WS->>DATA: 10. Query Data
    DATA->>WS: 11. Return Data
    WS->>U: 12. Display Results
```

## Design Patterns

### Multi-Tenancy Pattern

**Problem**: Multiple research projects need isolated environments within the same TRE infrastructure.

**Solution**: Implement logical separation using:
- Dedicated workspaces per project
- Role-based access control
- Data segregation
- Resource quotas

### Data Minimization Pattern

**Problem**: Researchers may not need access to full datasets.

**Solution**: 
- Implement data views with column/row-level filtering
- Use synthetic or anonymized data for development
- Provide aggregate data where possible

### Auditability Pattern

**Problem**: Need comprehensive audit trails for compliance.

**Solution**:
- Log all access attempts and data operations
- Immutable audit logs
- Centralized log aggregation
- Regular audit reviews

---

!!! tip "Adding Diagrams"
    We use Mermaid for diagrams. See the [Contributing Guide](../about/contributing.md) for examples.
