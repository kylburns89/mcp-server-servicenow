---
name: servicenow-cmdb
description: CMDB Data Foundations — explore CI classes, relationships, health, and CSDM compliance
tools: [list_ci, get_ci, get_ci_relationships, list_records, get_record, get_table_schema]
---

# ServiceNow CMDB Data Foundations

## When to use this skill

Use when the user asks about CMDB exploration, CI relationships, data quality, CMDB health, or CSDM compliance on a ServiceNow instance. This skill maps to the CIS - Data Foundations certification domains.

## Workflows

### 1. Explore CI Class Hierarchy

Discover what CI classes exist and their structure.

1. Get the base CI table schema:
   ```
   get_table_schema(table_name="cmdb_ci")
   ```
2. List CIs by class to see what's populated:
   ```
   list_ci(class_name="cmdb_ci_server", limit=10)
   list_ci(class_name="cmdb_ci_computer", limit=10)
   list_ci(class_name="cmdb_ci_app_server", limit=10)
   ```
3. Check class-specific fields by getting the schema for a subclass:
   ```
   get_table_schema(table_name="cmdb_ci_server")
   ```
4. Summarize which classes have data and how many CIs each contains.

### 2. Map CI Dependencies

Trace upstream and downstream dependencies for a given CI.

1. Find the target CI:
   ```
   list_ci(class_name="cmdb_ci", query="nameLIKE<search_term>", limit=10)
   ```
2. Get the CI details:
   ```
   get_ci(sys_id="<ci_sys_id>")
   ```
3. Get all relationships:
   ```
   get_ci_relationships(sys_id="<ci_sys_id>")
   ```
4. For each related CI, optionally recurse one level deeper to build a dependency map.
5. Present results as an upstream/downstream tree showing relationship types.

### 3. Check CMDB Health

Query the CMDB Health Dashboard tables for data quality KPIs.

1. List health metrics:
   ```
   list_records(table_name="cmdb_health_audit", limit=20, order_by="-sys_created_on")
   ```
2. Check specific health rules:
   ```
   list_records(table_name="cmdb_health_rule", query="active=true", limit=20)
   ```
3. Look at health scores by CI class:
   ```
   list_records(table_name="cmdb_health_config", limit=20)
   ```
4. Summarize: overall health score, top failing rules, classes with lowest scores.

### 4. Investigate Data Quality

Drill into data quality issues for specific CI classes.

1. Find CIs with missing mandatory fields:
   ```
   list_ci(class_name="cmdb_ci_server", query="ip_addressISEMPTY", limit=20)
   ```
2. Find duplicate CIs (same name, different sys_id):
   ```
   list_ci(class_name="cmdb_ci_server", query="nameLIKE<name>", limit=20)
   ```
3. Check for orphaned CIs (no relationships):
   ```
   # Get a CI and check if get_ci_relationships returns empty
   get_ci_relationships(sys_id="<ci_sys_id>")
   ```
4. Query data quality rules:
   ```
   list_records(table_name="dl_definition", query="active=true", limit=20)
   ```
5. Report: count of issues by category, affected CI classes, recommended actions.

### 5. CSDM Service Taxonomy

Explore the Common Service Data Model service hierarchy.

1. List Business Services:
   ```
   list_ci(class_name="cmdb_ci_service_business", limit=20)
   ```
2. List Technical Services:
   ```
   list_ci(class_name="cmdb_ci_service_technical", limit=20)
   ```
3. List Application Services:
   ```
   list_ci(class_name="cmdb_ci_service", query="sys_class_nameLIKEservice", limit=20)
   ```
4. For a specific service, map relationships to see which CIs support it:
   ```
   get_ci_relationships(sys_id="<service_sys_id>")
   ```
5. Check service-to-offering mappings:
   ```
   list_records(table_name="service_offering", limit=20)
   ```
6. Present the service taxonomy: business → technical → application services, with supporting CIs.

## Tips

- Use `fields` parameter on `list_ci` and `list_records` to limit returned data to relevant columns.
- Use encoded queries for complex filters: `active=true^sys_class_name=cmdb_ci_server^ip_addressISNOTEMPTY`.
- CMDB health tables may not exist on all instances — if a table returns 404, the Health Dashboard module may not be activated.
- CSDM class names vary by SN version. If a class returns no results, try `get_table_schema` to confirm it exists.
