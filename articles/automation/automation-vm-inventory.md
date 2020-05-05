---
title: Verwalten eines virtuellen Azure-Computers mit Bestandserfassung | Microsoft-Dokumentation
description: Verwalten eines virtuellen Computers mit Bestandserfassung
services: automation
ms.subservice: change-inventory-management
keywords: Inventar, Inventur, Bestand, Automatisierung, Änderung, Nachverfolgung
ms.date: 01/28/2020
ms.topic: conceptual
ms.openlocfilehash: 0627d2daa70c276535dc43b722e22e1d73b0c8d2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81617380"
---
# <a name="manage-an-azure-virtual-machine-with-inventory-collection"></a>Verwalten eines virtuellen Azure-Computers mit Bestandserfassung

Sie können die Bestandsnachverfolgung für einen virtuellen Azure-Computer auf der Ressourcenseite des virtuellen Computers aktivieren. Sie können die folgenden Bestandsinformationen auf Ihren Computern sammeln und anzeigen:

- Windows-Software (Windows-Anwendungen und Windows-Updates), -Dienste, -Dateien und -Registrierungsschlüssel
- Linux-Software (Pakete), -Daemons und -Dateien

Diese Methode stellt eine browserbasierte Benutzeroberfläche zum Einrichten und Konfigurieren der Bestandserfassung bereit.

## <a name="before-you-begin"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/).

In diesem Artikel wird davon ausgegangen, dass Sie über einen virtuellen Computer verfügen, um die Lösung darauf zu konfigurieren. Falls Sie noch nicht über einen virtuellen Azure-Computer verfügen, können Sie einen [virtuellen Computer erstellen](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="enable-inventory-collection-from-the-virtual-machine-resource-page"></a>Aktivieren der Bestandserfassung über die Ressourcenseite des virtuellen Computers

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Virtuelle Computer**.
2. Wählen Sie in der Liste mit den virtuellen Computern eine VM aus.
3. Wählen Sie im Menü **Ressource** unter **Vorgänge** die Option **Inventory**.
4. Wählen Sie einen Log Analytics-Arbeitsbereich aus, in dem Ihre Datenprotokolle gespeichert werden sollen.
    Wenn für diese Region kein Arbeitsbereich verfügbar ist, werden Sie aufgefordert, einen Standardarbeitsbereich und ein Automation-Konto zu erstellen.
5. Wählen Sie die Option **Aktivieren**, um das Onboarding für Ihren Computer zu starten.

   ![Anzeigen von Onboardingoptionen](./media/automation-vm-inventory/inventory-onboarding-options.png)

    Sie werden in einer Statusleiste benachrichtigt, dass die Lösung aktiviert wird. Dieser Vorgang kann bis zu 15 Minuten dauern. Während dieser Zeit können Sie das Fenster schließen oder geöffnet lassen. Sie werden benachrichtigt, wenn die Lösung aktiviert wurde. Sie können den Bereitstellungsstatus über den Benachrichtigungsbereich überwachen.

   ![Anzeigen der Inventarlösung direkt nach dem Onboarding](./media/automation-vm-inventory/inventory-onboarded.png)

Nachdem die Bereitstellung abgeschlossen ist, wird die Statusleiste nicht mehr angezeigt. Es werden weiterhin Bestandsdaten gesammelt, und es kann sein, dass die Daten noch nicht sichtbar sind. Eine vollständige Sammlung der Daten kann 24 Stunden dauern.

## <a name="configure-your-inventory-settings"></a>Konfigurieren der Inventareinstellungen

Standardmäßig sind Software, Windows-Dienste und Linux-Daemons für die Sammlung konfiguriert. Konfigurieren Sie die Einstellungen der Bestandserfassung, um auch Bestandsdaten zur Windows-Registrierung und zu den Dateien zu sammeln.

1. Klicken Sie oben auf der Seite „Bestand“ auf **Einstellungen bearbeiten**.
2. Navigieren Sie zum Hinzufügen einer neuen Sammlungseinstellung zu der Einstellungskategorie, die Sie hinzufügen möchten, indem Sie die Registerkarte **Windows-Registrierung**, **Windows-Dateien** oder **Linux-Dateien** auswählen.
3. Wählen Sie die entsprechende Kategorie aus, und klicken Sie oben auf der Seite auf **Hinzufügen**.

Die folgenden Tabellen enthalten Informationen zu den einzelnen Eigenschaften, die für die verschiedenen Kategorien konfiguriert werden können.

### <a name="windows-registry"></a>Windows-Registrierung

|Eigenschaft  |BESCHREIBUNG  |
|---------|---------|
|Aktiviert     | Bestimmt, ob die Einstellung angewendet wird        |
|Item Name     | Anzeigename der nachzuverfolgenden Datei        |
|Group     | Ein Gruppenname für die logische Gruppierung von Dateien        |
|Windows-Registrierungsschlüssel   | Der zu überprüfende Pfad für die Datei. Beispiel: "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup"      |

### <a name="windows-files"></a>Windows-Dateien

|Eigenschaft  |BESCHREIBUNG  |
|---------|---------|
|Aktiviert     | TRUE, wenn die Einstellung angewendet wird, andernfalls FALSE.        |
|Item Name     | Anzeigename der nachzuverfolgenden Datei.        |
|Group     | Gruppenname für die logische Gruppierung von Dateien       |
|Pfad eingeben     | Der zu überprüfende Pfad für die Datei, z. B. **c:\temp\meinedatei.txt**.

### <a name="linux-files"></a>Linux-Dateien

|Eigenschaft  |BESCHREIBUNG  |
|---------|---------|
|Aktiviert     | TRUE, wenn die Einstellung angewendet wird, andernfalls FALSE.        |
|Item Name     | Anzeigename der nachzuverfolgenden Datei.        |
|Group     | Gruppenname für die logische Gruppierung von Dateien        |
|Pfad eingeben     | Der zu überprüfende Pfad für die Datei, z. B. **/etc/*.conf**.       |
|Pfadtyp     | Der Typ des Elements, das nachverfolgt werden soll. Werte sind „Datei“ und „Verzeichnis“.        |
|Rekursion     | TRUE, wenn ob beim Suchen nach dem nachzuverfolgenden Element die Rekursion verwendet wird, andernfalls FALSE.        |
|Sudo verwenden     | TRUE, wenn sudo bei der Suche nach dem Element verwendet wird, andernfalls FALSE.         |
|Links     | Wert, der angibt, wie symbolische Verknüpfungen beim Durchlaufen von Verzeichnissen behandelt werden. Mögliche Werte: <br> Ignorieren: Symbolische Verknüpfungen werden ignoriert, und die referenzierten Dateien/Verzeichnisse werden nicht einbezogen.<br>Folgen: Folgt den symbolischen Verknüpfungen bei der Rekursion und bindet auch die referenzierten Dateien/Verzeichnisse ein.<br>Verwalten: Folgt den symbolischen Links und ermöglicht eine Änderung von zurückgegebenen Inhalten.      |

## <a name="manage-machine-groups"></a>Verwalten von Computergruppen

Mit der Option „Inventar“ können Sie in Azure Monitor-Protokolle Computergruppen erstellen und anzeigen. Computergruppen sind Sammlungen von Computern, die mit einer Abfrage in Azure Monitor-Protokolle definiert werden.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Wählen Sie zum Anzeigen Ihrer Computergruppen auf der Seite „Inventar“ die Option **Computergruppen**.

![Anzeigen der Computergruppen auf der Seite „Inventar“](./media/automation-vm-inventory/inventory-machine-groups.png)

Wenn Sie in der Liste eine Computergruppe auswählen, wird die Seite „Computergruppen“ geöffnet. Diese Seite enthält Informationen zur Computergruppe. Hierzu gehört auch die Log Analytics-Abfrage, die zum Definieren der Gruppe verwendet wird. Unten auf der Seite befindet sich eine Liste mit den Computern (ggf. auf mehreren Seiten), die Teil der Gruppe sind.

![Anzeigen der Seite „Computergruppe“](./media/automation-vm-inventory/machine-group-page.png)

Klicken Sie auf **+ Klonen**, um die Computergruppe zu klonen. Sie müssen für die Gruppe einen neuen Namen und einen Alias angeben. Die Definition kann jetzt geändert werden. Klicken Sie nach dem Ändern der Abfrage auf **Abfrage validieren**, um eine Vorschau für die Computer anzuzeigen, die ausgewählt werden sollen. Wenn Sie mit der Gruppe zufrieden sind, können Sie auf **Erstellen** klicken, um die Computergruppe zu erstellen.

Klicken Sie auf **+ Computergruppe erstellen**, um eine neue Computergruppe zu erstellen. Mit dieser Schaltfläche wird die Seite **Computergruppe erstellen** geöffnet, auf der Sie die neue Gruppe definieren können. Klicken Sie auf **Erstellen**, um die Gruppe zu erstellen.

![Erstellen der neuen Computergruppe](./media/automation-vm-inventory/create-new-group.png)

## <a name="disconnect-your-virtual-machine-from-management"></a>Trennen des virtuellen Computers von der Verwaltung

Gehen Sie wie folgt vor, um Ihren virtuellen Computer aus der Bestandsverwaltung herauszunehmen:

1. Wählen Sie im linken Bereich des Azure-Portals die Option **Log Analytics** und dann den Arbeitsbereich, den Sie beim Onboarding Ihres virtuellen Computers verwendet haben.
2. Öffnen Sie auf der Seite „Log Analytics“ das Menü **Ressourcen**.
3. Wählen Sie unter **Arbeitsbereichsdatenquellen** die Option **Virtuelle Computer** aus.
4. Wählen Sie in der Liste den virtuellen Computer aus, den Sie trennen möchten. Der virtuelle Computer ist in der Spalte **OMS-Verbindung** neben **Dieser Arbeitsbereich** mit einem grünen Häkchen gekennzeichnet.

   >[!NOTE]
   >OMS (Operations Management Suite) wird jetzt als „Azure Monitor-Protokolle“ bezeichnet.
   
5. Klicken Sie oben auf der nächsten Seite auf **Trennen**.
6. Klicken Sie im Bestätigungsfenster auf **Ja**, um den Computer von der Verwaltung zu trennen.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Verwaltung von Änderungen in Dateien und Registrierungseinstellungen auf Ihren virtuellen Computern finden Sie unter [Nachverfolgen von Änderungen an der Software in Ihrer Umgebung mit der Change Tracking-Lösung](../log-analytics/log-analytics-change-tracking.md).
* Informationen zur Verwaltung von Windows- und Paketupdates auf Ihren virtuellen Computern finden Sie unter [Lösung für die Updateverwaltung in Azure](../operations-management-suite/oms-solution-update-management.md).