---
title: Aktivieren der Unterstützung für Avere vFXT – Azure
description: Hier erfahren Sie, wie Sie das automatische Hochladen von Supportdaten zu Ihrem Cluster aus Avere vFXT for Azure aktivieren, um den Support beim Bereitstellen des Kundendiensts zu unterstützen.
author: ekpgh
ms.service: avere-vfxt
ms.topic: how-to
ms.date: 12/14/2019
ms.author: rohogue
ms.openlocfilehash: 93b99aa624a21d9312297e4279b1dcf053c79ae3
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "88272727"
---
# <a name="enable-support-uploads"></a>Aktivieren von Supportuploads

Avere vFXT für Azure kann automatisch Supportdaten über Ihren Cluster hochladen. Diese Uploads ermöglichen es den Supportmitarbeitern, den bestmöglichen Kundendienst zu bieten.

## <a name="steps-to-enable-uploads"></a>Schritte zum Aktivieren von Uploads

Führen Sie die folgenden Schritte über die Avere-Systemsteuerung aus, um den Support zu aktivieren. (Informationen zum Öffnen der Systemsteuerung finden Sie unter [Zugreifen auf den vFXT-Cluster](avere-vfxt-cluster-gui.md).)

1. Navigieren Sie am oberen Rand zur Registerkarte **Einstellungen**.
1. Klicken Sie links auf den Link **Support**, und akzeptieren Sie die Datenschutzrichtlinie.

   ![Screenshot der Avere-Systemsteuerung und des Popupfensters mit der Bestätigungsschaltfläche zum Akzeptieren der Datenschutzrichtlinie](media/avere-vfxt-privacy-policy.png)

1. Öffnen Sie auf der Seite „Supportkonfiguration“ den Abschnitt **Kundeninformationen**, indem Sie links auf das Dreieck klicken.
1. Klicken Sie auf die Schaltfläche **Revalidate upload information** (Uploadinformationen erneut überprüfen).
1. Legen Sie den Supportnamen des Clusters in **Eindeutiger Clustername** fest. Stellen Sie sicher, dass dieser Name Ihren Cluster für Supportmitarbeitern eindeutig identifiziert.
1. Aktivieren Sie die Kontrollkästchen für **Statistiküberwachung**, **Upload allgemeiner Informationen** und **Upload von Absturzinformationen**.
1. Klicken Sie auf **Submit**(Senden).

   ![Screenshot der Supporteinstellungsseite mit ausgefüllten Kundeninformationen](media/avere-vfxt-support-info.png)

1. Klicken Sie auf das Dreieck links neben **Secure Proactive Support (SPS)** , um den Abschnitt zu erweitern.
1. Aktivieren Sie das Kontrollkästchen für **SPS-Link aktivieren**.
1. Klicken Sie auf **Submit**(Senden).

   ![Screenshot der Supporteinstellungsseite mit ausgefülltem SPS-Abschnitt](media/avere-vfxt-support-sps.png)

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie dem Cluster ein lokales Speichersystem oder ein vorhandenes Cloudspeichersystem hinzufügen müssen, folgen Sie den Anweisungen unter [Konfigurieren von Speicher](avere-vfxt-add-storage.md).

Wenn Sie bereit sind, Clients an den Cluster anzufügen, lesen Sie [Einbinden des Avere vFXT-Clusters](avere-vfxt-mount-clients.md).
