---
title: Vorläufiges Löschen wird für alle Azure Key Vaults aktiviert | Microsoft-Dokumentation
description: Verwenden Sie dieses Dokument, um vorläufiges löschen für alle Schlüsseltresore zu übernehmen.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 07/27/2020
ms.author: sudbalas
ms.openlocfilehash: 0e811cc219002c034afb968be760ce2c249b08f3
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91825249"
---
# <a name="soft-delete-will-be-enabled-on-all-key-vaults"></a>Vorläufiges Löschen wird für alle Schlüsseltresore aktiviert

> [!WARNING]
> **Breaking Change**: Die Möglichkeit, das vorläufige Löschen zu deaktivieren, wird am Ende des Jahres eingestellt, und der vorläufige Löschschutz wird für alle Schlüsseltresore automatisch aktiviert.  Azure Key Vault-Benutzer und -Administratoren sollten das vorläufige Löschen für ihre Schlüsseltresore sofort aktivieren.
>
> Für verwaltetes HSM ist vorläufiges Löschen standardmäßig aktiviert und kann nicht deaktiviert werden.

Wenn ein Geheimnis aus einem Schlüsseltresor ohne vorläufigen Löschschutz gelöscht wird, wird das Geheimnis endgültig gelöscht. Benutzer können vorläufiges Löschen zurzeit bei der Erstellung des Schlüsseltresors deaktivieren, doch um ihre Geheimnisse vor versehentlichem oder bösartigem Löschen durch einen Benutzer zu schützen, aktiviert Microsoft bald den vorläufigen Löschschutz für **alle** Schlüsseltresore, und Benutzer haben dann nicht mehr die Möglichkeit, das vorläufige Löschen zu deaktivieren.

:::image type="content" source="../media/softdeletediagram.png" alt-text="<Alternativtext>":::

Vollständige Details zur Funktion für vorläufiges Löschen finden Sie unter [Übersicht über die Azure Key Vault-Funktion für vorläufiges Löschen](soft-delete-overview.md).

## <a name="how-do-i-respond-to-breaking-changes"></a>Gewusst wie: Reagieren auf Breaking Changes

Ein Schlüsseltresorobjekt kann nicht mit demselben Namen wie ein Schlüsseltresorobjekt im vorläufig gelöschten Zustand erstellt werden.  Wenn Sie beispielsweise einen Schlüssel mit dem Namen `test key` in Schlüsseltresor A löschen, können Sie erst dann einen neuen Schlüssel namens `test key` in Schlüsseltresor A erstellen, wenn das vorläufig gelöschte `test key`-Objekt endgültig gelöscht wurde.

### <a name="application-changes"></a>Anwendungsänderungen

Wenn Ihre Anwendung darauf basiert, dass vorläufiges Löschen nicht aktiviert ist, und erwartet, dass gelöschte Geheimnis- oder Schlüsseltresornamen für die sofortige Wiederverwendung verfügbar sind, muss Ihre Anwendungslogik die folgenden Änderungen vornehmen, um diese Änderung zu berücksichtigen.

1. Löschen des ursprünglichen Schlüsseltresors oder Geheimnisses
2. Endgültiges Löschen des Schlüsseltresors oder Geheimnisses im vorläufig gelöschten Zustand.
3. Warten – sofortiges Neuerstellen kann zu einem Konflikt führen.
4. Erstellen Sie den Schlüsseltresor erneut mit demselben Namen.
5. Implementieren Sie Wiederholungsversuche, wenn der Erstellungsvorgang weiterhin zu einem Namenskonfliktfehler führt. Es kann im schlimmsten Fall bis zu 10 Minuten dauern, bis DNS-Einträge aktualisiert werden.

### <a name="administration-changes"></a>Verwaltungsänderungen

Sicherheitsprinzipalen, die Zugriff benötigen, um Geheimnisse endgültig zu löschen, müssen zusätzliche Zugriffsrichtlinienberechtigungen gewährt werden, um diese Geheimnisse und den Schlüsseltresor endgültig zu löschen.

Wenn Sie eine Azure Policy für Ihre Schlüsseltresore haben, die die Deaktivierung des vorläufigen Löschens erfordert, muss diese Richtlinie deaktiviert werden.  Möglicherweise müssen Sie dieses Problem an einen Administrator eskalieren, der die auf Ihre Umgebung angewendeten Azure-Richtlinien kontrolliert. Wenn diese Richtlinie nicht deaktiviert wird, verlieren Sie möglicherweise die Möglichkeit, neue Schlüsseltresore im Gültigkeitsbereich der angewendeten Richtlinie zu erstellen.

Wenn für Ihre Organisation gesetzliche Complianceanforderungen gelten und sie nicht zulassen kann, dass gelöschte Schlüsseltresore und Geheimnisse für einen längeren Zeitraum in einem wiederherstellbaren Zustand verbleiben, müssen Sie den Aufbewahrungszeitraum für vorläufiges Löschen, der zwischen 7 und 90 Tagen konfigurierbar ist, anpassen, um die Standards Ihrer Organisation zu erfüllen.

## <a name="procedures"></a>Prozeduren

### <a name="audit-your-key-vaults-to-check-if-soft-delete-is-enabled"></a>Überwachen Ihrer Schlüsseltresore, um zu überprüfen, ob vorläufiges Löschen aktiviert ist

1. Melden Sie sich beim Azure-Portal an.
2. Suchen Sie nach „Azure Policy“.
3. Wählen Sie „Definitionen“ aus.
4. Wählen Sie unter „Kategorie“ im Filter „Key Vault“ aus.
5. Wählen Sie die Richtlinie „Key Vault-Objekte müssen wiederherstellbar sein“ aus.
6. Klicken Sie auf „Zuweisen“.
7. Legen Sie den Gültigkeitsbereich auf Ihr Abonnement fest.
8. Wählen Sie „Überprüfen + erstellen“ aus.
9. Es kann bis zu 24 Stunden dauern, bis eine vollständige Überprüfung Ihrer Umgebung abgeschlossen ist.
10. Klicken Sie auf dem Blatt „Azure Policy“ auf „Compliance“.
11. Wählen Sie die Richtlinie aus, die Sie angewendet haben.

Sie sollten jetzt filtern und feststellen können, für welche Ihrer Schlüsseltresore vorläufiges Löschen aktiviert ist (kompatible Ressourcen) und für welche Schlüsseltresore vorläufiges Löschen nicht aktiviert ist (nicht kompatible Ressourcen).

### <a name="turn-on-soft-delete-for-an-existing-key-vault"></a>Aktivieren des vorläufigen Löschens für einen vorhandenen Schlüsseltresor

1. Melden Sie sich beim Azure-Portal an.
2. Suchen Sie nach Ihrem Schlüsseltresor.
3. Wählen Sie unter „Einstellungen“ die Option „Eigenschaften“ aus.
4. Aktivieren Sie unter „Vorläufiges Löschen“ das Optionsfeld, das zu „Wiederherstellung dieses Tresors und zugehöriger Objekte aktivieren“ gehört.
5. Legen Sie den Aufbewahrungszeitraum für vorläufiges Löschen fest.
6. Wählen Sie „Speichern“ aus.

### <a name="grant-purge-access-policy-permissions-to-a-security-principal"></a>Gewähren von „Endgültig löschen“-Zugriffsrichtlinienberechtigungen für einen Sicherheitsprinzipal

1. Melden Sie sich beim Azure-Portal an.
2. Suchen Sie nach Ihrem Schlüsseltresor.
3. Wählen Sie unter „Einstellungen“ die Option „Zugriffsrichtlinien“ aus.
4. Wählen Sie den Dienstprinzipal aus, dem Sie den Zugriff gewähren möchten.
5. Scrollen Sie in jedem Berechtigungsdropdown unter „Schlüssel“, „Geheimnis“ und „Zertifikat“ nach unten bis zu „Privilegierte Vorgänge“, und wählen Sie die Berechtigung „Endgültig löschen“ aus.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="does-this-change-affect-me"></a>Betrifft mich diese Änderung?

Wenn Sie vorläufiges Löschen bereits aktiviert haben, oder wenn Sie Schlüsseltresorobjekte nicht löschen und neu mit demselben Namen erstellen, werden Sie wahrscheinlich keine Änderung beim Verhalten des Schlüsseltresors bemerken.

Wenn Sie eine Anwendung besitzen, die häufig Schlüsseltresorobjekte löscht und mit denselben Benennungskonventionen neu erstellt, müssen Sie Änderungen an Ihrer Anwendungslogik vornehmen, um das erwartete Verhalten zu gewährleisten. Siehe auch „Gewusst wie: Reagieren auf Breaking Changes“ im oben stehenden Absatz.

### <a name="how-do-i-benefit-from-this-change"></a>Wie profitiere ich von dieser Änderung?

Der vorläufige Löschschutz bietet Ihrer Organisation eine zusätzliche Schutzebene vor versehentlichem oder bösartigem Löschen. Als Schlüsseltresoradministrator können Sie den Zugriff sowohl auf Wiederherstellungsberechtigungen als auch auf Berechtigungen zum endgültigen Löschen einschränken.

Wenn ein Benutzer versehentlich einen Schlüsseltresor oder ein Geheimnis löscht, können Sie ihm Zugriffsberechtigungen erteilen, damit er das Geheimnis selbst wiederherstellen kann, ohne das Risiko zu erzeugen, dass er das Geheimnis oder den Schlüsseltresor endgültig löscht. Dieser Self-Service-Prozess minimiert Ausfallzeiten in Ihrer Umgebung und gewährleistet die Verfügbarkeit Ihrer Geheimnisse.

### <a name="how-do-i-find-out-if-i-need-to-take-action"></a>Gewusst wie: Muss ich Maßnahmen ergreifen?

Führen Sie die oben beschriebenen Schritte im Abschnitt „Vorgehensweise zum Überwachen Ihrer Schlüsseltresore, um zu überprüfen, ob vorläufiges Löschen aktiviert ist“ aus. Jeder Schlüsseltresor, für den vorläufiges Löschen nicht aktiviert ist, ist von dieser Änderung betroffen. Weitere Tools für die Überwachung werden in Kürze verfügbar sein, und dieses Dokument wird entsprechend aktualisiert.

### <a name="what-action-do-i-need-to-take"></a>Welche Maßnahmen muss ich ergreifen?

Stellen Sie sicher, dass Sie keine Änderungen an Ihrer Anwendungslogik vornehmen müssen. Sobald Sie sich dessen versichert haben, aktivieren Sie vorläufiges Löschen für alle Ihre Schlüsseltresore. Dadurch wird sichergestellt, dass Sie von einem Breaking Change unbeeinträchtigt bleiben, wenn am Ende des Jahres vorläufiges Löschen für alle Schlüsseltresore aktiviert wird.

### <a name="by-when-do-i-need-to-take-action"></a>Bis wann muss ich Maßnahmen ergreifen?

Vorläufiges Löschen wird bis Ende des Jahres für alle Schlüsseltresore aktiviert. Um sicherzustellen, dass Ihre Anwendungen nicht betroffen sind, aktivieren Sie vorläufiges Löschen so bald wie möglich für Ihre Schlüsseltresore.

## <a name="what-will-happen-if-i-dont-take-any-action"></a>Was passiert, wenn ich nichts unternehme?

Wenn Sie keine Maßnahmen ergreifen, wird vorläufiges Löschen automatisch am Ende des Jahres für alle Ihre Schlüsseltresore aktiviert. Dies kann zu Konfliktfehlern führen, wenn Sie versuchen, ein Schlüsseltresorobjekt zu löschen und es mit demselben Namen neu zu erstellen, ohne es zuerst aus dem vorläufig gelöschten Zustand endgültig zu löschen. Dies kann zum Fehlschlagen Ihrer Anwendungen oder Automatisierung führen.

## <a name="next-steps"></a>Nächste Schritte

- Setzen Sie sich bei Fragen zu dieser Änderung mit uns unter [akvsoftdelete@microsoft.com](mailto:akvsoftdelete@microsoft.com) in Verbindung.
- Lesen Sie [Vorläufiges Löschen: Übersicht](soft-delete-overview.md).
