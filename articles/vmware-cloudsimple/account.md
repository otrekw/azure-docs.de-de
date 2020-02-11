---
title: 'Kontoverwaltung: AVS-Portal (Azure VMware Solutions)'
description: Es wird beschrieben, wie Sie Konten über das AVS-Portal (Azure VMware Solution) verwalten.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 1fd7cb1c55fb664828448cef0b67ea9b16323bdf
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025366"
---
# <a name="manage-accounts-on-the-azure-vmware-solutions-avs-portal"></a>Verwalten von Konten im AVS-Portal (Azure VMware Solutions)

Wenn Sie Ihren AVS-Dienst erstellen, wird in AVS ein Konto erstellt. Das Konto wird mit Ihrem Azure-Abonnement verknüpft, in dem sich der Dienst befindet. Alle Benutzer, die im Abonnement die Rollen „Besitzer“ und „Mitwirkender“ haben, verfügen über Zugriff auf das AVS-Portal. Die Azure-Abonnement-ID und die Mandanten-ID, die mit dem AVS-Dienst verknüpft sind, finden Sie auf der Seite „Accounts“ (Konten).

Um Konten im AVS-Portal zu verwalten, [greifen Sie auf das Portal zu](access-cloudsimple-portal.md) und wählen im seitlichen Menü die Option **Account** (Konto) aus.

Wählen Sie **Summary** (Zusammenfassung) aus, um Informationen zur AVS-Konfiguration Ihres Unternehmens anzuzeigen. Die aktuelle Kapazität Ihrer Cloudkonfiguration wird angezeigt, einschließlich der Anzahl privater AVS-Clouds, des Gesamtspeichers, der vSphere-Clusterkonfiguration, der Anzahl von Knoten und der Anzahl von Computekernen. Ein Link zum Erwerb zusätzlicher Knoten ist enthalten, wenn die aktuelle Konfiguration nicht alle Ihre Anforderungen erfüllt.

## <a name="email-alerts"></a>E-Mail-Benachrichtigungen

Sie können E-Mail-Adressen aller Personen hinzufügen, die Sie über Änderungen an der Konfiguration der privaten AVS-Cloud benachrichtigen möchten.

1. Klicken Sie im Bereich **Additional email alerts** (Zusätzliche E-Mail-Benachrichtigungen) auf **Add new** (Neue hinzufügen).
2. Geben Sie die E-Mail-Adresse ein.
3. Drücken Sie die EINGABETASTE.  

Um einen Eintrag zu entfernen, klicken Sie auf **X**.

## <a name="avs-operator-access"></a>Zugriff des AVS-Operators

Mit der Einstellung für den Operatorzugriff kann AVS Sie bei der Problembehandlung unterstützen, indem Sie einem Supportmitarbeiter gestatten, sich an Ihrem AVS-Portal anzumelden. Diese Einstellung ist standardmäßig aktiviert. Alle Aktionen, die vom Supportmitarbeiter bei der Anmeldung bei Ihrem Kundenkonto durchgeführt werden, werden aufgezeichnet und stehen zur Überprüfung auf der Seite **Activity (Aktivität)**  > **Audit (Überwachung)** zur Verfügung.

Klicken Sie auf die Umschaltfläche **AVS operator access enabled** (AVS-Operatorzugriff aktiviert), um den Zugriff zu aktivieren oder zu deaktivieren.
