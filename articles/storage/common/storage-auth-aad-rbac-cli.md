---
title: Use Azure CLI to manage Azure AD access rights to blob and queue data with RBAC - Azure Storage
description: Use Azure CLI to assign access to containers and queues with role-based access control (RBAC). Azure Storage supports built-in and custom RBAC roles for authentication via Azure AD.
services: storage
author: tamram

ms.service: storage
ms.topic: article
ms.date: 03/21/2019
ms.author: tamram
ms.subservice: common
---

# Grant access to Azure blob and queue data with RBAC using Azure CLI

Azure Active Directory (Azure AD) authorizes access rights to secured resources through [role-based access control (RBAC)](../../role-based-access-control/overview.md). Azure Storage defines a set of built-in RBAC roles that encompass common sets of permissions used to access blob or queue data. 

When an RBAC role is assigned to an Azure AD security principal, Azure grants access to those resources for that security principal. Access can be scoped to the level of the subscription, the resource group, the storage account, or an individual container or queue. An Azure AD security principal may be a user, a group, an application service principal, or a [managed identity for Azure resources](../../active-directory/managed-identities-azure-resources/overview.md).

This article describes how to use Azure CLI to list built-in RBAC roles and assign them to users. For more information about using Azure CLI, see [Azure Command-Line Interface (CLI)](https://docs.microsoft.com/cli/azure).

## RBAC roles for blobs and queues

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

## Determine resource scope 

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## List available RBAC roles

To list available built-in RBAC roles with Azure CLI, use the [az role definition list](/cli/azure/role/definition#az-role-definition-list) command:

```azurecli-interactive
az role definition list --out table
```

You'll see the built-in Azure Storage data roles listed, together with other built-in roles for Azure:

```Example
Storage Blob Data Contributor             Allows for read, write and delete access to Azure Storage blob containers and data
Storage Blob Data Owner                   Allows for full access to Azure Storage blob containers and data, including assigning POSIX access control.
Storage Blob Data Reader                  Allows for read access to Azure Storage blob containers and data
Storage Queue Data Contributor            Allows for read, write, and delete access to Azure Storage queues and queue messages
Storage Queue Data Message Processor      Allows for peek, receive, and delete access to Azure Storage queue messages
Storage Queue Data Message Sender         Allows for sending of Azure Storage queue messages
Storage Queue Data Reader                 Allows for read access to Azure Storage queues and queue messages
```

## Assign an RBAC role to a user

To assign an RBAC role to a user, use the [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create) command. The format of the command can differ based on the scope of the assignment. The following examples show how to assign a role to a user at various scopes.

### Container scope

To assign a role scoped to a container, specify a string containing the scope of the container for the `--scope` parameter. The scope for a container is in the form:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/<container-name>
```

The following example assigns the **Storage Blob Data Contributor** role to a user, scoped to a container named *sample-container*. Make sure to replace the sample values and the placeholder values in brackets with your own values: 

```azurecli-interactive
az role assignment create \
    --role "Storage Blob Data Contributor" \
    --assignee <email> \
    --scope "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/sample-container"
```

### Queue scope

To assign a role scoped to a queue, specify a string containing the scope of the queue for the `--scope` parameter. The scope for a queue is in the form:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/<queue-name>
```

The following example assigns the **Storage Queue Data Contributor** role to a user, scoped to a queue named *sample-queue*. Make sure to replace the sample values and the placeholder values in brackets with your own values: 

```azurecli-interactive
az role assignment create \
    --role "Storage Queue Data Contributor" \
    --assignee <email> \
    --scope "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/sample-queue"
```

### Storage account scope

To assign a role scoped to the storage account, specify the scope of the storage account resource for the `--scope` parameter. The scope for a storage account is in the form:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>
```

The following example shows how to assign the **Storage Blob Data Reader** role to a user at the level of the storage account. Make sure to replace the sample values with your own values: 

```azurecli-interactive
az role assignment create \
    --role "Storage Blob Data Reader" \
    --assignee <email> \
    --scope "/subscriptions/<subscription-id>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/storagesamples"
```

### Resource group scope

To assign a role scoped to the resource group, specify the resource group name or ID for the `--resource-group` parameter. The following example assigns the **Storage Queue Data Reader** role to a user at the level of the resource group. Make sure to replace the sample values and placeholder values in brackets with your own values: 

```azurecli-interactive
az role assignment create \
    --role "Storage Queue Data Reader" \
    --assignee <email> \
    --resource-group sample-resource-group
```

### Subscription scope

To assign a role scoped to the subscription, specify the scope for the subscription for the `--scope` parameter. The scope for a subscription is in the form:

```
/subscriptions/<subscription>
```

The following example shows how to assign the **Storage Blob Data Reader** role to a user at the level of the storage account. Make sure to replace the sample values with your own values: 

```azurecli-interactive
az role assignment create \
    --role "Storage Blob Data Reader" \
    --assignee <email> \
    --scope "/subscriptions/<subscription-id>"
```

## Next steps

- [Manage access to Azure resources using RBAC and Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md)
- [Grant access to Azure blob and queue data with RBAC using Azure PowerShell](storage-auth-aad-rbac-powershell.md)
- [Grant access to Azure blob and queue data with RBAC in the Azure portal](storage-auth-aad-rbac-portal.md)
