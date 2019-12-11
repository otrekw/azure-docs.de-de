---
title: 'Azure AD Connect-Cloudbereitstellung: Transformationen'
description: In diesem Dokument wird beschrieben, wie Sie mithilfe von Transformationen die Standardattributzuordnungen ändern können.
author: billmath
ms.author: billmath
manager: davba
ms.date: 12/02/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0d37fdb4ad0d385914aecd4ca62be498c5c0e7c5
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74795521"
---
# <a name="transformations"></a>Transformationen

Mit einer Transformation können Sie das Standardverhalten der Synchronisierung eines Attributs mit Azure AD unter Verwendung der Cloudbereitstellung ändern.  

Dazu müssen Sie das Schema bearbeiten und es dann über eine Webanforderung erneut übermitteln.

Weitere Informationen zu Cloudbereitstellungsattributen finden Sie unter [Grundlegendes zum Azure AD-Schema](concept-attributes.md).


## <a name="retrieve-the-schema"></a>Abrufen des Schemas
Rufen Sie das Schema entsprechend den unter [Anzeigen des Schemas](concept-attributes.md#viewing-the-schema) beschriebenen Schritten ab. 


## <a name="custom-attribute-mapping"></a>Benutzerdefinierte Attributzuordnung
Gehen Sie wie folgt vor, um eine benutzerdefinierte Attributzuordnung hinzuzufügen.

1. Kopieren Sie das Schema in einen Text-Editor oder Code-Editor, z. B. [Visual Studio Code](https://code.visualstudio.com/).  
2. Suchen Sie das Objekt, das Sie im Schema aktualisieren möchten. ![](media/how-to-transformation/transform1.png)</br>
3. Suchen Sie den Code für **ExtensionAttribute3** unter dem Benutzerobjekt.

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
 4.  Bearbeiten Sie den Code so, dass das company-Attribut ExtensionAttribute3 zugeordnet ist.
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
 5. Kopieren Sie das Schema wieder zurück in den Graph-Tester, ändern Sie den Anforderungstyp in PUT, und wählen Sie **Abfrage ausführen** aus.  
 ![](media/how-to-transformation/transform2.png)</br>
 6.  Navigieren Sie im Azure-Portal zur Konfiguration der Cloudbereitstellung, und wählen Sie **Bereitstellung erneut starten** aus.
 ![](media/how-to-transformation/transform3.png)</br>
 7.  Überprüfen Sie nach einiger Zeit, ob die Attribute gefüllt werden, indem Sie die folgende Abfrage im Graph-Tester ausführen: `https://graph.microsoft.com/beta/users/{Azure AD user UPN}`.
 8.  Der Wert sollte nun angezeigt werden.
 ![](media/how-to-transformation/transform4.png)</br>

## <a name="custom-attribute-mapping-with-function"></a>Benutzerdefinierte Attributzuordnung mit Funktionen
Für erweiterte Zuordnungen können Sie Funktionen verwenden, mit denen Sie entsprechend den Anforderungen Ihrer Organisation die Daten bearbeiten und Werte für Attribute erstellen können.

Führen Sie dazu einfach die oben beschriebenen Schritte aus, und bearbeiten Sie die Funktion, über die der endgültige Wert erstellt wird.

Informationen zur Syntax und Beispiele für Ausdrücke finden Sie unter [Schreiben von Ausdrücken für Attributzuordnungen in Azure Active Directory](reference-expressions.md).


## <a name="next-steps"></a>Nächste Schritte 

- [Was ist eine Bereitstellung?](what-is-provisioning.md)
- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)
