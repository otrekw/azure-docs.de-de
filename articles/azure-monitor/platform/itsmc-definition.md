---
title: ITSM-Connector in Log Analytics
description: Dieser Artikel bietet eine Übersicht über den ITSM-Connector (IT-Service-Management-Connector) sowie Informationen zur Verwendung dieser Lösung, um die ITSM-Arbeitselemente in Log Analytics zu überwachen und zu verwalten und um etwaige Probleme schnell zu lösen.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 05/24/2018
ms.custom: references_regions
ms.openlocfilehash: 717a1bc4361ba4a7366f4864c1fe44f93b6f4b5e
ms.sourcegitcommit: 02b1179dff399c1aa3210b5b73bf805791d45ca2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2021
ms.locfileid: "98127854"
---
# <a name="connect-azure-to-itsm-tools-by-using-it-service-management-connector"></a>Verbinden von Azure mit ITSM-Tools mithilfe des ITSM-Connectors

:::image type="icon" source="media/itsmc-overview/itsmc-symbol.png":::

Dieser Artikel bietet Informationen dazu, wie Sie den ITSM-Connector (ITSMC) in Log Analytics konfigurieren, um Arbeitselemente zentral zu verwalten.

## <a name="add-it-service-management-connector"></a>Hinzufügen des ITSM-Connectors

Bevor Sie eine Verbindung herstellen können, müssen Sie den ITSM-Connector hinzufügen.

1. Wählen Sie im Azure-Portal **Ressource erstellen** aus:

   ![Screenshot des Menüelemente „Ressource erstellen“](media/itsmc-overview/azure-add-new-resource.png)

2. Suchen Sie im Azure Marketplace nach **ITSM-Connector**. Wählen Sie **Erstellen** aus:

   ![Screenshot der Schaltfläche „Erstellen“ im Azure Marketplace](media/itsmc-overview/add-itsmc-solution.png)

3. Wählen Sie im Abschnitt **LA-Arbeitsbereich** den Azure Log Analytics-Arbeitsbereich aus, in dem Sie den ITSM-Connector installieren möchten.
   >[!NOTE]
   >
   > * Der ITSM-Connector kann nur in Log Analytics-Arbeitsbereichen in den folgenden Regionen installiert werden: „USA, Osten“, „USA, Westen 2“, „USA, Süden-Mitte“, „USA, Westen-Mitte“, „US Gov Arizona“, „US Gov Virginia“, „Kanada, Mitte“, „Europa, Westen“, „Vereinigtes Königreich, Süden“, „Asien, Südosten“, „Japan, Osten“, „Indien, Mitte“ und „Australien, Südosten“.

4. Wählen Sie im Abschnitt **Log Analytics-Arbeitsbereich** die Ressourcengruppe aus, in der Sie die Ressource für den ITSM-Connector erstellen möchten:

   ![Screenshot des Abschnitts „Log Analytics-Arbeitsbereich“](media/itsmc-overview/itsmc-solution-workspace.png)
   >[!NOTE]
   >Im Rahmen der laufenden Umstellung von der Microsoft Operations Management Suite (OMS) auf Azure Monitor werden OMS-Arbeitsbereiche nun als *Log Analytics-Arbeitsbereiche* bezeichnet.

5. Klicken Sie auf **OK**.

Nachdem die Ressource für den ITSM-Connector bereitgestellt wurde, wird rechts oben im Fenster eine Benachrichtigung angezeigt.

## <a name="create-an-itsm-connection"></a>Erstellen einer ITSM-Verbindung

Nachdem Sie den ITSM-Connector installiert haben, können Sie eine Verbindung herstellen.

Um eine Verbindung herzustellen, müssen Sie Ihr ITSM-Tool vorbereiten, damit die Verbindung vom ITSM-Connector aus möglich ist.  

Wählen Sie basierend auf dem ITSM-Produkt, mit dem Sie eine Verbindung herstellen, einen der folgenden Links aus, um Anweisungen zu erhalten:

- [ServiceNow](./itsmc-connections-servicenow.md)
- [System Center Service Manager](./itsmc-connections-scsm.md)
- [Cherwell](./itsmc-connections-cherwell.md)
- [Provance](./itsmc-connections-provance.md)

Nachdem Sie Ihre ITSM-Tools vorbereitet haben, führen Sie diese Schritte aus, um eine Verbindung herzustellen:

1. Suchen Sie in **Alle Ressourcen** nach **ServiceDesk(*IhrArbeitsbereichsname*)** :

   ![Screenshot der zuletzt verwendeten Ressourcen im Azure-Portal](media/itsmc-definition/create-new-connection-from-resource.png)

1. Wählen Sie im linken Bereich unter **Arbeitsbereichsdatenquellen** die Option **ITSM-Verbindungen** aus:

   ![Screenshot des Menüelements „ITSM-Verbindungen“](media/itsmc-overview/add-new-itsm-connection.png)
1. Wählen Sie **Verbindung hinzufügen** aus.

1. Geben Sie die Verbindungseinstellungen wie beschrieben je nach ITSM-Produkten/-Diensten an:

    - [ServiceNow](./itsmc-connections-servicenow.md)
    - [System Center Service Manager](./itsmc-connections-scsm.md)
    - [Cherwell](./itsmc-connections-cherwell.md)
    - [Provance](./itsmc-connections-provance.md)

   > [!NOTE]
   >
   > Standardmäßig aktualisiert der ITSM-Connector die Konfigurationsdaten der Verbindung einmal alle 24 Stunden. Um die Daten Ihrer Verbindung bei von Ihnen vorgenommenen Änderungen oder Vorlagenupdates sofort zu aktualisieren, wählen Sie auf dem Blatt Ihrer Verbindung die Schaltfläche **Synchronisieren** aus:
   >
   > ![Screenshot der Schaltfläche „Synchronisieren“ auf dem Verbindungsblatt](media/itsmc-overview/itsmc-connections-refresh.png)

## <a name="use-itsmc"></a>Verwenden des ITSM-Connectors

   Mithilfe des ITSM-Connectors können Sie Warnungen im ITSM-Tool aus Azure Monitor-Warnungen erstellen.

## <a name="create-itsm-work-items-from-azure-alerts"></a>Erstellen von ITSM-Arbeitselementen aus Azure-Warnungen

Nachdem Sie die ITSM-Verbindung hergestellt haben, können Sie in Ihrem ITSM-Tool Arbeitselemente basierend auf Azure-Warnungen erstellen. Sie verwenden für das Erstellen der Arbeitselemente die ITSM-Aktion in Aktionsgruppen.

Die wiederverwendbaren Aktionsgruppen bieten Ihnen die Möglichkeit, modular Aktionen für Ihre Azure-Warnungen auszulösen. Sie können Aktionsgruppen im Azure-Portal mit Metrikwarnungen, Aktivitätsprotokollwarnungen und Azure Log Analytics-Warnungen verwenden.

> [!NOTE]
> Nachdem Sie die ITSM-Verbindung hergestellt haben, müssen Sie 30 Minuten warten, bis der Synchronisierungsvorgang abgeschlossen ist.

### <a name="template-definitions"></a>Vorlagendefinitionen

   Hierbei handelt es sich um Typen von Arbeitselementen, die vom ITSM-Tool definierte Vorlagen verwenden können.
Mithilfe von Vorlagen können Sie Felder definieren, die anhand von im Rahmen der Aktionsgruppe definierten festgelegten Werten automatisch aufgefüllt werden. Sie definieren Vorlagen im ITSM-Tool.
Sie können definieren, welche Vorlage Sie als Teil der Aktionsgruppendefinition verwenden möchten.

Gehen Sie folgendermaßen vor, um Aktionsgruppen zu erstellen:

1. Wählen Sie im Azure-Portal **Warnungen** aus.
2. Wählen Sie im Menü oben auf dem Bildschirm **Aktionen verwalten** aus:

    ![Screenshot des Menüelements „Aktionen verwalten“](media/itsmc-overview/action-groups-selection-big.png)

   Das Fenster **Aktionsgruppe erstellen** wird angezeigt.

3. Wählen Sie das **Abonnement** und die **Ressourcengruppe** aus, in dem bzw. der Sie die Aktionsgruppe erstellen möchten. Geben Sie einen **Aktionsgruppennamen** und einen **Anzeigenamen** für Ihre Aktionsgruppe an. Klicken Sie auf **Weiter: Benachrichtigungen**.

    ![Screenshot des Fensters „Aktionsgruppe erstellen“](media/itsmc-overview/action-groups-details.png)

4. Wählen Sie in der Benachrichtigungsliste Folgendes aus: **Weiter: Aktionen**.
5. Wählen Sie in der Aktionsliste in der Liste **Aktionstyp** den Eintrag **ITSM** aus. Geben Sie einen **Namen** für die Aktion an. Wählen Sie die Stiftschaltfläche aus, über die Sie die **Details bearbeiten** können.

    ![Screenshot einer Aktionsgruppendefinition](media/itsmc-definition/action-group-pen.png)

6. Wählen Sie in der Liste **Abonnement** das Abonnement aus, in dem sich Ihr Log Analytics-Arbeitsbereich befindet. Wählen Sie in der Liste **Verbindung** den Namen Ihres ITSM-Connectors aus. Auf diesen folgt der Name Ihres Arbeitsbereichs. Beispiel: „MeinITSM-Connector(MeinArbeitsbereich)“.

7. Wählen Sie einen Typ für das **Arbeitselement** aus.

8. Wenn Sie vordefinierte Felder mit festen Werten ausfüllen möchten, wählen Sie **Benutzerdefinierte Vorlage verwenden** aus. Wählen Sie andernfalls in der Liste **Vorlagen** eine vorhandene [Vorlage](#template-definitions) aus, und geben Sie die festen Werte in den Vorlagenfeldern ein.

9. Im letzten Abschnitt für die Definition der ITSM-Aktionsgruppe können Sie angeben, wie viele Arbeitselemente für die einzelnen Warnungen definiert werden sollen.

    >[!NOTE]
    >
    > * Dieser Abschnitt ist nur für Protokollsuchwarnungen relevant.
    > * Für alle anderen Warnungstypen wird pro Warnung ein Arbeitselement erstellt.

    * Bei Auswahl von „Incident“ oder „Warnung“ in der Dropdownliste „Arbeitselement“: ![Screenshot: ITSM-Fenster mit „Incident“](media/itsmc-overview/itsm-action-configuration.png)
        * Wenn Sie das Kontrollkästchen **Einzelne Arbeitselemente für jedes Konfigurationselement erstellen** aktivieren, wird für jedes Konfigurationselement in jeder Warnung ein neues Arbeitselement erstellt. Da mehrere Warnungen für die gleichen Konfigurationselemente betroffen sind, ist für jedes Konfigurationselement mehr als ein Arbeitselement vorhanden.

             Beispiel:
             1) Warnung 1 mit drei Konfigurationselementen: A, B, C: Drei Arbeitselemente werden erstellt.
             2) Warnung 2 mit einem Konfigurationselement: A: Ein Arbeitselement wird erstellt.

        * Wenn Sie das Kontrollkästchen **Einzelne Arbeitselemente für jedes Konfigurationselement erstellen** deaktivieren, wird vom ITSM-Connector nur ein Arbeitselement für jede Warnungsregel erstellt und an alle betroffenen Konfigurationselemente angefügt. Ein neues Arbeitselement wird erstellt, falls das vorherige geschlossen wurde.

        >[!NOTE]
        > In diesem Fall werden für einige ausgelöste Warnungen im ITSM-Tool keine neuen Arbeitselemente generiert.

        Beispiel:
         1) Warnung 1 mit drei Konfigurationselementen: A, B, C: Ein Arbeitselement wird erstellt.
         2) Warnung 2 für dieselbe Warnungsregel wie in Schritt „a“ mit einem Konfigurationselement: D: D wird an die betroffene Liste mit den Konfigurationselementen des Arbeitselements angefügt, das in Schritt „a“ erstellt wurde.
         3) Warnung 3 für eine andere Warnungsregel mit einem Konfigurationselement: E: Ein Arbeitselement wird erstellt.

    * Bei Auswahl von „Ereignis“ in der Dropdownliste „Arbeitselement“: ![Screenshot des ITSM-Fensters mit „Ereignis“](media/itsmc-overview/itsm-action-configuration-event.png)

        * Bei Auswahl von **Create individual work items for each Log Entry (Configuration item field is not filled. Can result in large number of work items.)** (Einzelne Arbeitselemente für jeden Protokolleintrag erstellen (Feld für Konfigurationselement ist nicht ausgefüllt. Dies kann zu einer großen Zahl von Arbeitselementen führen.)) im Abschnitt mit den Optionsfeldern wird in den Suchergebnissen der Abfrage der Protokollsuchwarnungen pro Zeile ein Arbeitselement erstellt. Die Beschreibungseigenschaft in den Nutzdaten des Arbeitselements enthält die Zeile aus den Suchergebnissen.
        * Wenn Sie im Abschnitt mit den Optionsfeldern die Option **Einzelne Arbeitselemente für jedes Konfigurationselement erstellen** auswählen, erhält jedes Konfigurationselement jeder Warnung ein neues Arbeitselement. Im ITSM-System kann mehr als ein Arbeitselement pro Konfigurationselement vorhanden sein. Dies entspricht der Aktivierung des Kontrollkästchens im Abschnitt „Incident/Warnung“.

10. Klicken Sie auf **OK**.

Verwenden Sie beim Erstellen oder Bearbeiten einer Azure-Warnungsregel eine Aktionsgruppe mit einer ITSM-Aktion. Beim Auslösen der Warnung wird das Arbeitselement im ITSM-Tool erstellt bzw. aktualisiert.

> [!NOTE]
>
>- Informationen zu den Preisen für ITSM-Aktionen finden Sie auf der [Seite mit der Preisübersicht](https://azure.microsoft.com/pricing/details/monitor/) für Aktionsgruppen.
>
>
>- Das Feld für Kurzbeschreibungen in der Warnungsregeldefinition ist auf 40 Zeichen begrenzt, wenn Sie es über eine ITSM-Aktion senden.

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung in ITSM-Connector](./itsmc-resync-servicenow.md)
