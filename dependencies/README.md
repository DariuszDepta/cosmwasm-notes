# Dependencies

```mermaid
flowchart BT
    A("`**cosmwasm-core**
        3.0.1`"):::someclass 
        
    A --> B(Round edge)
    
    B --> C{Decision}
    C --> D[Result one]
    C --> E[Result two]
    
    style A fill:#9ccc65,stroke:#388e3c
```
