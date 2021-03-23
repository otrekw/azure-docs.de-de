---
title: Erstellen der Azure VMware Solution-Jumpbox
description: Schritte zum Erstellen der Azure VMware Solution-Jumpbox
ms.topic: include
ms.date: 03/13/2021
ms.openlocfilehash: f746e11763e1df1686f3134960dea167bf1c9908
ms.sourcegitcommit: afb9e9d0b0c7e37166b9d1de6b71cd0e2fb9abf5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/14/2021
ms.locfileid: "103462248"
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
   | **Öffentliche Eingangsports** | Wählen Sie **Keine**. Wenn Sie „Keine“ auswählen, können Sie [JIT-Zugriff](../../security-center/security-center-just-in-time.md#jit-configure) verwenden, um den Zugriff auf den virtuellen Computer nur dann zu steuern, wenn Sie darauf zugreifen möchten. Alternativ können Sie auch eine [Azure Bastion](../../bastion/tutorial-create-host-portal.md)-Instanz verwenden, wenn Sie über das Internet sicher auf den Jumpboxserver zugreifen möchten, ohne Netzwerkports verfügbar zu machen.  |


1. Wählen Sie nach erfolgreicher Überprüfung die Option **Erstellen** aus, um den VM-Erstellungsprozess zu starten.

