# Week 1

## Day 1

- Microsoft Learn
- Micrososft Hands.on-labs (don't think that's still a thing)
- docs.microsoft.com/en-us/azure

Azure Core Services

1. VMs: availability sets, load balancers, virtual networks
2. AppServices: fully managed servers (you cannot do anything, no ability to RDP), web apps
3. Storage: 3 copies of the files in the region

Accessing Az resources: Portal, Powershell, CLI/Bash

Scripting *_IS_* tested: useful for automation, source control (with GitHub)

PWSH vs CLI

### CLI

Azure CLI reference. It always starts with `az`, followed by a resource + a verb

```ps1
az vm list
az vm create
az vm delete

az keyvault list|create|delete
az network vnet subnet list|create|delete
```

### PWSH

Azure PowerShell reference

```ps1
Get-AzVM
New-AzVM
Remove-AzVM

Connect-AzAccount
```

### Install PowerShell Core

```ps1
Install-Module -Name Az -AllowClobber
```

**AzureRM on a Pwsh cmdlet turns into > _Az_**

### Manage Az Subscriptions

All services > Subscriptions (Access Control (IAM))

To add a new role: Add damin account > owner; If I'm standing at the subscription lvl and I'm adding a new Owner, it would have owner access, to the entire subscription.

Recommended way of managing costs:

1. Break down projects into resource groups
2. Tags --> makes billing easier
3. Create multiple subscriptions

Policies: subscription/scope or tagging related policies

### Create and assign policies using TUI

First, register our account against the Policy Inisght provider: <https://docs.microsoft.com/en-us/azure/governance/policy/assign-policy-powershell> - this allows me to run policy commands withing powershell.

Use GitHub policies - Sample policy: <https://github.com/Azure/azure-policy/tree/master/samples/built-in-policy/apply-default-tag-value>

```ps1

# Creates a new policy called 'apply-default-tag-value'

$definition = New-AzPolicyDefinition -Name "apply-default-tag-value" -DisplayName "Append tag and its default value" -description "Appends the specified tag and value when any resource which is missing this tag is created or updated. Does not modify the tags of resources created before this policy was applied until those resources are changed. Does not apply to resource groups. New 'modify' effect policies are available that support remediation of tags on existing resources (see https://aka.ms/modifydoc)." -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/apply-default-tag-value/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/apply-default-tag-value/azurepolicy.parameters.json' -Mode Indexed

# shows what has been created

$definition

# How to apply the definition to a resource

$assignment = New-AzPolicyAssignment -Name <assignmentname> -Scope <scope>  -tagName <tagName> -tagValue <tagValue> -PolicyDefinition $definition

$assignment
```

Assign policy deffinition

```ps1
$assignment = New-AzPolicyAssignment -Name "testingJunio" -Scope "/subscription/e724faf3-5a4b-42ce-812f-9454ffc0577d/" -tagName "Billy" -tagValue "Joel" -PolicyDefinition $definition
```

Policies can be assigned at the subscription level or ar the resource group level.

### Policies

Policies are useful to fulfill standards. AZ **policy** service. We can define the policy scope (subscription/rgs); exclusion; policies (Built-in, also in GH/custom).

- Auditting: copliance issue; if the policy is not met, alert
- not allowing
- enforments: tags, OS upgrades, password complexity...

## Day 2

Diagnostics can be done at the **resource group lvl**.

Azure monitor: all monitor activities en Az. Central monitor for diagnostics.

- Diagnostics settings shows all active monitors. Diagnostic for VMs --> if you don't enable diagnostics, you won't get any; you **do** need a **storage account** (additional costs) de 5GB.

VM > Diagnostic settings > Agents (these agents _CAN_ be deinstalled)

Performance counters: every 60" by default; it is customizable.

- Logs = windows event logs
- Crash dumps: disabled by default

**Baseline** for resources: recovering method besides VM bakcup. Resources stored as pwsh/cli script or ARM template (in GitHub, for instance). Modify the template and _redeploy_ the template.

RG > top right **Deployments** > click deployment options > to the right

Always save two files:

1. template
2. also store the parameters

This can be added to the library > name > description

Automatic script > generales current state of the resource.

All services > templates saved

Alerts and metrics: rg > alert (add criteria) > new alert rule

1. define alert condition
2. define alert details
3. define action group

Metrics menu is bellow alerts. Add chart / add metric. Reports on the fly; reports to be permanent need to add them to a dashboard. Different resources can have different events that can be measured.

Action groups: alerts > manage action groups. Multiple actions accur when that occurs: function (how to create a function call, automations that happen), webhook (facebook), ITSM...

Costs: go into the subscriptions > **lost analysis**, see where. Cost management + billing --> orgnatization lvl reporting.

Log analytics: log workspace for log analytics. Storage account of containers to store the logs. Similar to splunk. It allows to `run queries`. Alerts can be set with a query. **Data sources --> select azure resources**.

Resource Groups: organizational structure. Resources are into the rg. Name has to be unique within the region.

Locks on resources to make change or delete:

- read-only
- delete

rg policy: rg is a scope - assignments > assign. Restrictions at the rg lvl, from wihing the RG.

Moving resources to:

- another rg
- another subscription

Moving managed disks needs to be registered as a provider for that feature:

```ps1
az feature register --namespace Microsoft.compute --ManagedResourcesMove

# show status of registering process

az feature show --namespace Microsoft.compute --ManagedResourcesMove

# Re-register compute service (recommended)

az provider register --namespace Microsoft.Compute
```

Storage: Premium SSD, Strandard HDD

- Blob Storage:
  - can be public
- General purpose:

Replication:

- ZRS (Zonally Redundant Storage)
- LRS (locally): your data + 2 additional copies within the region. Takes advantage of availability zones.
- GRS (globally): LRS + 3 additional copies in other regions of the world.

Access tier:

- Hot: inmediatelly available, lowest latency time in retrieving
- Cool: cheaper to store long terms, expense in retrieval and latency

Virtual networks (service end point):

- by default it is generally accessible (**All networks**); you get a publicly accessible URL and as long as you have the access keys you'll be able to read or write. Secure Access Signatures (SAS) to give limited access to people.
- Only **Selected networks**: turns off All networks; public access. Even if you have SAS, if you are not within a specific network, you won't be able to access the contents of the storage account. This option is more secure.

Within the storage account > Firewall and virtual networks (prevent access to public internet): change settings All networks <--> selected networks.

- IP firewall: we can add IP ranges
- Exceptions: store stuff for logging purposes, log analytics...

How to create a file share: <https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-create-file-share?tabs=azure-portal>

troubleshott file sync: <https://docs.microsoft.com/en-us/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal>

## Day 3

Turn off execution policies:

```ps1
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass

# Make deployment by running "deploy.ps1" file

./deploy.ps1 -subscriptionId <subscription Id> -resourceGroupName <it's going to be created if it doesn't exist> -deploymentName <XXX>
```

<https://github.com/olohmann/azure-hands-on-labs>
<https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator>
<https://github.com/MicrosoftLearning/AZ-103-MicrosoftAzureAdministrator>
