---
title: Verwenden von Unterdrückungsregeln für Warnungen, um falsch positive oder andere unerwünschte Sicherheitswarnungen in Azure Security Center zu unterdrücken.
description: In diesem Artikel wird erläutert, wie Sie mithilfe der Unterdrückungsregeln von Azure Security Center unerwünschte Sicherheitswarnungen ausblenden können.
author: memildin
manager: rkarlin
services: security-center
ms.author: memildin
ms.date: 05/04/2020
ms.service: security-center
ms.topic: conceptual
ms.openlocfilehash: 341373c9a8429f335f3064db7a94973d34e0ca1c
ms.sourcegitcommit: 1a0dfa54116aa036af86bd95dcf322307cfb3f83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2020
ms.locfileid: "88042499"
---
# <a name="suppress-alerts-from-azure-security-centers-threat-protection"></a>Unterdrücken von Warnungen aus dem Threat Protection-Modul von Azure Security Center

Auf dieser Seite wird erläutert, wie Sie mithilfe von Unterdrückungsregeln für Warnungen falsch positive oder andere unerwünschte Sicherheitswarnungen in Azure Security Center unterdrücken können.

## <a name="availability"></a>Verfügbarkeit

|Aspekt|Details|
|----|:----|
|Status des Release:|Vorschau|
|Preise:|Free-Tarif<br>(Die meisten Sicherheitswarnungen gelten nur für den Standard-Tarif)|
|Erforderliche Rollen und Berechtigungen:|**Sicherheitsadministrator** und **Besitzer** können Regeln erstellen und löschen.<br>**Sicherheitsleseberechtigter** und **Leser** können Regeln anzeigen.|
|Clouds:|![Ja](./media/icons/yes-icon.png) Kommerzielle Clouds<br>![Ja](./media/icons/yes-icon.png) National/Sovereign (US Gov, China Gov, andere Gov)|
|||




## <a name="what-are-suppression-rules"></a>Was sind Warnungsunterdrückungsregeln?

Die Threat Protection-Komponenten von Azure Security Center erkennen Bedrohungen in allen Bereichen Ihrer Umgebung und generieren Sicherheitswarnungen.

Wenn eine einzelne Warnung nicht interessant oder relevant ist, können Sie sie manuell verwerfen. Alternativ können Sie die Funktion „Unterdrückungsregeln“ verwenden, um ähnliche Warnungen zukünftig automatisch zu verwerfen. In der Regel verwenden Sie eine Unterdrückungsregel in folgenden Fällen:

- Unterdrücken von Warnungen, die Sie als falsch positiv identifiziert haben

- Unterdrücken von Warnungen, die zu oft ausgelöst werden und daher nicht nützlich sind

Mit Unterdrückungsregeln definieren Sie die Kriterien, nach denen Warnungen automatisch verworfen werden sollen.

> [!CAUTION]
> Durch Unterdrücken von Sicherheitswarnungen wird der Bedrohungsschutz von Security Center verringert. Sie sollten die potenziellen Auswirkungen der Unterdrückungsregeln sorgfältig überprüfen und diese über einen bestimmten Zeitraum überwachen.

![Warnungsunterdrückungsregel erstellen](media\alerts-suppression-rules\create-suppression-rule.gif)

## <a name="create-a-suppression-rule"></a>Erstellen einer Unterdrückungsregel

Es gibt einige Möglichkeiten, wie Sie Regeln zum Unterdrücken unerwünschter Sicherheitswarnungen erstellen können:

- Warnungen auf Verwaltungsgruppenebene können Sie mithilfe von Azure Policy unterdrücken.

- Um Warnungen auf Abonnementebene zu unterdrücken, können Sie wie im Folgenden erläutert das Azure-Portal oder die REST-API verwenden.

Mit Unterdrückungsregeln können nur Warnungen verworfen werden, die für die ausgewählten Abonnements bereits ausgelöst wurden.

So erstellen Sie eine Regel direkt im Azure-Portal

1. Auf der Security Center-Seite für Sicherheitswarnungen:

    - Suchen Sie die spezifische Warnung, die nicht mehr angezeigt werden soll, und wählen Sie im Menü mit den Auslassungspunkten (...) für die Warnung die Option **Create suppression rule** (Unterdrückungsregel erstellen) aus:

        [![Option **Create suppression rule**](media/alerts-suppression-rules/auto-dismiss-future-option.png)](media/alerts-suppression-rules/auto-dismiss-future-option.png#lightbox) (Unterdrückungsregel erstellen)

    - Oder wählen Sie oben auf der Seite den Link **Unterdrückungsregeln** und dann auf der Seite der Unterdrückungsregeln die Option **Neue Unterdrückungsregel erstellen** aus:

        ![Schaltfläche „Neue Unterdrückungsregel erstellen“](media/alerts-suppression-rules/create-new-suppression-rule.png)

1. Geben Sie im Bereich „Neue Unterdrückungsregel (Vorschau)“ Details zur neuen Regel ein.

    - Mit der Regel kann die Warnung für **alle Ressourcen** verworfen werden, sodass zukünftig keine derartige Regel mehr angezeigt wird. 
    
    - Mit der Regel kann die Warnung **nach bestimmten Kriterien** verworfen werden, z. B. wenn die Warnung sich auf eine bestimmte IP-Adresse, einen Prozessnamen, ein Benutzerkonto, eine Azure-Ressource oder einen Standort bezieht.

    > [!TIP]
    > Wenn Sie die Seite für die neue Regel über eine bestimmte Warnung geöffnet haben, werden die Warnung und das Abonnement automatisch in der neuen Regel konfiguriert. Wenn Sie den Link **Neue Unterdrückungsregel erstellen** verwendet haben, entsprechen die ausgewählten Abonnements dem aktuellen Filter im Portal.

    [![Bereich zum Erstellen von Unterdrückungsregeln](media/alerts-suppression-rules/new-suppression-rule-pane.png)](media/alerts-suppression-rules/new-suppression-rule-pane.png#lightbox)

1. Geben Sie Details zur Regel ein:

    - **Name:** ein Name für die Regel. Regelnamen müssen mit einem Buchstaben oder einer Ziffer beginnen, müssen zwischen 2 und 50 Zeichen lang sein und dürfen keine anderen Symbole als Bindestriche (-) oder Unterstriche (_) enthalten. 
    - **Status:** „Aktiviert“ oder „Deaktiviert“.
    - **Grund:** Wählen Sie einen der integrierten Gründe oder „Sonstiger“ aus, wenn diese Ihren Anforderungen nicht entsprechen.
    - **Ablaufdatum:** ein Ablaufdatum und eine Ablaufzeit für die Regel. Regeln können bis zu sechs Monate lang gelten.

1. Optional können Sie die Regel über die Schaltfläche **Simulate** (Simulieren) testen, um zu sehen, wie viele Warnungen verworfen werden, wenn die Regel aktiv ist.

1. Speichern Sie die Regel. 

## <a name="edit-a-suppression-rules"></a>Bearbeiten einer Unterdrückungsregel

Die erstellten Regeln können Sie auf der Seite der Unterdrückungsregeln bearbeiten.

1. Wählen Sie oben auf der Security Center-Seite für Sicherheitswarnungen den Link **Unterdrückungsregeln** aus.

1. Die Seite der Unterdrückungsregeln wird mit allen Regeln für das ausgewählte Abonnement geöffnet.

    [![Liste der Unterdrückungsregeln](media/alerts-suppression-rules/suppression-rules-page.png)](media/alerts-suppression-rules/suppression-rules-page.png#lightbox)

1. Um eine einzelne Regel zu bearbeiten, öffnen Sie das Menü mit den Auslassungspunkten (...) für die Regel, und wählen Sie **Bearbeiten** aus.

1. Nehmen Sie die gewünschten Änderungen vor, und wählen Sie dann **Anwenden** aus. 

## <a name="delete-a-suppression-rule"></a>Löschen einer Unterdrückungsregel

Wenn Sie eine oder mehrere der erstellten Regeln löschen möchten, verwenden Sie die Seite „Unterdrückungsregeln“.

1. Wählen Sie oben auf der Security Center-Seite für Sicherheitswarnungen den Link **Unterdrückungsregeln** aus.

1. Die Seite der Unterdrückungsregeln wird mit allen Regeln für das ausgewählte Abonnement geöffnet.

1. Um eine einzelne Regel zu löschen, öffnen Sie das Menü mit den Auslassungspunkten (...) für die Regel, und wählen Sie **Löschen** aus.

1. Wenn Sie mehrere Regeln löschen möchten, aktivieren Sie die Kontrollkästchen für die zu löschenden Regeln, und wählen Sie **Löschen** aus.

    ![Löschen einer oder mehrerer Unterdrückungsregeln](media/alerts-suppression-rules/delete-multiple-alerts.png)

## <a name="view-suppressed-alerts"></a>Anzeigen unterdrückter Warnungen

Warnungen, die den aktivierten Unterdrückungsregeln entsprechen, werden zwar weiterhin generiert, ihr Status wird jedoch auf **Abgelehnt** festgelegt. Der Status wird im Azure-Portal oder an dem Ort angezeigt, über den Sie auf die Security Center-Sicherheitswarnungen zugreifen. 

> [!TIP]
> [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/) erstellt keine Vorfälle für unterdrückte Warnungen. Bei anderen SIEMs können Sie unterdrückte Warnungen durch Verwendung des Status der Warnungen („Abgelehnt“) filtern.

Verwenden Sie den Filter in Security Center, um die Warnungen anzuzeigen, die durch die Regeln verworfen wurden.

* Öffnen Sie auf der Security Center-Seite für Sicherheitswarnungen die Filteroptionen, und wählen Sie **Abgelehnt** aus.  

   [![Anzeigen verworfener Warnungen](media/alerts-suppression-rules/view-dismissed-alerts.png)](media/alerts-suppression-rules/view-dismissed-alerts.png#lightbox)


## <a name="create-and-manage-suppression-rules-with-the-api"></a>Erstellen und Verwalten von Unterdrückungsregeln über die API

Sie können Unterdrückungsregeln für Warnungen über die Security Center-REST-API erstellen, anzeigen oder löschen. 

Die relevanten HTTP-Methoden für Unterdrückungsregeln in der REST-API lauten wie folgt:

- **PUT**: Erstellen oder Aktualisieren einer Unterdrückungsregel in einem angegebenen Abonnement

- **GET**:

    - Auflisten aller Regeln, die für ein bestimmtes Abonnement konfiguriert sind. Diese Methode gibt ein Array der anwendbaren Regeln zurück.

    - Abrufen der Details einer spezifischen Regel für ein bestimmtes Abonnement. Diese Methode gibt eine Unterdrückungsregel zurück.

    - Simulieren der Auswirkung einer Unterdrückungsregel in der Entwurfsphase. Mit diesem Aufruf ermitteln Sie, welche der vorhandenen Warnungen verworfen werden, wenn die Regel aktiv ist.

- **DELETE**: Löschen einer vorhandenen Regel (der Status der durch die Regel bereits verworfenen Warnungen wird jedoch nicht geändert).

Ausführliche Informationen und Verwendungsbeispiele finden Sie in der [API-Dokumentation](https://docs.microsoft.com/rest/api/securitycenter/). 


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurden Unterdrückungsregeln in Azure Security Center beschrieben, mit denen unerwünschte Warnungen automatisch verworfen werden.

Weitere Informationen zu Sicherheitswarnungen in Azure Security Center finden Sie auf den folgenden Seiten:

- [Sicherheitswarnungen und Kill Chain für Absichten:](alerts-reference.md) Referenzleitfaden für die Sicherheitswarnungen, die im Threat Protection-Modul von Azure Security Center möglicherweise angezeigt werden.
- [Bedrohungsschutz in Azure Security Center:](threat-protection.md) Beschreibung der zahlreichen Aspekte Ihrer Umgebung, die über das Threat Protection-Modul von Azure Security Center überwacht werden.