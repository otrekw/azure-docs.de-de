---
title: Erstellen einer privaten Cloud von Azure VMware Solution
description: Hier erfahren Sie mehr über die Schritte zum Erstellen einer privaten Azure VMware Solution-Cloud über das Azure-Portal.
ms.topic: include
ms.date: 04/07/2021
ms.openlocfilehash: 6b4e5631d1a4b6c5bf56b01aba12752595ef63b8
ms.sourcegitcommit: 6ed3928efe4734513bad388737dd6d27c4c602fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "107073360"
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
   | **Adressblock** | Geben Sie einen IP-Adressblock für das CIDR-Netzwerk für die private Cloud ein, etwa 10.175.0.0/22. |
   | **Virtual Network** | Wählen Sie für die private Azure VMware Solution-Cloud ein virtuelles Netzwerk aus, oder erstellen Sie ein neues.  |

   :::image type="content" source="../media/tutorial-create-private-cloud/create-private-cloud.png" alt-text="Geben Sie auf der Registerkarte „Grundlagen“ Werte für die Felder ein." border="true":::

1. Wählen Sie abschließend **Überprüfen + erstellen** aus. Überprüfen Sie auf dem nächsten Bildschirm die eingegebenen Informationen. Sind alle Informationen korrekt, wählen Sie **Erstellen** aus.

   > [!NOTE]
   > Dieser Schritt dauert ungefähr drei bis vier Stunden. Das Hinzufügen eines einzelnen Knotens in einem vorhandenen/gleichen Cluster dauert zwischen 30 und 45 Minuten.

1. Vergewissern Sie sich, dass die Bereitstellung erfolgreich war. Navigieren Sie zu der von Ihnen erstellten Ressourcengruppe, und wählen Sie Ihre private Cloud aus.  Wenn die Bereitstellung abgeschlossen ist, wird der Status **Erfolgreich** angezeigt. 

   :::image type="content" source="../media/tutorial-create-private-cloud/validate-deployment.png" alt-text="Vergewissern Sie sich, dass die Bereitstellung erfolgreich war." border="true":::
