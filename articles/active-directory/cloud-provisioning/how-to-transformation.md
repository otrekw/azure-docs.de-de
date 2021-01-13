---
title: 'Azure AD Connect-Cloudbereitstellung: Transformationen'
description: In diesem Artikel wird beschrieben, wie Sie mithilfe von Transformationen die Standardattributzuordnungen ändern können.
author: billmath
ms.author: billmath
manager: davba
ms.date: 12/02/2019
ms.topic: how-to
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a15a25ce4fbe1a43c2a5065bf88f5c40c5865721
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85360689"
---
# <a name="transformations"></a>Transformationen

Mit einer Transformation können Sie das Standardverhalten der Synchronisierung eines Attributs mit Azure Active Directory (Azure AD) unter Verwendung der Cloudbereitstellung ändern.

Dazu müssen Sie das Schema bearbeiten und es dann über eine Webanforderung erneut übermitteln.

Weitere Informationen zu Cloudbereitstellungsattributen finden Sie unter [Grundlegendes zum Azure AD-Schema](concept-attributes.md).


## <a name="retrieve-the-schema"></a>Abrufen des Schemas
Um das Schema abzurufen, führen Sie die Schritte in [Anzeigen des Schemas](concept-attributes.md#view-the-schema) aus. 

## <a name="custom-attribute-mapping"></a>Benutzerdefinierte Attributzuordnung
Führen Sie die folgenden Schritte aus, um eine benutzerdefinierte Attributzuordnung hinzuzufügen.

1. Kopieren Sie das Schema in einen Text-Editor oder Code-Editor, z. B. [Visual Studio Code](https://code.visualstudio.com/).
1. Suchen Sie das Objekt, das Sie im Schema aktualisieren möchten.

   ![Das Objekt im Schema](media/how-to-transformation/transform1.png)</br>
1. Suchen Sie den Code für `ExtensionAttribute3` unter dem Benutzerobjekt.

    ```
                            {
                                "defaultValue": null,
                                "exportMissingReferences": false,
                                "flowBehavior": "FlowWhenChanged",
                                "flowType": "Always",
                                "matchingPriority": 0,
                                "targetAttributeName": "ExtensionAttribute3",
                                "source": {
                                    "expression": "Trim([extensionAttribute3])",
                                    "name": "Trim",
                                    "type": "Function",
                                    "parameters": [
                                        {
                                            "key": "source",
                                            "value": {
                                                "expression": "[extensionAttribute3]",
                                                "name": "extensionAttribute3",
                                                "type": "Attribute",
                                                "parameters": []
                                            }
                                        }
                                    ]
                                }
                            },
    ```
1. Bearbeiten Sie den Code so, dass das company-Attribut `ExtensionAttribute3` zugeordnet ist.

   ```
                                    {
                                        "defaultValue": null,
                                        "exportMissingReferences": false,
                                        "flowBehavior": "FlowWhenChanged",
                                        "flowType": "Always",
                                        "matchingPriority": 0,
                                        "targetAttributeName": "ExtensionAttribute3",
                                        "source": {
                                            "expression": "Trim([company])",
                                            "name": "Trim",
                                            "type": "Function",
                                            "parameters": [
                                                {
                                                    "key": "source",
                                                    "value": {
                                                        "expression": "[company]",
                                                        "name": "company",
                                                        "type": "Attribute",
                                                        "parameters": []
                                                    }
                                                }
                                            ]
                                        }
                                    },
   ```
 1. Kopieren Sie das Schema wieder zurück in den Graph-Tester, ändern Sie den **Anforderungstyp** in **PUT**, und wählen Sie **Abfrage ausführen** aus.

    ![Ausführen der Abfrage](media/how-to-transformation/transform2.png)

 1. Navigieren Sie im Azure-Portal zur Konfiguration der Cloudbereitstellung, und wählen Sie **Bereitstellung erneut starten** aus.

    ![Erneutes Starten der Bereitstellung](media/how-to-transformation/transform3.png)

 1. Überprüfen Sie nach einiger Zeit, ob die Attribute gefüllt werden, indem Sie die folgende Abfrage im Graph-Tester ausführen: `https://graph.microsoft.com/beta/users/{Azure AD user UPN}`.
 1. Der Wert sollte nun angezeigt werden.

    ![Der Wert wird angezeigt.](media/how-to-transformation/transform4.png)

## <a name="custom-attribute-mapping-with-function"></a>Benutzerdefinierte Attributzuordnung mit Funktionen
Für komplexere Zuordnungen können Sie Funktionen verwenden, mit denen Sie entsprechend den Anforderungen Ihrer Organisation die Daten bearbeiten und Werte für Attribute erstellen können.

Führen Sie dazu die oben beschriebenen Schritte aus, und bearbeiten Sie dann die Funktion, über die der endgültige Wert erstellt wird.

Informationen zur Syntax und Beispiele für Ausdrücke finden Sie unter [Schreiben von Ausdrücken für Attributzuordnungen in Azure Active Directory](reference-expressions.md).


## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)
