---
name: deacero-domain
description: Use when queries or transformations involve DeAcero's data infrastructure — legacy PE systems, SQL databases, table schemas, business rules, or data lineage. Always invoke before accessing DeAcero-specific data sources.
version: "0.1.0"
status: "stub — domain context to be populated from DeAcero knowledge base"
---

# DeAcero Domain Context

## Identity

This skill provides domain context for DeAcero's data infrastructure. It is the authoritative reference for business rules, data lineage, table schemas, and legacy system boundaries within DeAcero's PE (Procesamiento Empresarial) environment.

## Scope

- **Legacy PE Systems**: Understanding DeAcero's legacy production systems (Operacion, ARE, CMP, AG, and others).
- **SQL Databases**: Schema knowledge for 15+ production SQL Server instances.
- **Business Rules**: Documented rules for pricing, inventory, billing, logistics, and other domains.
- **Data Lineage**: Tracing how data flows from source systems through transformations to reporting.
- **Domain Classification**: Mapping business processes to their corresponding SQL objects and Kedro datasets.

## Business Domains

| Domain | Description | Key Systems |
|---|---|---|
| ventas | Sales and billing processes | ARE, facturación |
| inventario | Inventory control | CMP, almacenes |
| finanzas | Accounting and finance | contabilidad |
| produccion | Manufacturing processes | AG, producción |
| logistica | Shipping and distribution | embarques |
| compras | Procurement and suppliers | proveedores |

## Status

This skill is a stub. Domain context will be populated progressively from the DeAcero knowledge base as it is discovered through `squit` searches and archaeology sessions. Each confirmed domain finding must be persisted via the `learning-protocol` and referenced here.

## Related Skills

- `squit-data` — semantic search over the legacy SQL corpus
- `kedro-builder` — translates domain knowledge into Kedro nodes
- `software-archeologist` (from `archaeology`) — discovers domain structure from legacy code
