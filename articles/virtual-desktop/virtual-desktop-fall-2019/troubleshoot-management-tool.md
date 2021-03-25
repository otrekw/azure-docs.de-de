---
title: Windows Virtual Desktop-Verwaltungstool (klassisch) – Azure
description: Behandeln von Problemen mit dem Windows Virtual Desktop-Verwaltungstool (klassisch).
author: Heidilohr
ms.topic: troubleshooting
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 6f03284103b4a2aa6900bf1ba5c50a4688c50b0e
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "88005481"
---
# <a name="troubleshoot-the-windows-virtual-desktop-classic-management-tool"></a>Problembehandlung für das Windows Virtual Desktop-Verwaltungstool (klassisch)

>[!IMPORTANT]
>Dieser Inhalt gilt für Windows Virtual Desktop (klassisch). Der Dienst unterstützt keine Windows Virtual Desktop-Objekte in Azure Resource Manager.

In diesem Artikel werden Probleme, die bei der Bereitstellung des Windows Virtual Desktop-Verwaltungstools auftreten können, und deren Behebung beschrieben.

## <a name="error-management-tool-services-configured-but-automated-setup-fails"></a>Error: Verwaltungstooldienste konfiguriert, aber automatisiertes Setup schlägt fehl

Wenn Sie die Dienste für das Verwaltungstool erfolgreich eingerichtet haben, das automatisierte Setup jedoch fehlschlägt, wird die folgende Fehlermeldung angezeigt:

```console
{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/arm-debug for usage details.","details":[{"code":"Conflict","message":"{\r\n "status": "Failed",\r\n "error": {\r\n "code": "ResourceDeploymentFailure",\r\n "message": "The resource operation completed with terminal provisioning state 'Failed'."\r\n }\r\n}"}]}
```

Dafür kommt in der Regel eine der beiden folgenden Ursachen infrage:

- Der Benutzer verfügt über Besitzerberechtigungen für das Abonnement und ist globaler Administrator auf Mandantenebene, kann sich aber nicht bei Azure anmelden.
- Für die Kontoeinstellungen des Benutzers ist die mehrstufige Authentifizierung aktiviert.

So beheben Sie dieses Problem:

1. Stellen Sie sicher, dass der Benutzer, den Sie für den Azure Active Directory-Benutzerprinzipalnamen erstellt haben, die Abonnementebene "Mitwirkender" aufweist.
2. Melden Sie sich bei <portal.azure.com> mit dem UPN-Konto an, um die Kontoeinstellungen zu überprüfen, und achten Sie darauf, dass die mehrstufige Authentifizierung nicht aktiviert ist. Deaktivieren Sie diese, falls sie aktiviert ist.
3. Rufen Sie die Windows Virtual Desktop-Einwilligungsseite auf und stellen Sie sicher, dass Server- und Client-Apps Konsens haben.
4. Arbeiten Sie das Tutorial [Bereitstellen eines Verwaltungstools](manage-resources-using-ui.md) durch, wenn das Problem weiterhin besteht, und stellen Sie das Tool erneut bereit.

## <a name="error-job-with-specified-id-already-exists"></a>Error: Auftrag mit der angegebenen ID bereits vorhanden

Wenn der Benutzer die Fehlermeldung sieht, dass der Auftrag mit der angegebenen ID bereits vorhanden ist, liegt dies daran, dass er beim Bereitstellen der Vorlage keinen eindeutigen Namen im Parameter "Anwendungsname" bereitgestellt hat.

Um dieses Problem zu beheben, stellen Sie das Verwaltungstool erneut bereit und geben Sie den Parameter "Anwendungsname" geeignet an.

## <a name="delayed-consent-prompt-when-opening-management-tool"></a>Verzögerte Zustimmungsaufforderung beim Öffnen des Verwaltungstools

Wenn Sie das Verwaltungstool bereitstellen, wird die Zustimmungsaufforderung u. U. nicht sofort geöffnet. Dies bedeutet, dass das Laden des Azure-Webanwendungsdiensts länger dauert als üblich. Die Aufforderung sollte angezeigt werden, nachdem Azure Web geladen wurde.

## <a name="the-user-cant-deploy-the-management-tool-in-the-east-us-region"></a>Benutzer kann das Verwaltungstool nicht in der Region "USA, Osten" bereitstellen

Wenn ein Kunde die Region auf "USA, Osten" festlegt, kann er das Verwaltungstool nicht bereitstellen.

Stellen Sie das Verwaltungstool in einer anderen Region bereit, um dieses Problem zu beheben. Die Bereitstellung des Tools in einer anderen Region sollte sich nicht auf die Benutzerfreundlichkeit auswirken.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Eskalationspfaden finden Sie unter [Übersicht über die Problembehandlung, Feedback und Support](troubleshoot-set-up-overview-2019.md).
- Weitere Informationen zum Melden von Problemen mit Windows Virtual Desktop-Tools finden Sie unter [ARM-Vorlagen für Remotedesktopdienste](https://github.com/Azure/RDS-Templates/blob/master/README.md)melden.
- Informationen zum Bereitstellen des Verwaltungstools finden Sie unter [Bereitstellen eines Verwaltungstools](manage-resources-using-ui.md).