---
title: Erstellen von berechtigten Autorisierungen
description: Beim Onboarding von Kunden in Azure Lighthouse können Sie es Benutzern in Ihrem Verwaltungsmandanten ermöglichen, ihre Rolle JIT-basiert (Just-In-Time) zu erhöhen.
ms.date: 05/25/2021
ms.topic: how-to
ms.openlocfilehash: f220574a2fb84fcf4e7a6e4933bcfbf61d882091
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110385982"
---
# <a name="create-eligible-authorizations"></a>Erstellen von berechtigten Autorisierungen

Beim Onboarding von Kunden in Azure Lighthouse erstellen Sie Autorisierungen, um Benutzern in Ihrem Verwaltungsmandanten integrierte Azure-Rollen zuzuweisen. Sie können auch berechtigte Autorisierungen erstellen, die [Azure Active Directory (Azure AD) Privileged Identity Management (PIM)](/azure/active-directory/privileged-identity-management/pim-configure) nutzen, damit Benutzer in Ihrem Verwaltungsmandanten ihre Rolle vorübergehend erhöhen können. So können Sie zusätzliche Berechtigungen auf JIT-Basis gewähren, damit Benutzer nur für einen festgelegten Zeitraum über diese Berechtigungen verfügen.

Durch das Erstellen von berechtigten Autorisierungen können Sie die Anzahl von permanenten Benutzerzuweisungen zu privilegierten Rollen verringern. Dadurch werden Sicherheitsrisiken im Zusammenhang mit privilegiertem Zugriff durch Benutzer in Ihrem Mandanten minimiert.

In diesem Thema wird die Funktionsweise von berechtigten Autorisierungen erläutert, und Sie erfahren, wie Sie diese Autorisierungen beim [Onboarding eines Kunden in Azure Lighthouse](onboard-customer.md) erstellen.

> [!IMPORTANT]
> Berechtigte Autorisierungen befinden sich derzeit in der öffentlichen Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="license-requirements"></a>Lizenzanforderungen

Zum Erstellen berechtigter Autorisierungen ist eine Enterprise Mobility + Security E5-Lizenz (EMS E5) oder Azure AD Premium P2-Lizenz erforderlich. Um die richtige Lizenz für Ihre Anforderungen zu ermitteln, lesen Sie [Vergleich: Allgemein verfügbare Features der Editionen Free, Basic und Premium](https://azure.microsoft.com/pricing/details/active-directory/).

Nicht der Kundenmandant, sondern der Verwaltungsmandant muss über die EMS E5- oder Azure AD Premium P2-Lizenz verfügen.

Zusätzliche Kosten, die mit einer berechtigten Rolle verbunden sind, fallen nur während des Zeitraums an, in dem der Benutzer seinen Zugriff auf diese Rolle erhöht.

Weitere Informationen zu Lizenzen für Benutzer finden Sie unter [Lizenzanforderungen für die Verwendung von PIM](/azure/active-directory/privileged-identity-management/subscription-requirements).

## <a name="how-eligible-authorizations-work"></a>Funktionsweise von berechtigten Autorisierungen

Eine berechtigte Autorisierung definiert eine Rollenzuweisung, bei der der Benutzer die Rolle aktivieren muss, um privilegierte Aufgaben auszuführen. Wenn der Benutzer die berechtigte Rolle aktiviert, erhält er für den festgelegten Zeitraum den vollständigen Zugriff, der durch diese Rolle erteilt wird.

Benutzer im Kundenmandanten können vor dem Onboarding alle Rollenzuweisungen überprüfen. Dies umfasst auch die Rollenzuweisungen in berechtigten Autorisierungen.

Sobald ein Benutzer eine berechtigte Rolle erfolgreich aktiviert hat, verfügt er für einen vorkonfigurierten Zeitraum zusätzlich zu seinen dauerhaften Rollenzuweisungen für diesen Bereich über diese erhöhte Rolle im delegierten Bereich.

Administratoren im Verwaltungsmandanten können alle Privileged Identity Management-Aktivitäten im Überwachungsprotokoll im Verwaltungsmandanten überprüfen. Kunden können diese Aktionen im Azure-Aktivitätsprotokoll für das delegierte Abonnement anzeigen.

Beim Erstellen einer berechtigten Autorisierung definieren Sie drei Elemente: den Benutzer, die Rolle und die Zugriffsrichtlinie.

- Der **Benutzer** kann entweder ein Einzelbenutzer im Verwaltungsmandanten oder eine Azure AD-Gruppe in diesem Mandanten sein. Wenn eine Gruppe definiert ist, kann jedes Mitglied dieser Gruppe seinen individuellen Zugriff gemäß der Zugriffsrichtlinie auf die Rolle erhöhen. Berechtigte Autorisierungen können nicht mit Dienstprinzipalen verwendet werden.
- Die **Rolle** kann eine beliebige integrierte Azure-Rolle sein, die für die delegierte Azure-Ressourcenverwaltung unterstützt wird (mit Ausnahme des Benutzerzugriffsadministrators).
- Die **Zugriffsrichtlinie** definiert die Anforderungen der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) und die Dauer, für die die Rolle für einen Benutzer aktiviert wird. Der maximale Zeitraum, der für eine Rolle festgelegt werden kann, ist 8 Stunden.

Nachfolgend finden Sie weitere Informationen zu diesen Elementen sowie dazu, wie Sie sie definieren.

## <a name="create-eligible-authorizations-using-azure-resource-manager-templates"></a>Erstellen von berechtigten Autorisierungen mithilfe von Azure Resource Manager-Vorlagen

Für das Onboarding Ihres Kunden in Azure Lighthouse verwenden Sie eine [Azure Resource Manager-Vorlage mit einer entsprechenden Parameterdatei](onboard-customer.md#create-an-azure-resource-manager-template), die Sie anpassen. Welche Vorlage Sie auswählen, hängt davon ab, ob Sie das Onboarding für ein vollständiges Abonnement, eine Ressourcengruppe oder mehrere Ressourcengruppen innerhalb eines Abonnements durchführen.

> [!NOTE]
> Das Onboarding kann auch mithilfe von Angeboten für verwaltete Dienste im Azure Marketplace durchgeführt werden. Berechtigte Autorisierungen werden bei diesen Angeboten jedoch derzeit nicht unterstützt.

Um beim Onboarding eines Kunden berechtigte Autorisierungen hinzuzufügen, verwenden Sie eine Vorlage aus dem [delegated-resource-management-eligible-authorizations-Abschnitt unseres Beispiel-Repositorys](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/delegated-resource-management-eligible-authorizations).


|So führen Sie das Onboarding durch (mit berechtigten Autorisierungen)  |Verwenden Sie diese Azure Resource Manager-Vorlage:  |Ändern Sie außerdem diese Parameterdatei: |
|---------|---------|---------|
|Subscription   |[subscription.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/delegated-resource-management-eligible-authorizations/subscription/subscription.json)  |[subscription.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/delegated-resource-management-eligible-authorizations/subscription/subscription.Parameters.json)    |
|Resource group   |[rg.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/delegated-resource-management-eligible-authorizations/rg/rg.json)  |[rg.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/delegated-resource-management-eligible-authorizations/rg/rg.parameters.json)    |
|Mehrere Ressourcengruppen in einem Abonnement   |[multiple-rg.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/delegated-resource-management-eligible-authorizations/rg/multiple-rg.json)  |[multiple-rg.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/delegated-resource-management-eligible-authorizations/rg/multiple-rg.parameters.json)    |

Nachfolgend ist die Vorlage **subscription.json** gezeigt, die für das Onboarding eines Abonnements mit berechtigten Autorisierungen verwendet werden kann. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "mspOfferName": {
            "type": "string",
            "metadata": {
                "description": "Specify a unique name for your offer"
            },
            "defaultValue": "<to be filled out by MSP> Specify a title for your offer"
        },
        "mspOfferDescription": {
            "type": "string",
            "metadata": {
                "description": "Name of the Managed Service Provider offering"
            },
            "defaultValue": "<to be filled out by MSP> Provide a brief description of your offer"
        },
        "managedByTenantId": {
            "type": "string",
            "metadata": {
                "description": "Specify the tenant id of the Managed Service Provider"
            },
            "defaultValue": "<to be filled out by MSP> Provide your tenant id"
        },
        "authorizations": {
            "type": "array",
            "metadata": {
                "description": "Specify an array of objects, containing tuples of Azure Active Directory principalId, a Azure roleDefinitionId, and an optional principalIdDisplayName. The roleDefinition specified is granted to the principalId in the provider's Active Directory and the principalIdDisplayName is visible to customers."
            },
            "defaultValue": [
                { 
                    "principalId": "00000000-0000-0000-0000-000000000000", 
                    "roleDefinitionId": "acdd72a7-3385-48ef-bd42-f606fba81ae7",
                    "principalIdDisplayName": "PIM_Group" 
                }, 
                { 
                    "principalId": "00000000-0000-0000-0000-000000000000", 
                    "roleDefinitionId": "91c1777a-f3dc-4fae-b103-61d183457e46",
                    "principalIdDisplayName": "PIM_Group" 
                }   
            ]
        }, 
        "eligibleAuthorizations": { 
            "type": "array", 
            "metadata": { 
                "description": "Provide the authorizations that will have just-in-time role assignments on customer environments" 
            },
           "defaultValue": [ 
                { 
                        "justInTimeAccessPolicy": { 
                            "multiFactorAuthProvider": "Azure", 
                            "maximumActivationDuration": "PT8H" 
                        },
                        "principalId": "00000000-0000-0000-0000-000000000000", 
                        "principalIdDisplayName": "PIM_Group",
                        "roleDefinitionId": "36243c78-bf99-498c-9df9-86d9f8d28608" 
                        
                }                    
            ]    

        }                 
    },
    "variables": {
        "mspRegistrationName": "[guid(parameters('mspOfferName'))]",
        "mspAssignmentName": "[guid(parameters('mspOfferName'))]"
    },
    "resources": [
        {
            "type": "Microsoft.ManagedServices/registrationDefinitions",
            "apiVersion": "2020-02-01-preview",
            "name": "[variables('mspRegistrationName')]",
            "properties": {
                "registrationDefinitionName": "[parameters('mspOfferName')]",
                "description": "[parameters('mspOfferDescription')]",
                "managedByTenantId": "[parameters('managedByTenantId')]",
                "authorizations": "[parameters('authorizations')]", 
                "eligibleAuthorizations": "[parameters('eligibleAuthorizations')]" 
            }
        },
        {
            "type": "Microsoft.ManagedServices/registrationAssignments",
            "apiVersion": "2020-02-01-preview",
            "name": "[variables('mspAssignmentName')]",
            "dependsOn": [
                "[resourceId('Microsoft.ManagedServices/registrationDefinitions/', variables('mspRegistrationName'))]"
            ],
            "properties": {
                "registrationDefinitionId": "[resourceId('Microsoft.ManagedServices/registrationDefinitions/', variables('mspRegistrationName'))]"
            }
        }
    ],
    
    "outputs": {
        "mspOfferName": {
            "type": "string",
            "value": "[concat('Managed by', ' ', parameters('mspOfferName'))]"
        },
        "authorizations": {
            "type": "array",
            "value": "[parameters('authorizations')]"
        }, 
        "eligibleAuthorizations": { 
            "type": "array", 
            "value": "[parameters('eligibleAuthorizations')]" 

        } 
    }
}
```

### <a name="define-eligible-authorizations-in-your-parameters-file"></a>Definieren von berechtigten Autorisierungen in der Parameterdatei

Die Beispielvorlage [subscription.Parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/delegated-resource-management-eligible-authorizations/subscription/subscription.Parameters.json) kann verwendet werden, um beim Onboarding eines Abonnements Autorisierungen (einschließlich berechtigte Autorisierungen) zu definieren.

Alle berechtigten Autorisierungen müssen im `eligibleAuthorizations`-Parameter definiert werden. In diesem Beispiel wird eine berechtigte Autorisierung hinzugefügt.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "mspOfferName": {
            "value": "Relecloud Managed Services"
        },
        "mspOfferDescription": {
            "value": "Relecloud Managed Services"
        },
        "managedByTenantId": {
            "value": "<insert the managing tenant id>"
        },
        "authorizations": {
            "value": [
                { 
                    "principalId": "00000000-0000-0000-0000-000000000000",
                    "roleDefinitionId": "acdd72a7-3385-48ef-bd42-f606fba81ae7",
                    "principalIdDisplayName": "PIM group"
                }
            ]
        }, 
        "eligibleAuthorizations":{
            "value": [
                {
                        "justInTimeAccessPolicy": {
                            "multiFactorAuthProvider": "Azure",
                            "maximumActivationDuration": "PT8H"
                        },
                        "principalId": "00000000-0000-0000-0000-000000000000", 
                        "principalIdDisplayName": "Tier 2 Support",
                        "roleDefinitionId": "b24988ac-6180-42a0-ab88-20f7382dd24c"

                }
            ]
        }
    }
}
```

Innerhalb des `eligibleAuthorizations`-Parameters gibt `principalId` die ID für den Azure AD-Benutzer oder die Azure AD-Gruppe an, für den bzw. die diese berechtigte Autorisierung gilt. Verwenden Sie keine Dienstprinzipalkonto-ID. Derzeit ist es nicht möglich, die Zugriffsrechte eines Dienstprinzipalkontos zu erhöhen und eine berechtigte Rolle für dieses Konto zu verwenden.

> [!IMPORTANT]
> Fügen Sie dieselbe `principalId` im Abschnitt `authorizations` Ihrer Vorlage mit einer anderen Rolle hinzu. Dies kann z. B. die Rolle „Leser“ oder eine andere integrierte Azure-Rolle mit Lesezugriff sein. Anderenfalls ist der Benutzer nicht in der Lage, seine Rolle über das Azure-Portal zu erhöhen.

`roleDefinitionId` enthält die Rollendefinitions-ID für eine [integrierte Azure-Rolle](/azure/role-based-access-control/built-in-roles), die der Benutzer auf Just-In-Time-Basis nutzen kann.

Über `justInTimeAccessPolicy` werden zwei Elemente angegeben:

- `multiFactorAuthProvider` kann entweder auf **Azure** festgelegt werden, um die mehrstufige Authentifizierung in Azure zu verwenden, oder auf **None**, wenn keine mehrstufige Authentifizierung erforderlich ist.
- `maximumActivationDuration` legt die Dauer fest, für die der Benutzer über die berechtigte Rolle verfügt. Dieser Wert muss im ISO 8601-Format angegeben werden. Der Mindestwert ist PT30M (30 Minuten), der Höchstwert lautet PT8H (8 Stunden).

> [!NOTE]
> Hinweis: Der Just-In-Time-Zugriff gilt nicht für `delegatedRoleDefinitionIds`, die Benutzerzugriffsadministratoren [verwalteten Identitäten zuweisen können](deploy-policy-remediation.md). Diese Rollenzuweisungen können nicht als berechtigte Autorisierungen erstellt werden. Auch für die Rolle „Benutzerzugriffsadministrator“ selbst kann keine berechtigte Autorisierung erstellt werden.

## <a name="elevation-process-for-users"></a>Rechteerweiterung für Benutzer

Nachdem Sie das Onboarding für einen Kunden in Azure Lighthouse durchgeführt haben, sind alle berechtigten Rollen, die Sie hinzugefügt haben, für den angegebenen Benutzer (oder für Benutzer in bestimmten Gruppen) verfügbar.

Die Benutzer können ihre Zugriffsrechte jederzeit erhöhen, indem sie im Azure-Portal die Seite **Meine Kunden** aufrufen, eine Delegierung auswählen und dann auf **Berechtigte Rollen verwalten** klicken. Anschließend führen sie die [Schritte zum Aktivieren der Rolle](/azure/active-directory/privileged-identity-management/pim-how-to-activate-role) in Azure AD Privileged Identity Management aus.

Nachdem die berechtigte Rolle aktiviert wurde, verfügt der Benutzer für die gesamte Dauer, die in der berechtigten Autorisierung festgelegt ist, über diese Rolle. Nach Ablauf dieses Zeitraums kann die Rolle erst wieder verwendet werden, wenn der Benutzer die Schritte zur Rechteerweiterung erneut ausführt, um seine Zugriffsrechte zu erhöhen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Onboarding von Kunden in Azure Lighthouse mithilfe von ARM-Vorlagen](onboard-customer.md).
- Erfahren Sie mehr über [Azure AD Privileged Identity Management](/azure/active-directory/privileged-identity-management/pim-configure).
- Erfahren Sie mehr über [Mandanten, Benutzer und Rollen in Azure Lighthouse](../concepts/tenants-users-roles.md).
