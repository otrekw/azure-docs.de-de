---
title: Verwenden von Zugriffsrichtlinien in Azure HPC Cache
description: Erstellen und Anwenden von benutzerdefinierten Zugriffsrichtlinien, um den Clientzugriff auf Speicherziele in Azure HPC Cache einzuschränken
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 12/28/2020
ms.author: v-erkel
ms.openlocfilehash: 795b194eb7cd31e633128c22ddffe808b32e07da
ms.sourcegitcommit: 7e97ae405c1c6c8ac63850e1b88cf9c9c82372da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/29/2020
ms.locfileid: "97802410"
---
# <a name="use-client-access-policies"></a>Verwenden von Clientzugriffsrichtlinien

In diesem Artikel wird erläutert, wie Sie benutzerdefinierte Clientzugriffsrichtlinien für Ihre Speicherziele erstellen und anwenden.

Mithilfe von Clientzugriffsrichtlinien wird gesteuert, wie Clients eine Verbindung mit den Exporten von Speicherzielen herstellen können. Sie können Dinge wie Root Squash und Lese-/Schreibzugriff auf der Clienthost- oder der Netzwerkebene steuern.

Zugriffsrichtlinien werden auf einen Namespacepfad angewendet. Dies bedeutet, dass Sie für zwei verschiedene Exporte in einem NFS-Speichersystem unterschiedliche Zugriffsrichtlinien verwenden können.

Diese Funktion ist für Workflows vorgesehen, in denen Sie steuern müssen, wie verschiedene Gruppen von Clients auf die Speicherziele zugreifen.

Wenn Sie die detaillierte Kontrolle des Speicherzugriffs nicht benötigen, können Sie die Standardrichtlinie verwenden oder die Standardrichtlinie mit Zusatzregeln anpassen.

## <a name="create-a-client-access-policy"></a>Erstellen einer Clientzugriffsrichtlinie

Verwenden Sie die Seite **Clientzugriffsrichtlinien** im Azure-Portal, um Richtlinien zu erstellen und zu verwalten. <!-- is there AZ CLI for this? -->

[![Screenshot der Seite für Clientzugriffsrichtlinien. Es sind mehrere Richtlinien definiert, und einige sind aufgeklappt, um ihre Regeln anzuzeigen.](media/policies-overview.png)](media/policies-overview.png#lightbox)

Jede Richtlinie besteht aus Regeln. Die Regeln werden auf Hosts in der Reihenfolge vom kleinsten (Host) bis zum größten Umfang (Standard) angewendet. Die erste übereinstimmende Regel wird angewendet, und spätere Regeln werden ignoriert.

Zum Erstellen einer neuen Zugriffsrichtlinie klicken Sie oben in der Liste auf die Schaltfläche **+ Zugriffsrichtlinie hinzufügen**. Geben Sie der neuen Zugriffsrichtlinie einen Namen, und geben Sie mindestens eine Regel ein.

![Screenshot des Blatts „Zugriffsrichtlinien bearbeiten“ mit mehreren ausgefüllten Regeln. Klicken Sie auf „OK“, um die Regel zu speichern.](media/add-policy.png)

Im Rest dieses Abschnitts werden die Werte erläutert, die Sie in Regeln verwenden können.

### <a name="scope"></a>`Scope`

Der Bereichsausdruck und der Adressfilter definieren gemeinsam, welche Clients von der Regel betroffen sind.

Verwenden Sie sie, um anzugeben, ob sich eine Regel auf einen einzelnen Client (Host), einen Bereich von IP-Adressen (Netzwerk) oder alle Clients (Standard) bezieht.

Wählen Sie für **Bereich** den passenden Wert für Ihre Regel aus:

* **Host**: Die Regel gilt für einen einzelnen Client.
* **Netzwerk**: Die Regel gilt für Clients in einem Bereich von IP-Adressen.
* **Standard**: Die Regel gilt für alle Clients.

Regeln in einer Richtlinie werden in dieser Reihenfolge ausgewertet. Nachdem eine Cienteinbindungsanforderung mit einer Regel übereinstimmt, werden die anderen ignoriert.

### <a name="address-filter"></a>Adressfilter

Der Wert von **Adressfilter** gibt an, welche Clients der Regel entsprechen.

Wenn Sie den Bereich auf **Host** festlegen, können Sie nur eine IP-Adresse im Filter angeben. Für die Bereichseinstellung **Standard** können Sie keine IP-Adressen im Feld **Adressfilter** eingeben, da der Standardbereich mit allen Clients übereinstimmt.

Geben Sie die IP-Adresse oder den Bereich von IP-Adressen für diese Regel an. Verwenden Sie die CIDR-Notation (Beispiel: 0.1.0.0/16), um einen Adressbereich anzugeben.

### <a name="access-level"></a>Zugriffsebene

Legen Sie fest, welche Berechtigungen den Clients erteilt werden sollen, die dem Bereich und dem Filter entsprechen.

Die Optionen sind **Lese-/Schreib-** , **schreibgeschützter** oder **kein Zugriff**.

### <a name="suid"></a>SUID

Aktivieren Sie das Feld **SUID**, um zuzulassen, dass Dateien im Speicher beim Zugriff Benutzer-IDs festlegen.

SUID wird normalerweise verwendet, um die Berechtigungen eines Benutzers vorübergehend heraufzusetzen, sodass der Benutzer eine Aufgabe ausführen kann, die mit dieser Datei zusammenhängt.

### <a name="submount-access"></a>Submountzugriff

Aktivieren Sie dieses Feld, um zuzulassen, dass die angegebenen Clients die Unterverzeichnisse dieses Exports direkt einbinden.

### <a name="root-squash"></a>Root Squash

Wählen Sie aus, ob für Clients, die dieser Regel entsprechen, Root Squash festgelegt werden soll.

Mit diesem Wert können Sie Root Squash auf der Ebene des Speicherexports zulassen. Darüber hinaus können Sie [Root Squash auf der Cacheebene festlegen](configuration.md#configure-root-squash).

Wenn Sie Root Squash aktivieren, müssen Sie außerdem den Benutzerwert für die anonyme ID auf eine dieser Optionen festlegen:

* **-2** (niemand)
* **65534** (niemand)
* **-1** (kein Zugriff)
* **65535** (kein Zugriff)
* **0** (Root ohne Berechtigungen)

## <a name="update-access-policies"></a>Aktualisieren von Zugriffsrichtlinien

Sie können Zugriffsrichtlinien in der Tabelle auf der Seite **Clientzugriffsrichtlinien** bearbeiten oder löschen.

Klicken Sie auf den Namen einer Richtlinie, um sie für die Bearbeitung zu öffnen.

Zum Löschen einer Richtlinie markieren Sie in der Liste das Kontrollkästchen neben ihrem Namen, und klicken Sie dann auf die Schaltfläche **Löschen** ganz oben in der Liste. Die Richtlinie mit dem Namen „Standard“ kann nicht gelöscht werden.

> [!NOTE]
> Zugriffsrichtlinien, die aktuell verwendet werden, können nicht gelöscht werden. Entfernen Sie eine Richtlinie aus allen Namespacepfaden, die sie enthalten, bevor Sie versuchen, sie zu löschen.

## <a name="next-steps"></a>Nächste Schritte

* Anwenden von Zugriffsrichtlinien in den Namespacepfaden für Ihre Speicherziele. Informationen zur Vorgehensweise finden Sie unter [Einrichten des aggregierten Namespace](add-namespace-paths.md).
