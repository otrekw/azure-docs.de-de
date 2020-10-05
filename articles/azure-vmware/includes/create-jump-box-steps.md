---
title: Erstellen der Azure VMware Solution-Jumpbox
description: Schritte zum Erstellen der Azure VMware Solution-Jumpbox
ms.topic: include
ms.date: 09/28/2020
ms.openlocfilehash: 5d5a5d82cf6e70e7ad55bcfa10c7f85372131c87
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2020
ms.locfileid: "91578415"
---
<!-- Used in deploy-azure-vmware-solution.md and tutorial-access-private-cloud.md -->

1. Wählen Sie in der Ressourcengruppe **+ Hinzufügen** aus, suchen Sie nach **Microsoft Windows 10**, wählen Sie die Option aus, und wählen Sie anschließend **Erstellen** aus.

   :::image type="content" source="../media/tutorial-access-private-cloud/ss8-azure-w10vm-create.png" alt-text="Eine neue Windows 10-VM für eine Jumpbox hinzufügen" border="true":::

1. Geben Sie die erforderlichen Informationen in die Felder ein, und wählen Sie anschließend **Überprüfen + erstellen** aus. 

   Weitere Informationen zu den Feldern finden Sie in der folgenden Tabelle:

   | Feld | Wert |
   | --- | --- |
   | **Abonnement** | Dieses Feld ist bereits mit dem Abonnement ausgefüllt, das zur Ressourcengruppe gehört. |
   | **Ressourcengruppe** | Dieses Feld ist bereits mit dem Namen der aktuellen Ressourcengruppe ausgefüllt, die Sie im vorherigen Tutorial erstellt haben.  |
   | **Name des virtuellen Computers** | Geben Sie einen eindeutigen Namen für den virtuellen Computer ein. |
   | **Region** | Wählen Sie den geografischen Standort des virtuellen Computers aus. |
   | **Verfügbarkeitsoptionen** | Übernehmen Sie den Standardwert. |
   | **Image** | Wählen Sie das VM-Image aus. |
   | **Größe** | Übernehmen Sie die Standardgröße. |
   | **Authentifizierungstyp**  | Wählen Sie **Kennwort** aus. |
   | **Benutzername** | Geben Sie den Benutzernamen für die Anmeldung bei dem virtuellen Computer ein. |
   | **Kennwort** | Geben Sie das Kennwort für die Anmeldung bei dem virtuellen Computer ein. |
   | **Kennwort bestätigen** | Geben Sie das Kennwort für die Anmeldung bei dem virtuellen Computer ein. |
   | **Öffentliche Eingangsports** | Wählen Sie **Keine**. Wenn Sie „Keine“ auswählen, können Sie [JIT-Zugriff](../../security-center/security-center-just-in-time.md#jit-configure) verwenden, um den Zugriff auf den virtuellen Computer nur dann zu steuern, wenn Sie darauf zugreifen möchten.  |


1. Wählen Sie nach erfolgreicher Überprüfung die Option **Erstellen** aus, um den VM-Erstellungsprozess zu starten.

   :::image type="content" source="../media/tutorial-access-private-cloud/ss11-review-create-wjb01.png" alt-text="Eine neue Windows 10-VM für eine Jumpbox hinzufügen" border="true":::