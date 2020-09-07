---
title: Ressourcenbestand in Azure Security Center
description: Erfahren Sie mehr über die Ressourcenverwaltungsumgebung von Azure Security Center, die Ihnen vollständigen Einblick in alle von Azure Security Center überwachten Ressourcen bietet.
author: memildin
manager: rkarlin
services: security-center
ms.author: memildin
ms.date: 08/11/2020
ms.service: security-center
ms.topic: conceptual
ms.openlocfilehash: f3a542cd62c3d593dbc0cce7982d47222e9a7c88
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89181102"
---
# <a name="explore-and-manage-your-resources-with-asset-inventory-and-management-tools"></a>Untersuchen und Verwalten Ihrer Ressourcen mit dem Ressourcenbestand und Verwaltungstools

Auf der Seite „Ressourcenbestand“ von Azure Security Center können Sie auf einer Seite den gesamten Sicherheitsstatus der Ressourcen anzeigen, die Sie mit Azure Security Center verbunden haben. 

Azure Security Center analysiert in regelmäßigen Abständen den Sicherheitsstatus der Azure-Ressourcen, um mögliche Sicherheitsrisiken zu erkennen. Anschließend erhalten Sie Empfehlungen dazu, wie Sie diese Sicherheitsrisiken beheben können.

Wenn eine Ressource ausstehende Empfehlungen hat, werden diese im Inventar angezeigt.

Verwenden Sie diese Ansicht und ihre Filter zur Beantwortung folgender Fragen:

- Für welche meiner Abonnements in Standardtarifen stehen Empfehlungen aus?
- Auf welchen meiner Computer mit dem Tag „Produktion“ fehlt der Log Analytics-Agent?
- Für wie viele meiner Computer, die mit einem bestimmten Tag gekennzeichnet sind, stehen Empfehlungen aus?
- Wie viele Ressourcen in einer bestimmten Ressourcengruppe weisen Sicherheitsergebnisse von einem Dienst zur Sicherheitsrisikobewertung auf?

Die Ressourcenverwaltungsfunktionen dieses Tools sind umfassend und wachsen weiter. 


## <a name="availability"></a>Verfügbarkeit

|Aspekt|Details|
|----|:----|
|Status des Release:|Vorschau|
|Preise:|Kostenlos|
|Erforderliche Rollen und Berechtigungen:|Alle Benutzer|
|Clouds:|![Ja](./media/icons/yes-icon.png) Kommerzielle Clouds<br>![Nein](./media/icons/no-icon.png) National/Sovereign (US Gov, China Gov, andere Gov)|
|||


## <a name="what-are-the-key-features-of-asset-inventory"></a>Welche sind die wichtigsten Features des Ressourcenbestands?

Auf der Bestandsseite werden die folgenden Tools angezeigt:

- **Zusammenfassungen** – Bevor Sie Filter definieren, wird oben in der Bestandsansicht ein auffälliger Streifen mit Werten angezeigt:

    - **Ressourcen gesamt**: Die Gesamtanzahl der mit Azure Security Center verbundenen Ressourcen.
    - **Fehlerhafte Ressourcen:** Ressourcen mit aktiven Sicherheitsempfehlungen. [Weitere Informationen zu Sicherheitsempfehlungen](https://docs.microsoft.com/azure/security-center/security-center-recommendations).
    - **Nicht überwachte Ressourcen**: Ressourcen mit Agent-Überwachungsproblemen – Der Log Analytics-Agent wurde bereitgestellt, aber der Agent sendet keine Daten oder hat andere Integritätsprobleme.

- **Filter** – Die zahlreichen Filter am oberen Rand der Seite bieten eine Möglichkeit, die Liste der Ressourcen schnell entsprechend der Frage zu verfeinern, die Sie zu beantworten versuchen. Wenn Sie beispielsweise die Frage *Auf welchen meiner Computer mit dem Tag „Produktion“ fehlt der Log Analytics-Agent?* beantworten möchten, könnten Sie den Filter **Agent-Überwachung** mit dem Filter **Tags** kombinieren, wie im folgenden Clip gezeigt:

    ![Filtern nach Produktionsressourcen, die nicht überwacht werden](./media/asset-inventory/filtering-to-prod-unmonitored.gif)

    Sobald Sie Filter angewendet haben, werden die Zusammenfassungswerte aktualisiert und an die Abfrageergebnisse angepasst. 

- **Exportoptionen** – Der Ressourcenbestand bietet die Möglichkeit, die Ergebnisse der ausgewählten Filteroptionen in eine CSV-Datei zu exportieren. Darüber hinaus können Sie die Abfrage selbst in den Azure Resource Graph-Explorer exportieren, um die KQL-Abfrage weiter zu verfeinern, zu speichern oder zu modifizieren.

    ![Exportoptionen für Ressourcenbestand](./media/asset-inventory/inventory-export-options.png)

    > [!TIP]
    > Die KQL-Dokumentation bietet eine Datenbank mit einigen Beispieldaten zusammen mit einigen einfachen Abfragen, um ein „Gefühl“ für die Sprache zu bekommen. [Weitere Informationen finden Sie in diesem KQL-Tutorial](https://docs.microsoft.com/azure/data-explorer/kusto/query/tutorial?pivots=azuredataexplorer).

- **Ressourcenverwaltungsoptionen**Mit dem Ressourcenbestand können Sie komplexe Suchabfragen durchführen. Wenn Sie die Ressourcen gefunden haben, die Ihren Abfragen entsprechen, bietet der Ressourcenbestand Verknüpfungen für Vorgänge wie z. B.:

    - Zuweisen von Tags zu den gefilterten Ressourcen: Aktivieren Sie die Kontrollkästchen neben den Ressourcen, die Sie mit einem Tag versehen möchten.
    - Onboarding neuer Server in Security Center: Verwenden Sie die Symbolleistenschaltfläche **Nicht-Azure-Server hinzufügen**


## <a name="how-does-asset-inventory-work"></a>Wie funktioniert der Ressourcenbestand?

Der Ressourcenbestand verwendet [Azure Resource Graph (ARG)](https://docs.microsoft.com/azure/governance/resource-graph/), einen Azure-Dienst, der die Möglichkeit bietet, die Daten zum Sicherheitsstatus von Security Center über mehrere Abonnements hinweg abzufragen.

ARG ist so konzipiert, dass es eine effiziente Ressourcenerkundung mit der Fähigkeit zur bedarfsorientierten Abfrage ermöglicht.

Mit Hilfe der [Kusto Query Language (KQL)](https://docs.microsoft.com/azure/data-explorer/kusto/query/) kann der Ressourcenbestand Ihnen durch Querverweise zwischen ASC-Daten und anderen Ressourceneigenschaften schnell tiefe Einblicke verschaffen.


## <a name="how-to-use-asset-inventory"></a>Verwenden des Ressourcenbestands

1. Wählen Sie auf der Security Center-Randleiste **Ressourcenbestands** aus.

1. Um eine bestimmte Ressource anzuzeigen, geben Sie optional den Namen in das Feld **Nach Namen filtern** ein.

1. Wählen Sie die entsprechenden Optionen in den Filtern aus, um die spezifische Abfrage zu erstellen, die Sie ausführen möchten.

    ![Filter des Ressourcenbestands](./media/asset-inventory/inventory-filters.png)

    Standardmäßig werden die Ressourcen nach der Anzahl aktiver Sicherheitsempfehlungen sortiert.

    > [!IMPORTANT]
    > Die Optionen in jedem Filter sind spezifisch für die Ressourcen in den aktuell ausgewählten Abonnements **und** Ihre Auswahl in den anderen Filtern.
    >
    > Wenn Sie z. B. nur ein Abonnement ausgewählt haben und das Abonnement keine Ressourcen mit ausstehenden Sicherheitsempfehlungen aufweist, die zu beheben sind (0 fehlerhafte Ressourcen), enthält der Filter **Empfehlungen** keine Optionen. 

1. Um den Filter **Sicherheitsergebnisse enthalten** zu verwenden, geben Sie freien Text aus der ID, der Sicherheitsprüfung oder dem CVE-Namen eines Sicherheitsergebnisses ein, um nach den betroffenen Ressourcen zu filtern:

    ![Filter „Sicherheitsergebnisse enthalten“](./media/asset-inventory/security-findings-contain-elements.png)

    > [!TIP]
    > Die Filter **Sicherheitsergebnisse enthalten** und **Tags** akzeptieren nur einen einzigen Wert. Um nach mehreren Werten zu filtern, verwenden Sie **Filter hinzufügen**.

1. Um den Filter **Tarif** zu verwenden, wählen Sie eine oder mehrere Optionen („Free“, „Partial“ oder „Standard“) aus:

    - **Free**: Ressourcen im Free-Tarif
    - **Standard**: Ressourcen im Standard-Tarif
    - **Partial**: Dies gilt für Abonnements im Tarif „Standard“, für die aber einige optionale Sicherheitspläne deaktiviert sind. Für das folgende Abonnement im Standard-Tarif sind beispielsweise fünf Elemente des Standard-Tarifs deaktiviert. 

        ![Abonnement im Standard (Partial)-Tarif](./media/asset-inventory/pricing-tier-partial.png)

1. Um die Ergebnisse der Abfrage weiter zu untersuchen, wählen Sie die Ressourcen aus, die für Sie von Interesse sind.

1. Wählen Sie optional **Im Resource Graph-Explorer anzeigen** aus, um die Abfrage im Resource Graph-Explorer zu öffnen.

    ![Ressourcenbestandabfrage in ARG](./media/asset-inventory/inventory-query-in-resource-graph-explorer.png)

1. Wenn Sie einige Filter definiert und die Seite offen gelassen haben, aktualisiert Security Center die Ergebnisse nicht automatisch. Änderungen an den Ressourcen wirken sich nicht auf die angezeigten Ergebnisse aus, es sei denn, Sie laden die Seite manuell neu oder wählen **Aktualisieren** aus.


## <a name="faq---inventory"></a>Häufig gestellte Fragen: Ressourcenbestand

### <a name="why-arent-all-of-my-subscriptions-machines-storage-accounts-etc-shown"></a>Warum werden nicht alle meine Abonnements, Computer, Speicherkonten usw. angezeigt?

In der Ansicht „Ressourcenbestand“ werden Ihre Ressourcen aus der Perspektive von Cloud Security Posture Management (CSPM) aufgelistet. Die Filter geben nicht jede Ressource in Ihrer Umgebung zurück, sondern nur diejenigen mit ausstehenden (oder „aktiven“) Empfehlungen. 

Wenn Sie z. B. über neun Abonnements verfügen, aber nur für acht liegen derzeit Empfehlungen vor, werden beim Filtern nach **Ressourcentyp = Abonnements** nur die acht Abonnements mit aktiven Empfehlungen angezeigt:

![Es werden nicht alle Abonnements zurückgegeben, wenn keine aktiven Empfehlungen vorliegen](./media/asset-inventory/filtered-subscriptions-some.png)


### <a name="why-do-some-of-my-resources-show-blank-values-in-the-pricing-or-agent-monitoring-columns"></a>Warum werden für einige meiner Ressourcen in den Spalten für Preise oder Agent-Überwachung keine Werte angezeigt?

Nicht alle von Security Center überwachten Ressourcen verfügen über Agents. Dies trifft beispielsweise auf Azure Storage-Konten oder PaaS-Ressourcen wie Datenträger, Logic Apps, Data Lake-Analysen und Event Hub zu.

Wenn Preise oder Agent-Überwachung für eine Ressource nicht relevant sind, wird in diesen Ressourcenbestandspalten nichts angezeigt.

![Für einige Ressourcen werden keine Informationen in den Spalten für Preise oder Agent-Überwachung angezeigt.](./media/asset-inventory/agent-pricing-blanks.png)



## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde die Seite „Ressourcenbestand“ von Azure Security Center beschrieben.

Weitere Informationen zu verwandten Tools finden Sie auf den folgenden Seiten:

- [Azure Resource Graph (ARG)](https://docs.microsoft.com/azure/governance/resource-graph/)

- [Kusto-Abfragesprache (KQL)](https://docs.microsoft.com/azure/data-explorer/kusto/query/)