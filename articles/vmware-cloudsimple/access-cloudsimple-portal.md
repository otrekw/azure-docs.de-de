---
title: Zugreifen auf Azure VMware Solution by CloudSimple – Portal
description: Beschreibt, wie Sie über das Azure-Portal auf das VMware Solution by CloudSimple-Portal zugreifen
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 95d27bab243f6805436465f5027e519d33e20f6f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79225086"
---
# <a name="access-the-vmware-solution-by-cloudsimple-portal-from-the-azure-portal"></a>Zugreifen auf das VMware Solution by CloudSimple-Portal über das Azure-Portal

Einmaliges Anmelden wird für den Zugriff auf das CloudSimple-Portal unterstützt. Nach Ihrer Anmeldung beim Azure-Portal können Sie auf das CloudSimple-Portal zugreifen, ohne sich erneut anmelden zu müssen. Wenn Sie zum ersten Mal auf das CloudSimple-Portal zugreifen, werden Sie aufgefordert, die Anwendung [CloudSimple Service Authorization](#consent-to-cloudsimple-service-authorization-application) (CloudSimple-Dienstautorisierung) zu autorisieren.  Autorisierung ist eine einmalige Aktion.

## <a name="before-you-begin"></a>Voraussetzungen

Benutzer mit den integrierten Rollen **Besitzer** und **Mitwirkender** können auf das CloudSimple-Portal zugreifen.  Die Rollen müssen in der Ressourcengruppe konfiguriert werden, in der der CloudSimple-Dienst bereitgestellt wird.  Die Rollen können auch für das CloudSimple-Dienstobjekt konfiguriert werden.  Weitere Informationen zum Überprüfen Ihrer Rolle finden Sie im Artikel [Anzeigen von Rollenzuweisungen](https://docs.microsoft.com/azure/role-based-access-control/check-access). Benutzer mit integrierten **Besitzer**- und **Mitwirkender**-Rollen können auf das CloudSimple-Portal zugreifen.  Die Rollen müssen für das Abonnement konfiguriert sein.  Weitere Informationen zum Überprüfen Ihrer Rolle finden Sie im Artikel [Anzeigen von Rollenzuweisungen](https://docs.microsoft.com/azure/role-based-access-control/check-access).

Wenn Sie benutzerdefinierte Rollen verwenden, sollte die Rolle über beliebige der folgenden Vorgänge unter ```Actions``` verfügen.  Weitere Informationen zu benutzerdefinierten Rollen finden Sie unter [Benutzerdefinierte Rollen für Azure-Ressourcen](https://docs.microsoft.com/azure/role-based-access-control/custom-roles).  Wenn einer der Vorgänge Teil von ```NotActions``` ist, kann der Benutzer nicht auf das CloudSimple-Portal zugreifen.

```
Microsoft.VMwareCloudSimple/*
Microsoft.VMwareCloudSimple/*/write
Microsoft.VMwareCloudSimple/dedicatedCloudServices/*
Microsoft.VMwareCloudSimple/dedicatedCloudServices/*/write
```

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="access-the-cloudsimple-portal"></a>Zugriff auf das CloudSimple-Portal

1. Wählen Sie **Alle Dienste** aus.

2. Suchen Sie nach **CloudSimple Services**.

3. Wählen Sie den CloudSimple-Dienst aus, über den Sie Ihre private Cloud erstellen möchten.

4. Klicken Sie auf der Seite **Übersicht** auf **Go to the CloudSimple portal**.  Wenn Sie über das Azure-Portal zum ersten Mal auf das CloudSimple-Portal zugreifen, werden Sie aufgefordert, die Anwendung [CloudSimple Service Authorization](#consent-to-cloudsimple-service-authorization-application) zu autorisieren. 

    ![Starten des CloudSimple-Portals](media/launch-cloudsimple-portal.png)

> [!NOTE]
> Wenn Sie einen Vorgang im Zusammenhang mit einer privaten Cloud (z.B. das Erstellen oder Erweitern einer privaten Cloud) direkt über das Azure-Portal auswählen, wird das CloudSimple-Portal auf der angegebenen Seite geöffnet.

Wählen Sie im CloudSimple-Portal im seitlichen Menü **Home** aus, um zusammenfassende Informationen zu Ihren privaten Clouds anzuzeigen. Die Ressourcen und die Kapazität Ihrer privaten Clouds werden angezeigt, zusammen mit Warnungen und Aufgaben, die Ihre Aufmerksamkeit erfordern. Um allgemeine Aufgaben anzuzeigen, klicken Sie auf die benannten Symbole am oberen Rand der Seite.

![Startseite](media/cloudsimple-portal-home.png)

## <a name="consent-to-cloudsimple-service-authorization-application"></a>Anwendung „Consent to CloudSimple Service Authorization“ (Zustimmung zur CloudSimple-Dienstautorisierung)

Wenn Sie das CloudSimple-Portal über das Azure-Portal zum ersten Mal starten, ist Ihre Zustimmung für die Anwendung „CloudSimple Service Authorization“ erforderlich.  Wählen Sie **Akzeptieren** aus, um die erforderlichen Berechtigungen und den Zugriff auf das CloudSimple-Portal zu gewähren.

![Consent to CloudSimple Service Authorization – Administratoren](media/cloudsimple-azure-consent.png)

Wenn Sie über globale Administratorrechte verfügen, können Sie für Ihre Organisation zustimmen.  Wählen Sie **Zustimmung im Namen Ihrer Organisation** aus.

![Consent to CloudSimple Service Authorization – globaler Administrator](media/cloudsimple-azure-consent-global-admin.png)

Wenn Ihre Berechtigungen den Zugriff auf das CloudSimple-Portal nicht erlauben, wenden Sie sich an den globalen Administrator Ihres Mandanten, um die erforderlichen Berechtigungen zu erhalten.  Ein globaler Administrator kann im Namen Ihrer Organisation zustimmen.

![Consent to CloudSimple Service Authorization – erfordert Administratoren](media/cloudsimple-azure-consent-requires-administrator.png)

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Erstellen einer privaten Cloud](https://docs.azure.cloudsimple.com/create-private-cloud/).
* Erfahren Sie mehr über [Konfigurieren einer Private Cloud-Umgebung](quickstart-create-private-cloud.md).
