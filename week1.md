# Week 1

Install PowerShell Core:

```ps1
Install-Module -Name Az -AllowClobber
```

Register our account against the Policy Inisght provider: https://docs.microsoft.com/en-us/azure/governance/policy/assign-policy-powershell - this allows me to run policy commands withing powershell.

Sample policy: https://github.com/Azure/azure-policy/tree/master/samples/built-in-policy/apply-default-tag-value

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
