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