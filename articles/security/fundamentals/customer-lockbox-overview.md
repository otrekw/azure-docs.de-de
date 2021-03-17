---
title: Kunden-Lockbox für Microsoft Azure
description: Technische Übersicht über Kunden-Lockbox für Microsoft Azure, ein Feature, das Kontrolle über den Cloudanbieterzugriff für Fälle bietet, in denen Microsoft Zugriff auf Kundendaten benötigt.
author: TerryLanfear
ms.service: security
ms.subservice: security-fundamentals
ms.topic: article
ms.author: terrylan
manager: rkarlin
ms.date: 02/19/2021
ms.openlocfilehash: 0146e4fcaf70d37975dc587a266c47bf4b3f4601
ms.sourcegitcommit: afb9e9d0b0c7e37166b9d1de6b71cd0e2fb9abf5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/14/2021
ms.locfileid: "103461673"
---
# <a name="customer-lockbox-for-microsoft-azure"></a>Kunden-Lockbox für Microsoft Azure

> [!NOTE]
> Um dieses Feature verwenden zu können, benötigt Ihre Organisation einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) mit der Mindestebene **Developer**.

Kunden-Lockbox für Microsoft Azure bietet eine Oberfläche, auf der Kunden Anforderungen nach Zugriff auf Kundendaten prüfen und dann genehmigen oder ablehnen können. Das Feature wird in Fällen verwendet, in denen ein Microsoft-Techniker während einer Supportanfrage auf Kundendaten zugreifen muss.

In diesem Artikel wird erläutert, wie Sie die Kunden-Lockbox aktivieren und Lockbox-Anforderungen für spätere Überprüfungen und Audits initiieren, nachverfolgen und speichern.

<a name='supported-services-and-scenarios-in-general-availability'></a><a name='supported-services-and-scenarios-in-preview'></a>
## <a name="supported-services-and-scenarios-general-availability"></a>Unterstützte Dienste und Szenarien (allgemein verfügbar)

Die folgenden Dienste sind jetzt für die Kunden-Lockbox allgemein verfügbar:

- Azure API Management
- Azure App Service
- Azure Cognitive Services
- Azure Container Registry
- Azure Database for MySQL
- Azure Databricks
- Azure Data Box
- Azure-Daten-Explorer
- Azure Data Factory
- Azure Database for PostgreSQL
- Azure Functions
- Azure HDInsight
- Azure Kubernetes Service
- Azure Monitor
- Azure Storage
- Azure SQL-Datenbank
- Azure-Abonnementübertragung
- Azure Synapse Analytics
- Virtuelle Computer in Azure (inklusive Remotedesktopzugriff, Zugriff auf Speicherabbilder und verwalteter Datenträger)

## <a name="enable-customer-lockbox"></a>Aktivieren der Kunden-Lockbox

Sie können die Kunden-Lockbox jetzt über das [Verwaltungsmodul](https://aka.ms/customerlockbox/administration) auf dem Blatt „Kunden-Lockbox“ aktivieren.  

> [!NOTE]
> Die Aktivierung der Kunden-Lockbox ist nur möglich, wenn dem Benutzerkonto die Rolle [Globaler Administrator](../../active-directory/roles/manage-roles-portal.md) zugewiesen wurde.

## <a name="workflow"></a>Workflow

Die folgenden Schritte beschreiben einen typischen Workflow für eine Kunden-Lockbox-Anforderung.

1. Eine Person in einer Organisation hat ein Problem mit ihrer Azure-Workload.

2. Nachdem diese Person das Problem behandelt hat, es aber nicht beheben kann, öffnet sie ein Supportticket über das [Azure-Portal](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac). Das Ticket wird einem Azure-Kundensupporttechniker zugewiesen.

3. Ein Azure-Supporttechniker überprüft den Service Request und ermittelt die nächsten Schritte zur Behebung des Problems.

4. Wenn der Supporttechniker das Problem nicht mithilfe von Standardtools und Telemetriedaten beheben kann, besteht der nächste Schritt darin, über einen JIT-Zugriffsdienst (Just-In-Time) erhöhte Berechtigungen anzufordern. Diese Anforderung kann vom ursprünglichen Supporttechniker oder einem anderen Supporttechniker gestellt werden, da das Problem an das Azure DevOps-Team eskaliert wird.

5. Nachdem die Zugriffsanforderung vom Azure-Techniker übermittelt wurde, wertet der JIT-Dienst die Anforderung unter Berücksichtigung von Faktoren wie den folgenden aus:
    - Bereich der Ressource
    - Ob die anfordernde Person eine isolierte Identität aufweist oder mehrstufige Authentifizierung ausführt
    - Berechtigungsebenen

    Basierend auf der JIT-Regel kann diese Anforderung auch eine Genehmigung von internen genehmigenden Personen bei Microsoft enthalten. Die genehmigende Person kann z. B. der Kundensupportleiter oder der DevOps-Manager sein.

6. Wenn für die Anforderung direkter Zugriff auf Kundendaten erforderlich ist, wird eine Kunden-Lockbox-Anforderung initiiert. Beispiel: Remotedesktopzugriff auf einen virtuellen Computer des Kunden.

    Die Anforderung befindet sich nun im Zustand **Kunde benachrichtigt** und wartet auf die Zustimmung des Kunden, bevor Zugriff gewährt wird.

7. Auf der Seite der Kundenorganisation erhält der Benutzer mit der [Rolle „Besitzer“](../../role-based-access-control/rbac-and-directory-admin-roles.md#azure-roles) für das Azure Abonnement eine E-Mail von Microsoft, in der er über die ausstehende Anforderung benachrichtigt wird. Für Kunden-Lockbox-Anforderungen ist diese Person die festgelegte genehmigende Person.

    Beispiel-E-Mail:

    ![Azure Kunden-Lockbox – E-Mail-Benachrichtigung](./media/customer-lockbox-overview/customer-lockbox-email-notification.png)

8. Die E-Mail-Benachrichtigung enthält einen Link zum Blatt **Kunden-Lockbox** im Verwaltungsmodul. Über diesen Link meldet sich die festgelegte genehmigende Person beim Azure-Portal an, um ausstehende Anforderungen anzuzeigen, die bei ihrer Organisation für Kunden-Lockbox vorliegen:

    ![Azure Kunden-Lockbox – Startseite](./media/customer-lockbox-overview/customer-lockbox-landing-page.png)

   Die Anforderung verbleibt vier Tage lang in der Kundenwarteschlange. Anschließend läuft die Zugriffsanforderung automatisch ab, und den Microsoft-Technikern wird kein Zugriff gewährt.

9. Um die Details der ausstehenden Anforderung abzurufen, kann die festgelegte genehmigende Person die Lockbox-Anforderung unter **Ausstehende Anforderungen** auswählen:

    ![Azure Kunden-Lockbox – Anzeigen der ausstehenden Anforderung](./media/customer-lockbox-overview/customer-lockbox-pending-requests.png)

10. Der festgelegte genehmigende Person kann auch die **DIENSTANFORDERUNGS-ID** auswählen, um die vom ursprünglichen Benutzer erstellte Supportticketanforderung anzuzeigen. Diese Informationen bieten einen Kontext für den Grund der Beteiligung des Microsoft-Supports und den Hintergrund des gemeldeten Problems. Beispiel:

    ![Azure Kunden-Lockbox – Anzeigen der Supportticketanforderung](./media/customer-lockbox-overview/customer-lockbox-support-ticket.png)

11. Nach dem Überprüfen der Anforderung wählt die festgelegte genehmigende Person **Genehmigen** oder **Verweigern** aus:

    ![Azure Kunden-Lockbox – Auswählen von „Genehmigen“ oder „Verweigern“](./media/customer-lockbox-overview/customer-lockbox-approval.png)

    Ergebnis der Auswahl:
    - **Genehmigen:**  Dem Microsoft-Techniker wird Zugriff gewährt. Der Zugriff gilt für eine Dauer von acht Stunden.
    - **Verweigern:** Die Anforderung nach erhöhten Zugriffsrechten durch den Microsoft-Techniker wird abgelehnt, und es wird keine weitere Aktion ausgeführt.

Für Überwachungszwecke werden die in diesem Workflow ausgeführten Aktionen in [Kunden-Lockbox-Anforderungsprotokollen](#auditing-logs) protokolliert.

## <a name="auditing-logs"></a>Überwachungsprotokolle

Kunden-Lockbox-Protokolle werden in Aktivitätsprotokollen gespeichert. Wählen Sie im Azure-Portal **Aktivitätsprotokolle** aus, um Überwachungsinformationen im Zusammenhang mit Kunden-Lockbox-Anforderungen anzuzeigen. Sie können nach bestimmten Aktionen filtern, z. B.:
- **Deny Lockbox Request** (Lockbox-Anforderung verweigern)
- **Create Lockbox Request** (Lockbox-Anforderung erstellen)
- **Approve Lockbox Request** (Lockbox-Anforderung genehmigen)
- **Lockbox Request Expiry** (Ablauf einer Lockbox-Anforderung)

Beispiel:

![Azure Kunden-Lockbox – Aktivitätsprotokolle](./media/customer-lockbox-overview/customer-lockbox-activitylogs.png)

## <a name="customer-lockbox-integration-with-azure-security-benchmark"></a>Kunden-Lockbox-Integration im Azure-Sicherheitsvergleichstest

Wir haben dem Azure-Sicherheitsvergleichstest eine neue Baselinekontrolle ([3.13](../benchmarks/security-control-identity-access-control.md#313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios)) für Kunden-Lockbox hinzugefügt. Kunden können nun die Benchmark nutzen, um die Anwendbarkeit der Kunden-Lockbox für einen Dienst zu überprüfen.

## <a name="exclusions"></a>Ausschlüsse

In den folgenden Supportszenarios werden keine Kunden-Lockbox-Anforderungen ausgelöst:

- Ein Microsoft-Techniker muss eine Aktivität ausführen, die außerhalb der Standardbetriebsvorgänge liegt. Beispiel: Wiederherstellen von Diensten in unerwarteten oder unvorhersehbaren Szenarien.
- Ein Microsoft-Techniker greift im Rahmen der Problembehandlung auf die Azure-Plattform zu und hat versehentlich Zugriff auf Kundendaten. Beispiel: Das Azure-Team führt eine Problembehandlung durch, die zur Erfassung eines Pakets auf einem Netzwerkgerät führt. In diesem Szenario kann der Techniker die Daten nicht lesen, wenn sie vom Kunden während der Übertragung verschlüsselt werden.

## <a name="next-steps"></a>Nächste Schritte

Die Kunden-Lockbox steht allen Kunden zur Verfügung, die einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) mit der Mindestebene **Developer** besitzen. Sie können die Kunden-Lockbox über das [Verwaltungsmodul](https://aka.ms/customerlockbox/administration) auf dem Blatt „Kunden-Lockbox“ aktivieren.

Kunden-Lockbox-Anforderungen werden von einem Microsoft-Techniker initiiert, falls dies für die weitere Bearbeitung eines Supportfalls erforderlich ist.
