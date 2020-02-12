---
title: 'Zugreifen auf Azure VMware Solutions (AVS): Portal'
description: Beschreibt, wie Sie über das Azure-Portal auf Azure VMware Solutions (AVS) zugreifen
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: c0b0601b9d2cf505aa5b25e10875338352cab771
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77015949"
---
# <a name="access-azure-vmware-solutions-avs-from-the-azure-portal"></a>Zugreifen auf Azure VMware Solutions (AVS) über das Azure-Portal

Einmaliges Anmelden wird für den Zugriff auf das AVS-Portal unterstützt. Nach Ihrer Anmeldung beim Azure-Portal können Sie auf das AVS-Portal zugreifen, ohne sich erneut anmelden zu müssen. Wenn Sie zum ersten Mal auf das AVS-Portal zugreifen, werden Sie aufgefordert, die Anwendung [AVS Service Authorization](#consent-to-avs-service-authorization-application) (AVS-Dienstautorisierung) zu autorisieren. Autorisierung ist eine einmalige Aktion.

## <a name="before-you-begin"></a>Voraussetzungen

Benutzer mit den integrierten Rollen **Besitzer** und **Mitwirkender** können auf das AVS-Portal zugreifen. Die Rollen müssen in der Ressourcengruppe konfiguriert werden, in der der AVS-Dienst bereitgestellt wird. Die Rollen können auch für das AVS-Dienstobjekt konfiguriert werden. Weitere Informationen zum Überprüfen Ihrer Rolle finden Sie im Artikel [Anzeigen von Rollenzuweisungen](https://docs.microsoft.com/azure/role-based-access-control/check-access). Nur Benutzer mit den integrierten Rollen **Besitzer** und **Mitwirkender** können auf das AVS-Portal zugreifen. Die Rollen müssen für das Abonnement konfiguriert sein. Weitere Informationen zum Überprüfen Ihrer Rolle finden Sie im Artikel [Anzeigen von Rollenzuweisungen](https://docs.microsoft.com/azure/role-based-access-control/check-access).

Wenn Sie benutzerdefinierte Rollen verwenden, sollte die Rolle über beliebige der folgenden Vorgänge unter ```Actions``` verfügen.  Weitere Informationen zu benutzerdefinierten Rollen finden Sie unter [Benutzerdefinierte Rollen für Azure-Ressourcen](https://docs.microsoft.com/azure/role-based-access-control/custom-roles). Wenn einer der Vorgänge Teil von ```NotActions``` ist, kann der Benutzer nicht auf das AVS-Portal zugreifen. 

```
Microsoft.VMwareCloudSimple/*
Microsoft.VMwareCloudSimple/*/write
Microsoft.VMwareCloudSimple/dedicatedCloudServices/*
Microsoft.VMwareCloudSimple/dedicatedCloudServices/*/write
```

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="access-the-avs-portal"></a>Zugreifen auf das AVS-Portal

1. Wählen Sie **Alle Dienste** aus.

2. Suchen Sie nach **AVS Services** (AVS-Dienste).

3. Wählen Sie den AVS-Dienst aus, über den Sie Ihre private Cloud erstellen möchten.

4. Klicken Sie auf der Seite **Übersicht** auf **Go to the AVS portal** (Zum AVS-Portal wechseln). Wenn Sie zum ersten Mal über das Azure-Portal auf das AVS-Portal zugreifen, werden Sie aufgefordert, die Anwendung [AVS Service Authorization](#consent-to-avs-service-authorization-application) zu autorisieren. 

    ![Starten des AVS-Portals](media/launch-cloudsimple-portal.png)

> [!NOTE]
> Wenn Sie einen Vorgang im Zusammenhang mit einer privaten Cloud (z. B. Erstellen oder Erweitern einer privaten Cloud) direkt über das Azure-Portal auswählen, wird das AVS-Portal auf der angegebenen Seite geöffnet.

Wählen Sie im AVS-Portal im seitlichen Menü **Home** aus, um zusammenfassende Informationen zu Ihren privaten AVS-Clouds anzuzeigen. Die Ressourcen und die Kapazität Ihrer privaten AVS-Cloud werden angezeigt, zusammen mit Warnungen und Aufgaben, die Ihre Aufmerksamkeit erfordern. Um allgemeine Aufgaben anzuzeigen, klicken Sie auf die benannten Symbole am oberen Rand der Seite.

![Startseite](media/cloudsimple-portal-home.png)

## <a name="consent-to-avs-service-authorization-application"></a>Einwilligen für die Anwendung AVS Service Authorization

Wenn Sie das AVS-Portal über das Azure-Portal zum ersten Mal starten, ist Ihre Einwilligung für die Anwendung AVS Service Authorization erforderlich. Wählen Sie **Akzeptieren** aus, um die erforderlichen Berechtigungen und den Zugriff auf das AVS-Portal zu gewähren.

![Einwilligen für AVS Service Authorization – Administratoren](media/cloudsimple-azure-consent.png)

Wenn Sie über globale Administratorrechte verfügen, können Sie für Ihre Organisation zustimmen. Wählen Sie **Zustimmung im Namen Ihrer Organisation** aus.

![Einwilligen für AVS Service Authorization – globaler Administrator](media/cloudsimple-azure-consent-global-admin.png)

Wenn Ihre Berechtigungen den Zugriff auf das AVS-Portal nicht zulassen, wenden Sie sich an den globalen Administrator Ihres Mandanten, um die erforderlichen Berechtigungen zu erhalten. Ein globaler Administrator kann im Namen Ihrer Organisation zustimmen.

![Einwilligen für AVS Service Authorization – Administratoren erforderlich](media/cloudsimple-azure-consent-requires-administrator.png)

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Erstellen einer privaten Cloud](https://docs.azure.cloudsimple.com/create-private-cloud/).
* Erfahren Sie mehr über [Konfigurieren einer Private Cloud-Umgebung](quickstart-create-private-cloud.md).
