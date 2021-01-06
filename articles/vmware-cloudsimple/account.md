---
title: Kontoverwaltung – Azure VMware Solution by CloudSimple-Portal
description: Beschreibt, wie Sie Konten in Azure VMware Solution by CloudSimple über das Portal verwalten.
author: Ajayan1008
ms.author: v-hborys
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 4c26d5accce77ce6fd8c9b6c2b519b93f95013ce
ms.sourcegitcommit: d7d5f0da1dda786bda0260cf43bd4716e5bda08b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2021
ms.locfileid: "97895171"
---
# <a name="manage-accounts-on-the-azure-vmware-solution-by-cloudsimple-portal"></a>Verwalten von Konten über das VMware Solution by CloudSimple-Portal

Wenn Sie Ihren CloudSimple-Dienst erstellen, wird ein Konto bei CloudSimple erstellt. Das Konto wird mit Ihrem Azure-Abonnement verknüpft, in dem sich der Dienst befindet. Alle Benutzer, die im Abonnement die Rollen „Besitzer“ und „Mitwirkender“ ausüben, haben Zugriff auf das CloudSimple-Portal. Die Azure-Abonnement-ID und die Mandanten-ID, die mit dem CloudSimple-Dienst verknüpft sind, finden Sie auf der Seite „Accounts“ (Konten).

Um Konten im CloudSimple-Portal zu verwalten, [greifen Sie auf das Portal zu](access-cloudsimple-portal.md), und wählen Sie im seitlichen Menü die Option **Account** (Konto) aus.

Wählen Sie **Summary** (Zusammenfassung) aus, um Informationen zur CloudSimple-Konfiguration Ihres Unternehmens anzuzeigen. Die aktuelle Kapazität Ihrer Cloudkonfiguration wird angezeigt, einschließlich der Anzahl der privaten Clouds, des Gesamtspeichers, der vSphere-Clusterkonfiguration, der Anzahl der Knoten und der Anzahl von Computekernen. Ein Link zum Erwerb zusätzlicher Knoten ist enthalten, wenn die aktuelle Konfiguration nicht alle Ihre Anforderungen erfüllt.

## <a name="email-alerts"></a>E-Mail-Benachrichtigungen

Sie können E-Mail-Adressen aller Personen hinzufügen, die Sie über Änderungen an der Konfiguration der privaten Cloud benachrichtigen möchten.

1. Klicken Sie im Bereich **Additional email alerts** (Zusätzliche E-Mail-Benachrichtigungen) auf **Add new** (Neue hinzufügen).
2. Geben Sie die E-Mail-Adresse ein.
3. Drücken Sie die EINGABETASTE.  

Um einen Eintrag zu entfernen, klicken Sie auf **X**.

## <a name="cloudsimple-operator-access"></a>CloudSimple-Zugriff für Dienstvorgangspersonal

Mit der Einstellung „Operator Access“ (Operatorzugriff) kann CloudSimple Sie bei der Problembehandlung unterstützen, indem Sie einem Supportmitarbeiter gestatten, sich an Ihrem CloudSimple-Portal anzumelden.  Diese Einstellung ist standardmäßig aktiviert. Alle Aktionen, die vom Supportmitarbeiter bei der Anmeldung bei Ihrem Kundenkonto durchgeführt werden, werden aufgezeichnet und stehen zur Überprüfung auf der Seite **Activity (Aktivität)**  > **Audit (Überwachung)** zur Verfügung.

Klicken Sie auf die Umschaltfläche **CloudSimple operator access enabled** (CloudSimple-Operatorzugriff aktiviert), um den Zugriff zu aktivieren oder zu deaktivieren.
