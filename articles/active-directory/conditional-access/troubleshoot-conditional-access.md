---
title: Beheben von Anmeldeproblemen bei bedingtem Zugriff – Azure Active Directory
description: In diesem Artikel wird beschrieben, was zu tun ist, wenn Ihre Richtlinien für den bedingten Zugriff zu unerwarteten Ergebnissen führen.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: troubleshooting
ms.date: 04/30/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, martinco
ms.collection: M365-identity-device-management
ms.openlocfilehash: c9a2ab4b1ebc1c958be9dc4bd07a010f7fef8afc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "82610509"
---
# <a name="troubleshooting-sign-in-problems-with-conditional-access"></a>Beheben von Anmeldeproblemen bei bedingtem Zugriff

Anhand der Informationen in diesem Artikel und mithilfe von Fehlermeldungen und dem Azure AD-Anmeldeprotokoll können Sie Probleme bei unerwarteten Anmeldeergebnissen im Zusammenhang mit dem bedingten Zugriff beheben.

## <a name="conditional-access-sign-in-interrupt"></a>Unterbrechung der Anmeldung bei bedingtem Zugriff

Die erste Möglichkeit besteht darin, die angezeigte Fehlermeldung zu überprüfen. Bei Problemen mit der Anmeldung über einen Webbrowser enthält die eigentliche Fehlerseite ausführliche Informationen. Diese Informationen können lediglich das Problem beschreiben und eine Lösung vorschlagen.

![Anmeldefehler – kompatibles Gerät erforderlich](./media/troubleshoot-conditional-access/image1.png)

Die Meldung für den obigen Fehler besagt, dass der Zugriff auf die Anwendung nur von Geräten oder Clientanwendungen erfolgen kann, welche die Richtlinie für die Verwaltung mobiler Geräte im Unternehmen erfüllen. In diesem Fall erfüllen die Anwendung und das Gerät diese Richtlinie nicht.

## <a name="azure-ad-sign-in-events"></a>Azure AD-Anmeldeereignisse

Die zweite Methode zum Abrufen detaillierter Informationen zu der Anmeldeunterbrechung besteht darin, die Azure AD-Anmeldeereignisse zu überprüfen, um festzustellen, welche Richtlinie(n) für den bedingten Zugriff angewendet wurde(n) und aus welchem Grund.

Weitere Informationen zu diesem Problem erhalten Sie, wenn Sie auf der ersten Fehlerseite auf **Weitere Informationen** klicken. Durch das Klicken auf **Weitere Informationen** werden Informationen zur Problembehandlung angezeigt, die beim Durchsuchen der Azure AD-Anmeldeereignisse für ein bestimmtes Fehlerereignis, das dem Benutzer angezeigt wurde, oder beim Öffnen eines Supportfalls bei Microsoft hilfreich sein können.

![Weitere Informationen zu einer unterbrochenen Anmeldung im Webbrowser bei bedingtem Zugriff](./media/troubleshoot-conditional-access/image2.png)

Gehen Sie wie folgt vor, um herauszufinden, welche Richtlinie(n) für den bedingten Zugriff angewendet wurde(n) und aus welchem Grund.

1. Melden Sie sich als globaler Administrator, Sicherheitsadministrator oder globaler Leser im **Azure-Portal** an.
1. Navigieren Sie zu **Azure Active Directory** > **Anmeldungen**.
1. Suchen Sie nach dem Ereignis für die zu überprüfende Anmeldung. Filtern Sie unnötige Informationen heraus, indem Sie Filter und Spalten hinzufügen oder entfernen.
   1. Fügen Sie Filter hinzu, um den Bereich einzugrenzen:
      1. **Korrelations-ID** zum Untersuchen eines bestimmten Ereignisses.
      1. **Bedingter Zugriff** zum Anzeigen von Richtlinienfehlern und Richtlinienkonformität. Legen Sie den Filter so fest, dass nur Fehler angezeigt werden, um die Ergebnisse einzugrenzen.
      1. **Benutzername** zum Anzeigen von Informationen zu bestimmten Benutzern.
      1. **Datum** zur Festlegung des fraglichen Zeitraums.

   ![Auswählen des Filters für den bedingten Zugriff im Anmeldeprotokoll](./media/troubleshoot-conditional-access/image3.png)

1. Wenn Sie das Anmeldeereignis, das dem Anmeldefehler des Benutzers entspricht, gefunden haben, wählen Sie die Registerkarte **Bedingter Zugriff** aus. Auf der Registerkarte „Bedingter Zugriff“ wird die spezielle Richtlinie (ggf. auch mehrere Richtlinien) angezeigt, die zur Unterbrechung der Anmeldung geführt hat.
   1. Die Informationen auf der Registerkarte **Problembehandlung und Support** geben möglicherweise eindeutig Auskunft darüber, warum bei der Anmeldung ein Fehler aufgetreten ist (z. B. ein Gerät, das die Konformitätsanforderungen nicht erfüllt).
   1. Zur weiteren Untersuchung führen Sie einen Drilldown in die Konfiguration der Richtlinien durch, indem Sie auf den **Richtliniennamen** klicken. Durch Klicken auf den **Richtliniennamen** wird für die ausgewählte Richtlinie die Benutzeroberfläche für die Richtlinienkonfiguration angezeigt. Hier können Sie die Richtlinie überprüfen und bearbeiten.
   1. Der **Clientbenutzer** und die **Gerätedetails**, die für die Bewertung der Richtlinie für bedingten Zugriff verwendet wurden, sind auch auf den Registerkarten **Grundlegende Infos**, **Standort**, **Geräteinformationen**, **Authentifizierungsdetails** und **Weitere Details** des Anmeldeereignisses verfügbar.
   1. Wenn Sie die Auslassungspunkte rechts von der Richtlinie auswählen, werden Richtliniendetails angezeigt. So erhalten Administratoren zusätzliche Informationen zum Grund für die erfolgreiche oder nicht erfolgreiche Anwendung einer Richtlinie.

   ![Registerkarte „Bedingter Zugriff“ des Anmeldeereignisses](./media/troubleshoot-conditional-access/image5.png)

   ![Richtliniendetails (Vorschau)](./media/troubleshoot-conditional-access/policy-details.png)

Wenn die im Ereignis angezeigten Informationen nicht ausreichen, um die Anmeldeergebnisse zu verstehen oder die Richtlinie anzupassen, um die gewünschten Ergebnisse zu erhalten, kann ein Supportfall geöffnet werden. Navigieren Sie zur Registerkarte **Problembehandlung und Support** des Anmeldeereignisses, und wählen Sie **Neue Supportanfrage erstellen** aus.

![Registerkarte „Problembehandlung und Support“ des Anmeldeereignisses](./media/troubleshoot-conditional-access/image6.png)

Geben Sie bei der Übermittlung des Vorfalls die Anforderungs-ID sowie Uhrzeit und Datum des Anmeldeereignisses in den Details an. Diese Informationen ermöglichen es dem Microsoft-Support, das betreffende Ereignis zu finden.

### <a name="conditional-access-error-codes"></a>Feldercodes bei bedingtem Zugriff

| Anmeldefehlercode | Fehlerzeichenfolge |
| --- | --- |
| 53000 | DeviceNotCompliant |
| 53001 | DeviceNotDomainJoined |
| 53002 | ApplicationUsedIsNotAnApprovedApp |
| 53003 | BlockedByConditionalAccess |
| 53004 | ProofUpBlockedDueToRisk |

## <a name="next-steps"></a>Nächste Schritte

- [Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal](../reports-monitoring/concept-sign-ins.md)
- [Beheben von Problemen beim bedingten Zugriff mit dem Was-wäre-wenn-Tool](troubleshoot-conditional-access-what-if.md)
- Bewährte Methoden für den [Bedingten Zugriff in Azure Active Directory](best-practices.md)
