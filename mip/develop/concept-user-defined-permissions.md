---
title: Concepts - The core concepts in the MIP SDK - User-defined permissions. 
description: This article will help you understand the core SDK concept called user-defined permissions.
author: tommoser
ms.service: information-protection
ms.topic: conceptual
ms.date: 02/02/2021
ms.author: tommos
---

# Microsoft Information Protection SDK - User-defined Permissions

The Microsoft Information Protection SDK supports two primary types of label-driven permissions: Template-based and user-defined.

- **Template-based permissions:** These rights are defined by the label administrator in Security and Compliance Center. These labels are centrally managed, and changes in the configuration will impact users who already have copies of the files. For example, if the administrator removes a user from the list of authorized users, that user will no longer have access to the protected data the next time they attempt to fetch a license.

- **User-defined permissions**: These rights are defined **at the time of labeling** by the end user or application. Permissions are passed to the MIP SDK in the form of a collection of user-to-roles or users-to-rights mappings. These rights are written in to the publishing license for the protected document and, unlike template-based permissions, cannot be centrally managed or modified after sharing without direct access and modification of the document.

## Users, Rights, and Roles

Because it's expected that rights will be defined by the user at the time of labeling, your application must provide an interface to allow the user or service to provide input on the email addresses and rights or roles that the user will have. This configuration is accomplished by passing in a collection of `UserRoles` or `UserRights` objects that specifically define who should have what level of access to the documents.

```csharp
// Create a List<string> of the first set of permissions. 
List<string> users = new List<string>()
{
    "alice@contoso.com",
    "bob@contoso.com"
};

// Create a List<string> of the Rights the above users should have. 
List<string> rights = new List<string>()
{
    Rights.View,
    Rights.Edit                
};

// Create a UserRights object containing the defined users and rights.
UserRights userRights = new UserRights(users, rights);

// Add them to a new List<UserRights>
List<UserRights> userRightsList = new List<UserRights>()
{
    userRights
};
```

The result is that you'll have a `List<UserRights>` collection specifying that both Alice and Bob have VIEW and EDIT on the protected file. To add more users with a *different* set of permissions, you'd repeat the process to create a second `UserRights` object, passing in the new users and permissions, then add to the `List<UserRights>` collection by calling `userRightsList.Add(userRights2)`.

This pattern is also true for `UserRoles` and can be implemented simply by replacing **rights** with **roles** and create a `List<UserRoles>` collection.

### Apply Protection

Setting protection can be achieved by creating a `ProtectionDescriptor` from the `List<UserRights>` or `List<UserRoles>` object, then passing that to `FileHandler.SetProtection()`. Finally, commit the change to the file to write a new file. 

### When to apply protection to files

When you set a label via `FileHandler.SetLabel()` MIP SDK has all it needs to take action and apply any protection. When the label is configured for user-defined permissions (UDP), your application has no way to know ahead of time that the label is a UDP label. The MIP SDK surfaces this information by throwing an exception of the type `Microsoft.InformationProtection.Exceptions.AdhocProtectionRequiredException`. Your `FileHandler` code should catch this exception, then trigger your user or service interface to define the custom permissions. Once complete, you'll be able to set protection. The following example shows the end-to-end pattern, but does assume you've already implemented a function to build the `List<UserRights>` object.

```csharp
try
{
    // Attempt to set the label. If it's a UDP label, this will throw. 
    handler.SetLabel(engine.GetLabelById(options.LabelId), labelingOptions, new ProtectionSettings());
}

catch (Microsoft.InformationProtection.Exceptions.AdhocProtectionRequiredException)
{
    // Assumes you've create a function that returns the List<UserRights> as previously detailed. 
    List<UserRights> userRightsList = GetUserRights();

    // Create a ProtectionDescriptor using the set of UserRights.
    ProtectionDescriptor protectionDescriptor = new ProtectionDescriptor(userRightsList);
    
    // Apply protection to the file using the new ProtectionDescriptor. 
    handler.SetProtection(protectionDescriptor, new ProtectionSettings());

    // Set the label. This will now succeed as protection has been defined. 
    handler.SetLabel(engine.GetLabelById(options.LabelId), labelingOptions, new ProtectionSettings());

    // Commit the change. 
    var result = Task.Run(async () => await handler.CommitAsync("myFileOutput.xlsx")).Result;
}
```

## Custom Protection

This process can also be used to set only protection by setting protection and skipping the `SetLabel()` step. If your application does not need to apply a label, the exception handler isn't required and protection can be set by following the `ProtectionDescriptor` -> `SetProtection()` -> `CommitAsync()` pattern.

## Next Steps

- [Review label encryption configuration in the Microsoft 365 documentation.](/microsoft-365/compliance/encryption-sensitivity-labels?view=o365-worldwide#understand-how-the-encryption-works)