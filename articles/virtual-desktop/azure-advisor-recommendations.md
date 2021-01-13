---
title: 'Exemplarische Vorgehensweise zu Azure Advisor für Windows Virtual Desktop: Azure'
description: Vorgehensweise beim Auflösen von Azure Advisor-Empfehlungen für Windows Virtual Desktop.
author: Heidilohr
ms.topic: conceptual
ms.date: 08/28/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 760e0212d2d863e6b869c23c2e523a0e056a28ed
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90033029"
---
# <a name="how-to-resolve-azure-advisor-recommendations"></a>Auflösen von Azure Advisor-Empfehlungen

In diesem Artikel wird beschrieben, wie Sie Empfehlungen auflösen können, die in Azure Advisor für Windows Virtual Desktop angezeigt werden.

## <a name="no-validation-environment-enabled"></a>„Keine Überprüfungsumgebung aktiviert“

>[!div class="mx-imgBorder"]
>![Screenshot der Seite "Azure Advisor: Optimaler Betrieb“. Die Empfehlung „Keine Überprüfungsumgebung aktiviert“ wird rot hervorgehoben.](media/no-validation-environment.png)

Diese Empfehlung wird unter „Optimaler Betrieb“ angezeigt. Die Empfehlung sollte auch eine Warnmeldung wie die folgende anzeigen:

„In diesem Abonnement ist keine Überprüfungsumgebung aktiviert. Beim Erstellen Ihrer Hostpools haben Sie auf der Registerkarte „Eigenschaften“ unter „Überprüfungsumgebung“ die Option **Nein** ausgewählt. Um die Geschäftskontinuität durch Windows Virtual Desktop-Dienstbereitstellungen zu gewährleisten, stellen Sie sicher, dass Sie über mindestens einen Hostpool mit einer Überprüfungsumgebung verfügen, in der Sie auf potenzielle Probleme testen können.

Sie können durch Aktivieren einer Überprüfungsumgebung in einem Ihrer Hostpools bewirken, dass diese Warnmeldung nicht mehr angezeigt wird.

So aktivieren Sie eine Überprüfungsumgebung:

1. Navigieren Sie im Azure-Portal zu Ihrer Homepage, und wählen Sie den Hostpool aus, den Sie ändern möchten.

2. Wählen Sie dann den Hostpool aus, den Sie aus einer Produktionsumgebung in eine Überprüfungsumgebung ändern möchten.

3. Wählen Sie in Ihrem Hostpool die Option **Eigenschaften** in der linken Spalte aus. Scrollen Sie anschließend nach unten, bis „Überprüfungsumgebung“ angezeigt wird. Wählen Sie **Ja** und dann **Anwenden** aus.

>[!div class="mx-imgBorder"]
>![Screenshot des Menüs „Eigenschaften“. „Überprüfungsumgebung“ ist rot hervorgehoben, und die Option „Ja“ ist ausgewählt.](media/validation-yes.png)

Diese Änderungen führen nicht dazu, dass die Warnung sofort entfällt, aber sie wird nach einiger Zeit nicht mehr angezeigt werden. Azure Advisor wird zwei Mal täglich aktualisiert. Bis dahin können Sie die Empfehlung manuell auf einen späteren Zeitpunkt verschieben oder verwerfen. Wir empfehlen Ihnen abzuwarten, bis die Empfehlung von selbst nicht mehr angezeigt wird. Auf diese Weise kann Azure Advisor Ihnen mitteilen, ob Probleme auftreten, wenn sich die Einstellungen ändern.

## <a name="not-enough-production-non-validation-environments-enabled"></a>„Nicht genügend Produktionsumgebungen (Nicht-Überprüfungsumgebungen) aktiviert.“

Diese Empfehlung wird unter „Optimaler Betrieb“ angezeigt.

Für diese Empfehlung wird die Warnmeldung aus einem der folgenden Gründe angezeigt:

- In ihrer Überprüfungsumgebung sind zu viele Hostpools vorhanden.
- Sie verfügen über keine Produktionshostpools.

Es wird empfohlen, dass Benutzer weniger als die Hälfte ihrer Hostpools in einer Überprüfungsumgebung verwenden.

So beheben Sie diese Warnung:

1. Navigieren Sie im Azure-Portal zu Ihrer Homepage.

2. Wählen Sie die Hostpools aus, die aus Überprüfung in Produktion geändert werden sollen.

3. Wählen Sie in Ihrem Hostpool die Registerkarte **Eigenschaften** in der Spalte auf der rechten Seite des Bildschirms aus. Scrollen Sie anschließend nach unten, bis „Überprüfungsumgebung“ angezeigt wird. Wählen Sie **Nein** und dann **Anwenden** aus.

>[!div class="mx-imgBorder"]
>![Screenshot des Menüs „Eigenschaften“. „Überprüfungsumgebung“ ist rot hervorgehoben, und die Option „Nein“ ist ausgewählt.](media/validation-no.png)

Diese Änderungen führen nicht dazu, dass die Warnung sofort entfällt, aber sie wird nach einiger Zeit nicht mehr angezeigt werden. Azure Advisor wird zwei Mal täglich aktualisiert. Bis dahin können Sie die Empfehlung manuell auf einen späteren Zeitpunkt verschieben oder verwerfen. Wir empfehlen Ihnen abzuwarten, bis die Empfehlung von selbst nicht mehr angezeigt wird. Auf diese Weise kann Azure Advisor Ihnen mitteilen, ob Probleme auftreten, wenn sich die Einstellungen ändern.

## <a name="not-enough-links-are-unblocked-to-successfully-implement-your-vm"></a>„Es werden nicht genügend Links freigegeben, um Ihre VM erfolgreich zu implementieren.“

Diese Empfehlung wird unter „Optimaler Betrieb“ angezeigt.

Sie müssen die Blockierung bestimmter URLs aufheben, um sicherzustellen, dass Ihr virtueller Computer (VM) ordnungsgemäß funktioniert. Die Liste wird in der [Liste der sicheren URLs](safe-url-list.md) angezeigt. Wenn die Blockierung der URLs nicht aufgehoben wird, funktioniert die VM nicht ordnungsgemäß.

Um diese Empfehlung aufzulösen, stellen Sie sicher, dass Sie die Blockierung aller URLs in der [Liste der sicheren URLs](safe-url-list.md) aufheben. Sie können auch das Diensttag oder FQDN-Tags verwenden, um die Blockierung von URLs aufzuheben.

## <a name="propose-new-recommendations"></a>Vorschlagen neuer Empfehlungen

Sie können uns helfen, Azure Advisor zu verbessern, indem Sie Vorschläge für Empfehlungen einreichen. Ihre Empfehlung könnte einem anderen Benutzer in einer schwierigen Lage unterstützen. Um einen Vorschlag einzureichen, besuchen Sie [unser UserVoice-Forum](https://windowsvirtualdesktop.uservoice.com/forums/930847-azure-advisor-recommendations), und füllen Sie das Übermittlungsformular aus. Wenn Sie das Formular ausfüllen, stellen Sie sicher, dass Sie so viele Details wie möglich angeben.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie nach ausführlicheren Anleitungen zur Lösung häufiger Probleme suchen, sehen Sie sich [Übersicht zu Problembehandlung, Feedback und Support für Windows Virtual Desktop](troubleshoot-set-up-overview.md) an.
