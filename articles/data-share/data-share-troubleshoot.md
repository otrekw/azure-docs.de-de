---
title: Behandeln von Problemen mit Azure Data Share
description: Hier erfahren Sie, wie Sie beim Erstellen oder Empfangen von Datenfreigaben mit Azure Data Share Probleme mit Einladungen und Fehler behandeln.
services: data-share
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: troubleshooting
ms.date: 12/16/2020
ms.openlocfilehash: 3aa1c0b8579bd37d2bb51cbde70997131c696813
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97964506"
---
# <a name="troubleshoot-common-problems-in-azure-data-share"></a>Behandeln allgemeiner Probleme in Azure Data Share 

In diesem Artikel erfahren Sie, wie Sie allgemeine Probleme in Azure Data Share behandeln. 

## <a name="azure-data-share-invitations"></a>Azure Data Share-Einladungen 

In einigen Fällen wird möglicherweise eine leere Einladungsliste angezeigt, wenn neue Benutzer in einer E-Mail-Einladung auf **Einladung annehmen** klicken. 

:::image type="content" source="media/no-invites.png" alt-text="Screenshot: leere Einladungsliste":::

Das Problem kann eine der folgenden Ursachen haben:

* **Der Azure Data Share-Dienst ist im Azure-Mandanten nicht als Ressourcenanbieter eines Azure-Abonnements registriert.** Dieses Problem tritt auf, wenn Ihr Azure-Mandant keine Data Share-Ressource enthält. 

    Wenn Sie eine Azure Data Share-Ressource erstellen, wird der Ressourcenanbieter in Ihrem Azure-Abonnement automatisch registriert. Sie können den Data Share-Dienst manuell registrieren, indem Sie die folgenden Schritte ausführen. Sie müssen über die [Rolle „Mitwirkender“](../role-based-access-control/built-in-roles.md#contributor) für das Azure-Abonnement verfügen, um diese Schritte ausführen zu können. 

    1. Navigieren Sie im Azure-Portal zu **Abonnements**.
    1. Wählen Sie das Abonnement aus, das Sie zum Erstellen der Azure Data Share-Ressource verwenden möchten.
    1. Er wählt **Ressourcenanbieter** aus.
    1. Suchen Sie nach **Microsoft.DataShare**.
    1. Wählen Sie **Registrieren**.

* **Die Einladung wird nicht an Ihre E-Mail-Adresse für die Anmeldung bei Azure, sondern an Ihren E-Mail-Alias gesendet.** Wenn Sie den Azure Data Share-Dienst bereits registriert oder eine Data Share-Ressource im Azure-Mandanten erstellt haben, aber die Einladung weiterhin nicht angezeigt wird, ist möglicherweise Ihr E-Mail-Alias als Empfänger festgelegt. Wenden Sie sich an Ihren Datenanbieter, und vergewissern Sie sich, dass die Einladung an Ihre E-Mail-Adresse für die Anmeldung bei Azure und nicht an Ihren E-Mail-Alias gesendet wird.

* **Die Einladung wurde bereits akzeptiert.** Der Link in der E-Mail führt Sie zur Seite der **Data Share-Einladungen** im Azure-Portal. Auf dieser Seite werden nur ausstehende Einladungen aufgeführt. Bereits akzeptierte Einladungen werden auf der Seite nicht angezeigt. Wechseln Sie zu der Data Share-Ressource, mit der Sie die Einladung akzeptiert haben. Dort können Sie die empfangenen Freigaben anzeigen und die Einstellung für den Azure Data Explorer-Zielcluster konfigurieren.

## <a name="creating-and-receiving-shares"></a>Erstellen und Empfangen von Freigaben

Wenn Sie eine neue Freigabe erstellen, Datasets hinzufügen oder Datasets zuordnen, kann es zur Anzeige der folgenden Fehler kommen:

* „Fehler beim Hinzufügen von Datasets.“
* „Fehler beim Zuordnen von Datasets.“
* „Data Share-Ressource ‚x‘ kann kein Zugriff auf ‚y‘ gewährt werden.“
* „Sie besitzen nicht die erforderlichen Berechtigungen für ‚x‘.“
* „Für mindestens eine der ausgewählten Ressourcen konnten keine Schreibberechtigungen für das Azure Data Share-Konto hinzugefügt werden.“

Wenn Sie keine ausreichenden Berechtigungen für den Azure-Datenspeicher besitzen, wird möglicherweise einer dieser Fehler angezeigt. Weitere Informationen finden Sie unter [Rollen und Anforderungen](concepts-roles-permissions.md). 

Sie benötigen Schreibberechtigungen, um Daten aus einem Azure-Datenspeicher freizugeben oder zu empfangen. Diese Berechtigung ist üblicherweise in der Rolle „Mitwirkender“ enthalten. 

Wenn Sie zum ersten Mal Daten aus dem Azure-Datenspeicher freigeben oder aus diesem empfangen, benötigen Sie außerdem die Berechtigung *Microsoft.Authorization/role assignments/write*. Diese Berechtigung ist üblicherweise in der Rolle „Besitzer“ enthalten. Selbst wenn Sie die Azure Data Store-Ressource erstellt haben, sind Sie nicht automatisch deren Besitzer. 

Wenn Sie über die geeigneten Berechtigungen verfügen, gewährt der Azure Data Share-Dienst der verwalteten Identität der Data Share-Ressource automatisch Zugriff auf den Datenspeicher. Dies kann einige Minuten dauern. Wenn aufgrund dieser Verzögerung ein Fehler auftritt, versuchen Sie es nach einigen Minuten noch mal.

Für eine SQL-basierte Freigabe sind zusätzliche Berechtigungen erforderlich. Informationen zu den Voraussetzungen finden Sie im Artikel zur [Freigabe aus SQL-Quellen](how-to-share-from-sql.md).

## <a name="snapshots"></a>Momentaufnahmen
Es kann verschiedene Gründe haben, dass eine Momentaufnahme nicht erfolgreich erstellt werden kann. Eine ausführliche Fehlermeldung können Sie öffnen, indem Sie die Startzeit der Momentaufnahme und dann den Status jedes Datasets auswählen. 

Fehler bei Momentaufnahmen haben häufig diese Ursachen:

* Data Share fehlt die Berechtigung zum Lesen aus dem Quelldatenspeicher oder zum Schreiben in den Zieldatenspeicher. Weitere Informationen finden Sie unter [Rollen und Anforderungen](concepts-roles-permissions.md). Wenn Sie zum ersten Mal eine Momentaufnahme erstellen, benötigt die Data Share-Ressource möglicherweise einige Minuten, um Zugriff auf den Azure-Datenspeicher zu erhalten. Warten Sie einige Minuten, und versuchen Sie es noch mal.
* Die Data Share-Verbindung mit dem Quell- oder Zieldatenspeicher wird durch eine Firewall blockiert.
* Ein freigegebenes Dataset, ein Quell- oder ein Zieldatenspeicher wurde gelöscht.

Für Speicherkonten kann es zu Fehlern bei der Momentaufnahme kommen, weil zum Zeitpunkt der Momentaufnahmeerstellung eine Datei in der Quelle aktualisiert wird. Als Ergebnis wird möglicherweise eine 0-Byte-Datei im Ziel angezeigt. Nach der Aktualisierung in der Quelle sollten die Momentaufnahmen erfolgreich erstellt werden können.

Für SQL-Quellen kann es aus anderen Gründen zu Fehlern bei Momentaufnahmen kommen:

* Das SQL-Quellskript oder das SQL-Zielskript zum Gewähren der Data Share-Berechtigung wurde nicht ausgeführt. Möglicherweise wird das Skript auch für Azure SQL-Datenbank oder Azure Synapse Analytics (vormals Azure SQL Data Warehouse) unter Verwendung der SQL-Authentifizierung anstelle der Azure Active Directory-Authentifizierung ausgeführt.  
* Der Quelldatenspeicher oder der SQL-Zieldatenspeicher wurde angehalten.
* Der Prozess für Momentaufnahmen oder der Zieldatenspeicher unterstützt keine SQL-Datentypen. Weitere Informationen finden Sie im Artikel zur [Freigabe aus SQL-Quellen](how-to-share-from-sql.md#supported-data-types).
* Der Quelldatenspeicher oder SQL-Zieldatenspeicher ist durch andere Prozesse gesperrt. Durch Azure Data Share erfolgt keine Sperrung dieser Datenspeicher. Vorhandene Sperren für diese Datenspeicher können jedoch zu Fehlern bei der Momentaufnahmeerstellung führen.
* Auf die SQL-Zieltabelle wird mit einer Fremdschlüsseleinschränkung verwiesen. Wenn bei der Momentaufnahmeerstellung eine gleichnamige Zieltabelle vorhanden ist, löscht Azure Data Share die Tabelle und erstellt eine neue Tabelle. Wenn auf die SQL-Zieltabelle mit einer Fremdschlüsseleinschränkung verwiesen wird, kann die Tabelle nicht gelöscht werden.
* Die CSV-Zieldatei wird generiert, aber die Daten können nicht in Excel gelesen werden. Dieses Problem tritt möglicherweise auf, wenn die SQL-Quelltabelle Daten mit Sonderzeichen umfasst. Wählen Sie in Excel die Registerkarte **Daten abrufen** und dann die CSV-Datei aus. Wählen Sie den Dateiursprung **65001: Unicode (UTF-8)** aus, und laden Sie anschließend die Daten.

## <a name="updated-snapshot-schedules"></a>Aktualisierte Zeitpläne für Momentaufnahmen
Nachdem der Datenanbieter den Momentaufnahmezeitplan für die gesendete Freigabe aktualisiert hat, muss der Datenconsumer den vorherigen Momentaufnahmezeitplan deaktivieren. Anschließend muss der aktualisierte Momentaufnahmezeitplan für die empfangene Freigabe aktiviert werden. 

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Freigeben von Daten finden Sie im Tutorial zur [Datenfreigabe](share-your-data.md). 

Weitere Informationen zum Datenempfang finden Sie im Tutorial [Akzeptieren und Empfangen von Daten](subscribe-to-data-share.md).