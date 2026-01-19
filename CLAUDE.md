# CLAUDE.md

Reusable Terraform module for Google Cloud SQL PostgreSQL instances. Supports primary instances with read replicas, IAM authentication, automated backups, and Query Insights.

## Structure

```
main.tf           # Primary instance, databases, users
read_replica.tf   # Read replica configuration
variables.tf      # Input variables
outputs.tf        # Module outputs
versions.tf       # Provider version constraints
```

## Notes

- Terraform >= 0.13, providers: google (>= 4.0), google-beta, null, random
- Module only - reference via `github.com/dapperlabs-platform/terraform-google-database?ref=<version>`
- `disk_size` changes ignored due to CloudSQL autoresize
- Set `deletion_protection = true` for production
