---
title: Blaupausenbeispiel „DoD-Auswirkungsstufe 5“
description: Bereitstellungsschritte für das Blaupausenbeispiel „DoD-Auswirkungsstufe 5“, einschließlich Details zum Blaupausenartefaktparameter
ms.date: 06/30/2020
ms.topic: sample
ms.openlocfilehash: 9267ed0de35107b8f6582d852ab925b4dbbbdba4
ms.sourcegitcommit: a989fb89cc5172ddd825556e45359bac15893ab7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2020
ms.locfileid: "85804408"
---
# <a name="deploy-the-dod-impact-level-5-blueprint-sample"></a>Bereitstellen des Blaupausenbeispiels „DoD-Auswirkungsstufe 5“

Zum Bereitstellen des Blaupausenbeispiels „DoD-Auswirkungsstufe 5“ (Department of Defense Impact Level 5, DoD IL5) von Azure Blueprints müssen die folgenden Schritte ausgeführt werden:

> [!div class="checklist"]
> - Erstellen einer neuen Blaupause mithilfe des Beispiels
> - Markieren Ihrer Kopie des Beispiels als **Veröffentlicht**
> - Zuweisen Ihrer Kopie der Blaupause zu einem vorhandenen Abonnement

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen, bevor Sie beginnen.

## <a name="create-blueprint-from-sample"></a>Erstellen einer Blaupause mithilfe des Beispiels

Implementieren Sie zuerst das Blaupausenbeispiel, indem Sie mithilfe des Beispiels eine neue Blaupause in Ihrer Umgebung erstellen.

1. Wählen Sie **Alle Dienste** im linken Bereich aus. Suchen Sie nach **Blaupausen**, und wählen Sie die Option aus.

1. Klicken Sie links auf der Seite **Erste Schritte** unter _Blaupause erstellen_ auf die Schaltfläche **Erstellen**.

1. Suchen Sie unter _Weitere Beispiele_ nach dem Blaupausenbeispiel **DoD-Auswirkungsstufe 5**, und wählen Sie **Dieses Beispiel verwenden** aus.

1. Geben Sie die _Grundlagen_ des Blaupausenbeispiels ein:

   - **Name der Blaupause**: Geben Sie einen Namen für Ihre Kopie des Blaupausenbeispiels „DoD-Auswirkungsstufe 5“ ein.
   - **Definitionsspeicherort**: Klicken Sie auf die Schaltfläche mit den Auslassungspunkten, und wählen Sie die Verwaltungsgruppe aus, in der Sie Ihre Kopie des Beispiels speichern möchten.

1. Wählen Sie oben auf der Seite die Registerkarte _Artefakte_ oder unten auf der Seite die Option **Weiter: Artefakte** aus.

1. Überprüfen Sie die Liste der Artefakte, aus denen das Blaupausenbeispiel besteht. Viele der Artefakte enthalten Parameter, die Sie später definieren. Wählen Sie **Entwurf speichern** aus, nachdem Sie das Blaupausenbeispiel überprüft haben.

## <a name="publish-the-sample-copy"></a>Veröffentlichen der Kopie des Beispiels

Ihre Kopie des Blaupausenbeispiels wurde jetzt in Ihrer Umgebung erstellt. Sie wird im Modus **Entwurf** erstellt und muss **veröffentlicht** werden, bevor sie zugewiesen und bereitgestellt werden kann. Die Kopie des Blaupausenbeispiels kann an Ihre Umgebung und Ihre Anforderungen angepasst werden. Durch diese Änderungen ist sie aber möglicherweise nicht mehr mit den Steuerungen der DoD-Auswirkungsstufe 5 konform.

1. Wählen Sie **Alle Dienste** im linken Bereich aus. Suchen Sie nach **Blaupausen**, und wählen Sie die Option aus.

1. Wählen Sie links die Seite **Blaupausendefinitionen** aus. Verwenden Sie die Filter, um Ihre Kopie des Blaupausenbeispiels zu suchen, und wählen Sie es aus.

1. Wählen Sie oben auf der Seite die Option **Blaupause veröffentlichen** aus. Geben Sie auf der neuen Seite rechts für Ihre Kopie des Blaupausenbeispiels eine **Version** an. Diese Eigenschaft ist hilfreich, wenn Sie später Änderungen vornehmen. Geben Sie **Änderungshinweise** an, etwa „Erste Version aus dem Blaupausenbeispiel „DoD IL5“ veröffentlicht“. Wählen Sie dann unten auf der Seite **Veröffentlichen** aus.

## <a name="assign-the-sample-copy"></a>Zuweisen der Kopie des Beispiels

Nachdem die Kopie des Blaupausenbeispiels erfolgreich **veröffentlicht** wurde, kann sie einem Abonnement innerhalb der Verwaltungsgruppe, in der sie gespeichert wurde, zugewiesen werden. In diesem Schritt werden Parameter angegeben, damit jede Bereitstellung der Kopie des Blaupausenbeispiels eindeutig ist.

1. Wählen Sie **Alle Dienste** im linken Bereich aus. Suchen Sie nach **Blaupausen**, und wählen Sie die Option aus.

1. Wählen Sie links die Seite **Blaupausendefinitionen** aus. Verwenden Sie die Filter, um Ihre Kopie des Blaupausenbeispiels zu suchen, und wählen Sie es aus.

1. Wählen Sie oben auf der Seite mit der Blaupausendefinition die Option **Blaupause zuweisen** aus.

1. Geben Sie die Parameterwerte für die Blaupausenzuweisung an:

   - Grundlagen

     - **Abonnements**: Wählen Sie mindestens eines der Abonnements in der Verwaltungsgruppe aus, in der Sie die Kopie des Blaupausenbeispiels gespeichert haben. Wenn Sie mehrere Abonnements auswählen, wird für jedes Abonnement eine Zuweisung mit den eingegebenen Parametern erstellt.
     - **Zuweisungsname:** Der Name wird basierend auf dem Namen der Blaupause vorab ausgefüllt.
       Sie können ihn nach Bedarf ändern oder unverändert übernehmen.
     - **Standort**: Wählen Sie eine Region aus, in der die verwaltete Identität erstellt werden soll. Azure Blueprint verwendet diese verwaltete Identität zum Bereitstellen aller Artefakte in der zugewiesenen Blaupause. Weitere Informationen finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../../../../active-directory/managed-identities-azure-resources/overview.md).
     - **Version der Blaupausendefinition:** Wählen Sie eine **veröffentlichte** Version Ihrer Kopie des Blaupausenbeispiels aus.

   - Zuweisung sperren

     Wählen Sie die Blaupausensperreinstellung für die Umgebung aus. Weitere Informationen finden Sie unter [Grundlegendes zur Ressourcensperre in Azure Blueprint](../../concepts/resource-locking.md).

   - Verwaltete Identität

     Behalten Sie die Standardeinstellung _Vom System zugewiesen_ für die verwaltete Identität bei.

   - Artefaktparameter

     Die in diesem Abschnitt definierten Parameter gelten für das Artefakt, unter dem sie definiert werden. Diese Parameter sind [dynamische Parameter](../../concepts/parameters.md#dynamic-parameters), da sie während der Zuweisung der Blaupause definiert werden. Eine vollständige Liste der Artefaktparameter und die zugehörigen Beschreibungen finden Sie in der [Tabelle der Artefaktparameter](#artifact-parameters-table).

1. Nachdem Sie alle Parameter eingegeben haben, wählen Sie unten auf der Seite die Option **Zuweisen** aus. Die Blaupausenzuweisung wird erstellt, und die Artefaktbereitstellung wird gestartet. Die Bereitstellung dauert ungefähr eine Stunde. Um den Status der Bereitstellung zu überprüfen, öffnen Sie die Blaupausenzuweisung.

> [!WARNING]
> Der Azure Blueprints-Dienst und die integrierten Blaupausenbeispiele sind **kostenlos**. Azure-Ressourcen werden [nach Produkt abgerechnet](https://azure.microsoft.com/pricing/). Verwenden Sie den [Preisrechner](https://azure.microsoft.com/pricing/calculator/) zum Schätzen der Kosten für die Ausführung der Ressourcen, die mit diesem Blaupausenbeispiel bereitgestellt werden.

## <a name="artifact-parameters-table"></a>Tabelle der Artefaktparameter

Die folgende Tabelle enthält eine Aufstellung der Parameter des Blaupausenartefakts:

|Artefaktname|Artefakttyp|Parametername|BESCHREIBUNG|
|-|-|-|-|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Liste der Benutzer, die in der Windows-VM-Administratorengruppe enthalten sein müssen|Eine durch Semikolon getrennte Liste von Benutzern, die in der Gruppe der lokalen Administratoren enthalten sein sollen, z. B. Administrator; myUser1; myUser2|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Liste der aus der Windows-VM-Administratorengruppe ausgeschlossenen Benutzer|Eine durch Semikolon getrennte Liste von Benutzern, die nicht in der Gruppe der lokalen Administratoren enthalten sein sollen, z. B. Administrator; myUser1; myUser2|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Liste von Ressourcentypen, für die Diagnoseprotokolle aktiviert werden sollen||
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Log Analytics-Arbeitsbereichs-ID für VM-Agent-Berichterstellung|ID (GUID) des Log Analytics-Arbeitsbereichs, in dem VM-Agents Bericht erstatten sollen|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Liste der Regionen, in denen Network Watcher aktiviert werden soll|Verwenden Sie „Get-AzLocation“, um eine vollständige Liste der Regionen abzurufen.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|TLS-Mindestversion für Windows-Webserver|Die TLS-Mindestprotokollversion, die auf Windows-Webservern aktiviert werden soll|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Neueste PHP-Version|Neueste unterstützte PHP-Version für App Services|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Neueste Java-Version|Neueste unterstützte Java-Version für App Services|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Neueste Python-Version für Windows|Neueste unterstützte Python-Version für App Services|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Neueste Python-Version für Linux|Neueste unterstützte Python-Version für App Services|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Optional: Liste der Windows-VM-Images, die den Log Analytics-Agent zum Hinzufügen zum Überwachungsbereich unterstützen|Eine durch Semikolons getrennte Liste mit Images. Beispiel: /subscriptions/<subscriptionId>/resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Optional: Liste der Linux-VM-Images, die den Log Analytics-Agent zum Hinzufügen zum Überwachungsbereich unterstützen|Eine durch Semikolons getrennte Liste mit Images. Beispiel: /subscriptions/<subscriptionId>/resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Ihrem Abonnement sollte mehr als ein Besitzer zugewiesen sein.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Die Datenträgerverschlüsselung sollte auf virtuelle Computer angewendet werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: E-Mail-Benachrichtigung des Abonnementbesitzers bei Warnungen mit hohem Schweregrad muss aktiviert sein|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Remotedebuggen sollte für Funktions-Apps deaktiviert sein.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Sicherstellen, dass die neueste .NET Framework-Version angegeben ist, wenn sie als Teil der Funktions-App verwendet wird|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Transparent Data Encryption für SQL-Datenbanken sollte aktiviert werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Bewertung von Sicherheitsrisiken für verwaltete SQL-Instanzen aktivieren|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Stellen Sie sicher, dass die neueste PHP-Version angegeben ist, wenn sie als Teil der API-App verwendet wird.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Ein Azure Active Directory-Administrator sollte für SQL-Server-Instanzen bereitgestellt werden|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Nur sichere Verbindungen mit Ihrer Redis Cache-Instanz sollten aktiviert werden|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Die Endpoint Protection-Lösung sollte für VM-Skalierungsgruppen installiert sein.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Nicht eingeschränkten Netzwerkzugriff auf Speicherkonten überwachen|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Advanced Data Security-Einstellungen für verwaltete SQL-Instanzen sollten eine E-Mail-Adresse für den Empfang von Sicherheitswarnungen enthalten.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Sicherheitsrisiken in der Sicherheitskonfiguration von VM-Skalierungsgruppen sollten beseitigt werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Sichere Übertragung in Speicherkonten sollte aktiviert werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Die adaptive Anwendungssteuerung sollte auf virtuellen Computern aktiviert werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Georedundante Sicherung muss für Azure Database for PostgreSQL aktiviert sein|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Stellen Sie sicher, dass die neueste Java-Version angegeben ist, wenn sie als Teil der Web-App verwendet wird.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Maximal 3 Besitzer sollten für Ihr Abonnement festgelegt sein.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Für Ihr Abonnement muss eine E-Mail-Adresse als Sicherheitskontakt angegeben sein|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: CORS sollte nicht jeder Ressource Zugriff auf Ihre Webanwendungen erteilen.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Externe Konten mit Schreibberechtigungen sollten aus Ihrem Abonnement entfernt werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Externe Konten mit Leseberechtigungen sollten aus Ihrem Abonnement entfernt werden|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Veraltete Konten sollten aus Ihrem Abonnement entfernt werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Zugriff auf Funktions-App nur über HTTPS gestatten|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Stellen Sie sicher, dass die neueste Python-Version angegeben ist, wenn sie als Teil der Web-App verwendet wird.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Stellen Sie sicher, dass die neueste Python-Version angegeben ist, wenn sie als Teil der Funktions-App verwendet wird.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Stellen Sie sicher, dass die neueste PHP-Version angegeben ist, wenn sie als Teil der Web-App verwendet wird.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Stellen Sie sicher, dass die neueste Python-Version angegeben ist, wenn sie als Teil der API-App verwendet wird.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Sicherheitsrisiken sollten durch eine Lösung zur Sicherheitsrisikobewertung beseitigt werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Georedundante Sicherung muss für Azure Database for MySQL aktiviert sein|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Sicherstellen, dass die neueste .NET Framework-Version angegeben ist, wenn sie als Teil der Web-App verwendet wird|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Systemupdates sollten auf Ihren Computern installiert sein.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Stellen Sie sicher, dass die neueste Java-Version angegeben ist, wenn sie als Teil der API-App verwendet wird.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Stellen Sie sicher, dass die neueste HTTP-Version angegeben ist, wenn sie zum Ausführen der Web-App verwendet wird.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: In Ihrer API-App sollte die neueste TLS-Version verwendet werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: MFA sollte für Konten mit Schreibrechten für Ihr Abonnement aktiviert werden|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Advanced Data Security-Einstellungen für SQL Server sollten eine E-Mail-Adresse für den Empfang von Sicherheitswarnungen enthalten.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Stellen Sie sicher, dass die neueste HTTP-Version angegeben ist, wenn sie zum Ausführen der API-App verwendet wird.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Die Microsoft IaaSAntimalware-Erweiterung muss auf Windows-Servern bereitgestellt werden|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Sicherstellen, dass die neueste Java-Version angegeben ist, wenn sie als Teil der Funktions-App verwendet wird|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Zugriff über Endpunkt mit Internetzugriff sollte eingeschränkt werden|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Standard-Tarif von Security Center muss ausgewählt sein|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Verwendung benutzerdefinierter RBAC-Regeln überwachen|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Zugriff auf Webanwendung nur über HTTPS gestatten|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Die Überwachung in SQL Server muss aktiviert werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Der Log Analytics-Agent sollte auf virtuellen Computern installiert werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: DDoS Protection Standard sollte aktiviert sein.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: MFA sollte für Konten mit Besitzerberechtigungen in Ihrem Abonnement aktiviert sein.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Stellen Sie sicher, dass die neueste PHP-Version angegeben ist, wenn sie als Teil der Funktions-App verwendet wird.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Advanced Data Security muss für Ihre SQL-Server aktiviert werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Advanced Data Security muss für Ihre verwalteten SQL-Instanzen aktiviert werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: In den Advanced Data Security-Einstellungen für die verwaltete SQL-Instanz müssen E-Mail-Benachrichtigungen an Administratoren und Abonnementbesitzer aktiviert sein|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Fehlenden Endpoint Protection-Schutz in Azure Security Center überwachen|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Die Just-In-Time-Netzwerkzugriffssteuerung sollte auf virtuelle Computer angewendet werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Für Ihr Abonnement muss eine Telefonnummer für den Sicherheitskontakt angegeben werden|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Service Fabric-Cluster sollten nur Azure Active Directory für die Clientauthentifizierung verwenden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Auf API-Apps sollte nur über HTTPS zugegriffen werden können|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Advanced Threat Protection-Typen sollten in den Advanced Data Security-Einstellungen für verwaltete SQL-Instanzen auf „Alle“ festgelegt werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Georedundanter Speicher muss für Speicherkonten aktiviert sein|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Sicherstellen, dass die neueste .NET Framework-Version angegeben ist, wenn sie als Teil der API-App verwendet wird|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Systemupdates für VM-Skalierungsgruppen sollten installiert werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: E-Mail-Benachrichtigungen an Administratoren und Abonnementbesitzer sollten in den erweiterten Einstellungen für Datensicherheit in SQL Server aktiviert werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Remotedebuggen sollte für Webanwendungen deaktiviert sein.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Langfristige georedundante Sicherung muss für Azure SQL-Datenbank aktiviert sein|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Sicherheitsrisiken in der Sicherheitskonfiguration für Ihre Computer sollten beseitigt werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Stellen Sie sicher, dass die neueste HTTP-Version angegeben ist, wenn sie zum Ausführen der Funktions-App verwendet wird.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: MFA sollte für Ihre Abonnementkonten mit Leseberechtigungen aktiviert sein|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Advanced Threat Protection-Typen sollten in den Advanced Data Security-Einstellungen für SQL Server auf „Alle“ festgelegt werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Sicherheitsrisiken in Containersicherheitskonfigurationen sollten behoben werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Remotedebuggen sollte für API-Apps deaktiviert sein.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Veraltete Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Für Ihre SQL Server-Instanzen muss eine Sicherheitsrisikobewertung aktiviert sein.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Der Log Analytics-Agent sollte in Virtual Machine Scale Sets installiert sein.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: In Ihrer Web-App sollte die neueste TLS-Version verwendet werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Externe Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: In Ihrer Funktions-App sollte die neueste TLS-Version verwendet werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: [Vorschau]: Für Kubernetes Service muss ein Upgrade auf eine Kubernetes-Version ohne Sicherheitsrisiko durchgeführt werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|
|DoD-Auswirkungsstufe 5|Richtlinienzuweisung|Auswirkung für die Richtlinie: Sicherheitsrisiken in SQL-Datenbanken sollten beseitigt werden.|Azure Policy-Auswirkung für diese Richtlinie. Weitere Informationen zu Auswirkungen finden Sie unter https://aka.ms/policyeffects.|

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun die Schritte zur Bereitstellung des Blaupausenbeispiels „DoD-Auswirkungsstufe 5“ angesehen haben, lesen Sie die folgenden Artikel, um mehr über die Blaupausen- und Steuerungszuordnung zu erfahren:

> [!div class="nextstepaction"]
> [Blaupause „DoD-Auswirkungsstufe 5“: Übersicht](./index.md)
> [Blaupause „DoD-Auswirkungsstufe 5“: Steuerungszuordnung](./control-mapping.md)

Weitere Artikel zu Blaupausen und ihrer Nutzung:

- Erfahren Sie mehr über den [Lebenszyklus von Blaupausen](../../concepts/lifecycle.md).
- Machen Sie sich mit der Verwendung [statischer und dynamischer Parameter](../../concepts/parameters.md) vertraut.
- Erfahren Sie, wie Sie die [Abfolge von Blaupausen](../../concepts/sequencing-order.md) anpassen können.
- Erfahren Sie, wie Sie [Ressourcen in Blaupausen sperren](../../concepts/resource-locking.md) können.
- Lernen Sie, wie Sie [vorhandene Zuweisungen aktualisieren](../../how-to/update-existing-assignments.md).