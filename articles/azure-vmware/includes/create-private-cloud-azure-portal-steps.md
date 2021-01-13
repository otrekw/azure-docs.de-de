---
title: Bereitstellen von Azure VMware Solution
description: Hier werden die Schritte zum Bereitstellen von Azure VMware Solution über das Azure-Portal erläutert.
ms.topic: include
ms.date: 09/28/2020
ms.openlocfilehash: 4d0881721cd8c13d1b6c9fb3a29e4cdb6d6a753f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91578313"
---
<!-- Used in deploy-azure-vmware-solution.md and tutorial-create-private-cloud.md -->

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie **Neue Ressource erstellen** aus. Geben Sie im Textfeld **Marketplace durchsuchen** den Text `Azure VMware Solution` ein, und wählen Sie in der Liste die Option **Azure-VMware-Lösung** aus. Wählen Sie im Fenster **Azure VMware Solution** die Option **Erstellen** aus.

1. Geben Sie auf der Registerkarte **Grundlegende Einstellungen** Werte für die Felder ein. In der folgenden Tabelle werden die Eigenschaften der Felder aufgeführt:

   | Feld   | Wert  |
   | ---| --- |
   | **Abonnement** | Das Abonnement, das Sie für die Bereitstellung verwenden möchten.|
   | **Ressourcengruppe** | Die Ressourcengruppe für die Ressourcen Ihrer privaten Cloud. |
   | **Location** | Wählen Sie einen Standort aus (beispielsweise **USA, Osten**).|
   | **Ressourcenname** | Der Name Ihrer privaten Azure VMware Solution-Cloud |
   | **SKU** | Wählen Sie den folgenden SKU-Wert aus: AV36 |
   | **Hosts** | Die Anzahl von Hosts, die dem privaten Cloudcluster hinzugefügt werden sollen. Der Standardwert ist 3, er kann jedoch nach der Bereitstellung erhöht oder verringert werden.  |
   | **vCenter admin password** (vCenter-Administratorkennwort) | Geben Sie ein Cloudadministratorkennwort ein. |
   | **NSX-T manager password** (NSX-T-Manager-Kennwort) | Geben Sie ein NSX-T-Administratorkennwort ein. |
   | **Adressblock** | Geben Sie einen IP-Adressblock für das CIDR-Netzwerk für die private Cloud ein, etwa 10.175.0.0/22. |
   | **Virtual Network** | Wählen Sie für die private Azure VMware Solution-Cloud ein virtuelles Netzwerk aus, oder erstellen Sie ein neues.  |

   :::image type="content" source="../media/tutorial-create-private-cloud/create-private-cloud.png" alt-text="Geben Sie auf der Registerkarte „Grundlagen“ Werte für die Felder ein." border="true":::

1. Wählen Sie abschließend **Überprüfen + erstellen** aus. Überprüfen Sie auf dem nächsten Bildschirm die eingegebenen Informationen. Sind alle Informationen korrekt, wählen Sie **Erstellen** aus.

   > [!NOTE]
   > Dieser Schritt dauert etwa zwei Stunden. 

1. Vergewissern Sie sich, dass die Bereitstellung erfolgreich war. Navigieren Sie zu der von Ihnen erstellten Ressourcengruppe, und wählen Sie Ihre private Cloud aus.  Wenn die Bereitstellung abgeschlossen ist, wird der Status **Erfolgreich** angezeigt. 

   :::image type="content" source="../media/tutorial-create-private-cloud/validate-deployment.png" alt-text="Geben Sie auf der Registerkarte „Grundlagen“ Werte für die Felder ein." border="true":::