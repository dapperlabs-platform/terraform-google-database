# terraform-google-sql for PostgreSQL

Note: CloudSQL provides [disk autoresize](https://cloud.google.com/sql/docs/mysql/instance-settings#automatic-storage-increase-2ndgen) feature which can cause a [Terraform configuration drift](https://www.hashicorp.com/blog/detecting-and-managing-drift-with-terraform) due to the value in `disk_size` variable, and hence any updates to this variable is ignored in the [Terraform lifecycle](https://www.terraform.io/docs/configuration/resources.html#ignore_changes).

<!-- BEGIN_TF_DOCS -->
Copyright 2019 Google LLC

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

     http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 0.13 |
| <a name="requirement_google"></a> [google](#requirement\_google) | >= 4.0 |
| <a name="requirement_null"></a> [null](#requirement\_null) | >= 3.1.0 |
| <a name="requirement_random"></a> [random](#requirement\_random) | >= 3.1.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_google"></a> [google](#provider\_google) | >= 4.0 |
| <a name="provider_google-beta"></a> [google-beta](#provider\_google-beta) | n/a |
| <a name="provider_null"></a> [null](#provider\_null) | >= 3.1.0 |
| <a name="provider_random"></a> [random](#provider\_random) | >= 3.1.0 |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [google-beta_google_sql_database_instance.default](https://registry.terraform.io/providers/hashicorp/google-beta/latest/docs/resources/google_sql_database_instance) | resource |
| [google_project_iam_member.iam_binding](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/project_iam_member) | resource |
| [google_sql_database.additional_databases](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/sql_database) | resource |
| [google_sql_database.default](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/sql_database) | resource |
| [google_sql_database_instance.replicas](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/sql_database_instance) | resource |
| [google_sql_user.additional_users](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/sql_user) | resource |
| [google_sql_user.default](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/sql_user) | resource |
| [google_sql_user.iam_account](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/sql_user) | resource |
| [null_resource.module_depends_on](https://registry.terraform.io/providers/hashicorp/null/latest/docs/resources/resource) | resource |
| [random_id.suffix](https://registry.terraform.io/providers/hashicorp/random/latest/docs/resources/id) | resource |
| [random_id.user-password](https://registry.terraform.io/providers/hashicorp/random/latest/docs/resources/id) | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_activation_policy"></a> [activation\_policy](#input\_activation\_policy) | The activation policy for the master instance.Can be either `ALWAYS`, `NEVER` or `ON_DEMAND`. | `string` | `"ALWAYS"` | no |
| <a name="input_additional_databases"></a> [additional\_databases](#input\_additional\_databases) | A list of databases to be created in your cluster | <pre>list(object({<br>    name      = string<br>    charset   = string<br>    collation = string<br>  }))</pre> | `[]` | no |
| <a name="input_additional_users"></a> [additional\_users](#input\_additional\_users) | A list of users to be created in your cluster | <pre>list(object({<br>    name     = string<br>    password = string<br>  }))</pre> | `[]` | no |
| <a name="input_availability_type"></a> [availability\_type](#input\_availability\_type) | The availability type for the master instance.This is only used to set up high availability for the PostgreSQL instance. Can be either `ZONAL` or `REGIONAL`. | `string` | `"ZONAL"` | no |
| <a name="input_backup_configuration"></a> [backup\_configuration](#input\_backup\_configuration) | The backup\_configuration settings subblock for the database setings | <pre>object({<br>    enabled                        = bool<br>    start_time                     = string<br>    location                       = string<br>    point_in_time_recovery_enabled = bool<br>    transaction_log_retention_days = string<br>    retained_backups               = number<br>    retention_unit                 = string<br>  })</pre> | <pre>{<br>  "enabled": false,<br>  "location": null,<br>  "point_in_time_recovery_enabled": false,<br>  "retained_backups": null,<br>  "retention_unit": null,<br>  "start_time": null,<br>  "transaction_log_retention_days": null<br>}</pre> | no |
| <a name="input_create_timeout"></a> [create\_timeout](#input\_create\_timeout) | The optional timout that is applied to limit long database creates. | `string` | `"15m"` | no |
| <a name="input_database_flags"></a> [database\_flags](#input\_database\_flags) | The database flags for the master instance. See [more details](https://cloud.google.com/sql/docs/postgres/flags) | <pre>list(object({<br>    name  = string<br>    value = string<br>  }))</pre> | `[]` | no |
| <a name="input_database_version"></a> [database\_version](#input\_database\_version) | The database version to use | `string` | n/a | yes |
| <a name="input_db_charset"></a> [db\_charset](#input\_db\_charset) | The charset for the default database | `string` | `""` | no |
| <a name="input_db_collation"></a> [db\_collation](#input\_db\_collation) | The collation for the default database. Example: 'en\_US.UTF8' | `string` | `""` | no |
| <a name="input_db_name"></a> [db\_name](#input\_db\_name) | The name of the default database to create | `string` | `"default"` | no |
| <a name="input_delete_timeout"></a> [delete\_timeout](#input\_delete\_timeout) | The optional timout that is applied to limit long database deletes. | `string` | `"15m"` | no |
| <a name="input_deletion_protection"></a> [deletion\_protection](#input\_deletion\_protection) | Used to block Terraform from deleting a SQL Instance. | `bool` | `true` | no |
| <a name="input_disk_autoresize"></a> [disk\_autoresize](#input\_disk\_autoresize) | Configuration to increase storage size. | `bool` | `true` | no |
| <a name="input_disk_size"></a> [disk\_size](#input\_disk\_size) | The disk size for the master instance. | `number` | `10` | no |
| <a name="input_disk_type"></a> [disk\_type](#input\_disk\_type) | The disk type for the master instance. | `string` | `"PD_SSD"` | no |
| <a name="input_edition"></a> [edition](#input\_edition) | The edition of the master instance | `string` | `null` | no |
| <a name="input_enable_default_db"></a> [enable\_default\_db](#input\_enable\_default\_db) | Enable or disable the creation of the default database | `bool` | `true` | no |
| <a name="input_enable_default_user"></a> [enable\_default\_user](#input\_enable\_default\_user) | Enable or disable the creation of the default user | `bool` | `true` | no |
| <a name="input_encryption_key_name"></a> [encryption\_key\_name](#input\_encryption\_key\_name) | The full path to the encryption key used for the CMEK disk encryption | `string` | `null` | no |
| <a name="input_iam_user_emails"></a> [iam\_user\_emails](#input\_iam\_user\_emails) | A list of IAM users to be created in your cluster | `list(string)` | `[]` | no |
| <a name="input_insights_config"></a> [insights\_config](#input\_insights\_config) | The insights\_config settings for the database.<br>  {<br>    query\_string\_length     = number<br>    record\_application\_tags = bool<br>    record\_client\_address   = bool<br>  } | <pre>object({<br>    query_insights_enabled  = bool<br>    query_string_length     = number<br>    record_application_tags = bool<br>    record_client_address   = bool<br>  })</pre> | <pre>{<br>  "query_insights_enabled": true,<br>  "query_string_length": 1024,<br>  "record_application_tags": false,<br>  "record_client_address": false<br>}</pre> | no |
| <a name="input_ip_configuration"></a> [ip\_configuration](#input\_ip\_configuration) | The ip configuration for the master instances. | <pre>object({<br>    authorized_networks = list(map(string))<br>    ipv4_enabled        = bool<br>    private_network     = string<br>    ssl_mode            = optional(string)<br>  })</pre> | <pre>{<br>  "authorized_networks": [],<br>  "ipv4_enabled": true,<br>  "private_network": null<br>}</pre> | no |
| <a name="input_maintenance_window_day"></a> [maintenance\_window\_day](#input\_maintenance\_window\_day) | The day of week (1-7) for the master instance maintenance. | `number` | `1` | no |
| <a name="input_maintenance_window_hour"></a> [maintenance\_window\_hour](#input\_maintenance\_window\_hour) | The hour of day (0-23) maintenance window for the master instance maintenance. | `number` | `23` | no |
| <a name="input_maintenance_window_update_track"></a> [maintenance\_window\_update\_track](#input\_maintenance\_window\_update\_track) | The update track of maintenance window for the master instance maintenance.Can be either `canary` or `stable`. | `string` | `"canary"` | no |
| <a name="input_module_depends_on"></a> [module\_depends\_on](#input\_module\_depends\_on) | List of modules or resources this module depends on. | `list(any)` | `[]` | no |
| <a name="input_name"></a> [name](#input\_name) | The name of the Cloud SQL resources | `string` | n/a | yes |
| <a name="input_pricing_plan"></a> [pricing\_plan](#input\_pricing\_plan) | The pricing plan for the master instance. | `string` | `"PER_USE"` | no |
| <a name="input_project_id"></a> [project\_id](#input\_project\_id) | The project ID to manage the Cloud SQL resources | `string` | n/a | yes |
| <a name="input_random_instance_name"></a> [random\_instance\_name](#input\_random\_instance\_name) | Sets random suffix at the end of the Cloud SQL resource name | `bool` | `false` | no |
| <a name="input_read_replica_deletion_protection"></a> [read\_replica\_deletion\_protection](#input\_read\_replica\_deletion\_protection) | Used to block Terraform from deleting replica SQL Instances. | `bool` | `false` | no |
| <a name="input_read_replica_name_suffix"></a> [read\_replica\_name\_suffix](#input\_read\_replica\_name\_suffix) | The optional suffix to add to the read instance name | `string` | `""` | no |
| <a name="input_read_replicas"></a> [read\_replicas](#input\_read\_replicas) | List of read replicas to create<br>  {<br>    name            = string<br>    tier            = string<br>    zone            = string<br>    disk\_type       = string<br>    disk\_autoresize = bool<br>    disk\_size       = string<br>    user\_labels     = map(string)<br>    database\_flags = list(object({<br>      name  = string<br>      value = string<br>    }))<br>    ip\_configuration = object({<br>      authorized\_networks = list(map(string))<br>      ipv4\_enabled        = bool<br>      private\_network     = string<br>      require\_ssl         = bool<br>    })<br>  } | `list(any)` | `[]` | no |
| <a name="input_region"></a> [region](#input\_region) | The region of the Cloud SQL resources | `string` | `"us-central1"` | no |
| <a name="input_secondary_zone"></a> [secondary\_zone](#input\_secondary\_zone) | The secondary\_zone for the master instance, it should not include what's in the zone variable: `us-central1-b`, `us-east1-c`. | `string` | n/a | yes |
| <a name="input_tier"></a> [tier](#input\_tier) | The tier for the master instance. | `string` | `"db-f1-micro"` | no |
| <a name="input_update_timeout"></a> [update\_timeout](#input\_update\_timeout) | The optional timout that is applied to limit long database updates. | `string` | `"15m"` | no |
| <a name="input_user_labels"></a> [user\_labels](#input\_user\_labels) | The key/value labels for the master instances. | `map(string)` | `{}` | no |
| <a name="input_user_name"></a> [user\_name](#input\_user\_name) | The name of the default user | `string` | `"default"` | no |
| <a name="input_user_password"></a> [user\_password](#input\_user\_password) | The password for the default user. If not set, a random one will be generated and available in the generated\_user\_password output variable. | `string` | `""` | no |
| <a name="input_zone"></a> [zone](#input\_zone) | The zone for the master instance, it should be something like: `us-central1-a`, `us-east1-c`. | `string` | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_generated_user_password"></a> [generated\_user\_password](#output\_generated\_user\_password) | The auto generated default user password if not input password was provided |
| <a name="output_instance_connection_name"></a> [instance\_connection\_name](#output\_instance\_connection\_name) | The connection name of the master instance to be used in connection strings |
| <a name="output_instance_first_ip_address"></a> [instance\_first\_ip\_address](#output\_instance\_first\_ip\_address) | The first IPv4 address of the addresses assigned. |
| <a name="output_instance_ip_address"></a> [instance\_ip\_address](#output\_instance\_ip\_address) | The IPv4 address assigned for the master instance |
| <a name="output_instance_name"></a> [instance\_name](#output\_instance\_name) | The instance name for the master instance |
| <a name="output_instance_self_link"></a> [instance\_self\_link](#output\_instance\_self\_link) | The URI of the master instance |
| <a name="output_instance_server_ca_cert"></a> [instance\_server\_ca\_cert](#output\_instance\_server\_ca\_cert) | The CA certificate information used to connect to the SQL instance via SSL |
| <a name="output_instance_service_account_email_address"></a> [instance\_service\_account\_email\_address](#output\_instance\_service\_account\_email\_address) | The service account email address assigned to the master instance |
| <a name="output_instances"></a> [instances](#output\_instances) | A list of all `google_sql_database_instance` resources we've created |
| <a name="output_primary"></a> [primary](#output\_primary) | The `google_sql_database_instance` resource representing the primary instance |
| <a name="output_private_ip_address"></a> [private\_ip\_address](#output\_private\_ip\_address) | The first private (PRIVATE) IPv4 address assigned for the master instance |
| <a name="output_public_ip_address"></a> [public\_ip\_address](#output\_public\_ip\_address) | The first public (PRIMARY) IPv4 address assigned for the master instance |
| <a name="output_read_replica_instance_names"></a> [read\_replica\_instance\_names](#output\_read\_replica\_instance\_names) | The instance names for the read replica instances |
| <a name="output_replicas"></a> [replicas](#output\_replicas) | A list of `google_sql_database_instance` resources representing the replicas |
| <a name="output_replicas_instance_connection_names"></a> [replicas\_instance\_connection\_names](#output\_replicas\_instance\_connection\_names) | The connection names of the replica instances to be used in connection strings |
| <a name="output_replicas_instance_first_ip_addresses"></a> [replicas\_instance\_first\_ip\_addresses](#output\_replicas\_instance\_first\_ip\_addresses) | The first IPv4 addresses of the addresses assigned for the replica instances |
| <a name="output_replicas_instance_first_public_ip_addresses"></a> [replicas\_instance\_first\_public\_ip\_addresses](#output\_replicas\_instance\_first\_public\_ip\_addresses) | The first IPv4 addresses of the addresses assigned for the replica instances |
| <a name="output_replicas_instance_self_links"></a> [replicas\_instance\_self\_links](#output\_replicas\_instance\_self\_links) | The URIs of the replica instances |
| <a name="output_replicas_instance_server_ca_certs"></a> [replicas\_instance\_server\_ca\_certs](#output\_replicas\_instance\_server\_ca\_certs) | The CA certificates information used to connect to the replica instances via SSL |
| <a name="output_replicas_instance_service_account_email_addresses"></a> [replicas\_instance\_service\_account\_email\_addresses](#output\_replicas\_instance\_service\_account\_email\_addresses) | The service account email addresses assigned to the replica instances |
<!-- END_TF_DOCS -->