# Preguntas

1. cómo se hace la conexión de una VPN con Azure?

## Repasar

- Query search: <https://docs.microsoft.com/en-us/azure/azure-monitor/log-query/search-queries> <https://docs.microsoft.com/en-us/azure/azure-monitor/log-query/get-started-portal>
- RBAC: <https://docs.microsoft.com/en-us/azure/role-based-access-control/custom-roles> <https://docs.microsoft.com/en-us/azure/role-based-access-control/resource-provider-operations#microsoftresources>
- AZ AD: <https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal> <https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/license-users-groups>
- Alerts: <https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-rate-limiting>
- DNS: <https://docs.microsoft.com/en-us/azure/dns/dns-import-export>
- Policies: <https://docs.microsoft.com/en-us/azure/governance/policy/concepts/definition-structure>
- BUR: <https://docs.microsoft.com/en-us/azure/backup/backup-azure-files> <https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-first-look-arm#defining-a-backup-policy> <https://blogs.microsoft.com/firehose/2015/02/16/february-update-to-azure-backup-includes-data-retention-up-to-99-years-offline-backup-and-more/> <https://docs.microsoft.com/en-us/azure/backup/backup-configure-vault>
- Storage Explorer: <https://docs.microsoft.com/en-us/azure/storage/common/storage-dotnet-shared-access-signature-part-1> <https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows>
- Storage account: <https://docs.microsoft.com/en-us/azure/storage/common/storage-account-options> <https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal>
- File share: <https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-create-file-share>
- <https://docs.microsoft.com/en-us/azure/storage/common/storage-dotnet-shared-access-signature-part-1> <https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows>
- DataSet and DriveSet: <https://docs.microsoft.com/en-us/previous-versions/azure/storage/common/storage-import-export-tool-preparing-hard-drives-import?toc=%2Fazure%2Fstorage%2Ffiles%2Ftoc.json>
- More exam questions: <https://books.google.com.ar/books?id=fivbDwAAQBAJ&pg=PT40&lpg=PT40&dq=azure+%22file+explorer%22+vs+%22storage+explorer%22&source=bl&ots=1NXD3KbHiT&sig=ACfU3U3oy1uwWIpioUuTwnyfVY4iffE6BA&hl=es-419&sa=X&ved=2ahUKEwiF4uups5DqAhVXH7kGHYe8CAEQ6AEwEXoECAoQAQ#v=onepage&q=azure%20%22file%20explorer%22%20vs%20%22storage%20explorer%22&f=false>
- Storage Explorer: <https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/move-data-to-azure-blob-using-azure-storage-explorer>

| | Standard Load Balancer | Basic Load Balancer |
| --- | --- | --- |
| [Backend pool size](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer) | Supports up to 1000 instances. | Supports up to 300 instances. |
| Backend pool endpoints | Any virtual machines or virtual machine scale sets in a single virtual network. | Virtual machines in a single availability set or virtual machine scale set. |
| [Health probes](./load-balancer-custom-probe-overview.md#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [Health probe down behavior](./load-balancer-custom-probe-overview.md#probedown) | TCP connections stay alive on an instance probe down __and__ on all probes down. | TCP connections stay alive on an instance probe down. All TCP connections terminate when all probes are down. |
| Availability Zones | Zone-redundant and zonal frontends for inbound and outbound traffic. | Not available |
| Diagnostics | [Azure Monitor multi-dimensional metrics](./load-balancer-standard-diagnostics.md) | [Azure Monitor logs](./load-balancer-monitor-log.md) |
| HA Ports | [Available for Internal Load Balancer](./load-balancer-ha-ports-overview.md) | Not available |
| Secure by default | Closed to inbound flows unless allowed by a network security group. Please note that internal traffic from the VNet to the internal load balancer is allowed. | Open by default. Network security group optional. |
| Outbound Rules | [Declarative outbound NAT configuration](./load-balancer-outbound-rules-overview.md) | Not available |
| TCP Reset on Idle | [Available on any rule](./load-balancer-tcp-reset.md) | Not available |
| [Multiple front ends](./load-balancer-multivip-overview.md) | Inbound and [outbound](./load-balancer-outbound-connections.md) | Inbound only |
| Management Operations | Most operations < 30 seconds | 60-90+ seconds typical |
| SLA | [99.99%](https://azure.microsoft.com/support/legal/sla/load-balancer/v1_0/) | Not available | 