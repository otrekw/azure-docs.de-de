---
title: Ausführen von privilegierten Containern in einem Azure Red Hat OpenShift-Cluster | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie privilegierte Container zum Überwachen von Sicherheit und Konformität ausführen.
author: makdaam
ms.author: b-lejaku
ms.service: container-service
ms.topic: conceptual
ms.date: 12/05/2019
keywords: aro, openshift, aquasec, twistlock, red hat
ms.openlocfilehash: e1c1dd9f27a207f78dd22e271f6b070c7f92f622
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2020
ms.locfileid: "78271376"
---
# <a name="run-privileged-containers-in-an-azure-red-hat-openshift-cluster"></a>Ausführen von privilegierten Containern in einem Azure Red Hat OpenShift-Cluster

Sie können in Azure Red Hat OpenShift-Clustern keine beliebigen privilegierten Container ausführen.
Zwei Lösungen für die Sicherheitsüberwachung und Konformität können in ARO-Clustern ausgeführt werden.
In diesem Dokument werden die Unterschiede in der Dokumentation von Sicherheitsproduktanbietern zur generischen OpenShift-Bereitstellung beschrieben.


Lesen Sie diese Anweisungen, bevor Sie die Anweisungen des Anbieters befolgen.
Die Abschnittstitel in den unten angegebenen produktspezifischen Schritten beziehen sich direkt auf die Abschnittstitel in der Anbieterdokumentation.

## <a name="before-you-begin"></a>Voraussetzungen

In der Dokumentation der meisten Sicherheitsprodukte wird vorausgesetzt, dass Sie über Clusteradministratorrechte verfügen.
Kundenadministratoren verfügen in Azure Red Hat OpenShift nicht über alle Berechtigungen. Die erforderlichen Berechtigungen zum Ändern der clusterweiten Ressourcen sind beschränkt.

Stellen Sie zunächst sicher, dass der Benutzer beim Cluster als Kundenadministrator angemeldet ist, indem Sie `oc get scc` ausführen. Alle Benutzer, die Mitglieder der Kundenadministratorgruppe sind, verfügen über Berechtigungen zum Anzeigen der Sicherheitskontexteinschränkungen (Security Context Constraints, SCCs) im Cluster.

Stellen Sie als Nächstes sicher, dass die `oc`-Binärversion auf `3.11.154` festgelegt ist.
```
oc version
oc v3.11.154
kubernetes v1.11.0+d4cacc0
features: Basic-Auth GSSAPI Kerberos SPNEGO

Server https://openshift.aqua-test.osadev.cloud:443
openshift v3.11.154
kubernetes v1.11.0+d4cacc0
```

## <a name="product-specific-steps-for-aqua-security"></a>Produktspezifische Schritte für Aqua Security
Die grundlegenden Anweisungen, die geändert werden, finden Sie in der [Dokumentation zur Aqua Security-Bereitstellung](https://docs.aquasec.com/docs/openshift-red-hat). Die hier beschriebenen Schritte werden in Verbindung mit der Dokumentation zur Aqua-Bereitstellung ausgeführt.

Der erste Schritt ist das Versehen der erforderlichen Sicherheitskontexteinschränkungen, die aktualisiert werden sollen, mit entsprechenden Anmerkungen. Diese Anmerkungen verhindern, dass der Synchronisierungspod des Clusters Änderungen an diesen Sicherheitskontexteinschränkungen rückgängig macht.

```
oc annotate scc hostaccess openshift.io/reconcile-protect=true
oc annotate scc privileged openshift.io/reconcile-protect=true
```

### <a name="step-1-prepare-prerequisites"></a>Schritt 1: Vorbereiten der erforderlichen Komponenten
Beachten Sie, dass Sie sich als ARO-Kundenadministrator beim Cluster anmelden müssen (nicht mit der Rolle „Clusteradministrator“).

Erstellen Sie das Projekt und das Dienstkonto.
```
oc new-project aqua-security
oc create serviceaccount aqua-account -n aqua-security
```

Weisen Sie „aqua-account“ mit dem unten angegebenen Befehl anstelle der Rolle „cluster-reader“ die Rolle „customer-admin-cluster“ zu.
```
oc adm policy add-cluster-role-to-user customer-admin-cluster system:serviceaccount:aqua-security:aqua-account
oc adm policy add-scc-to-user privileged system:serviceaccount:aqua-security:aqua-account
oc adm policy add-scc-to-user hostaccess system:serviceaccount:aqua-security:aqua-account
```

Fahren Sie mit den weiteren Anweisungen unter Schritt 1 fort.  Bei diesen Anweisungen geht es um die Einrichtung für die Aqua-Registrierung.

### <a name="step-2-deploy-the-aqua-server-database-and-gateway"></a>Schritt 2: Bereitstellen von Aqua-Server, -Datenbank und -Gateway
Führen Sie die Schritte zum Installieren von „aqua-console.yaml“ aus, die in der Aqua-Dokumentation angegeben sind.

Ändern Sie die bereitgestellte Datei `aqua-console.yaml`.  Entfernen Sie die beiden obersten Objekte mit den Bezeichnungen `kind: ClusterRole` und `kind: ClusterRoleBinding`.  Diese Ressourcen werden nicht erstellt, weil der Kundenadministrator derzeit nicht über die Berechtigung zum Ändern von `ClusterRole`- und `ClusterRoleBinding`-Objekten verfügt.

Die zweite Änderung wird am `kind: Route`-Teil von `aqua-console.yaml` vorgenommen. Ersetzen Sie den folgenden YAML-Code für das `kind: Route`-Objekt in der Datei `aqua-console.yaml`.
```
apiVersion: route.openshift.io/v1
kind: Route
metadata:
  labels:
    app: aqua-web
  name: aqua-web
  namespace: aqua-security
spec:
  port:
    targetPort: aqua-web
  tls:
    insecureEdgeTerminationPolicy: Redirect
    termination: edge
  to:
    kind: Service
    name: aqua-web
    weight: 100
  wildcardPolicy: None
```

Befolgen Sie die restlichen Anweisungen.

### <a name="step-3-login-to-the-aqua-server"></a>Schritt 3: Anmelden beim Aqua-Server
Dieser Abschnitt wurde nicht geändert.  Befolgen Sie die Anleitung in der Aqua-Dokumentation.

Verwenden Sie den unten angegebenen Befehl, um die Adresse der Aqua-Konsole abzurufen.
```
oc get route aqua-web -n aqua-security
```

### <a name="step-4-deploy-aqua-enforcers"></a>Schritt 4: Bereitstellen von Aqua-Enforcern
Legen Sie beim Bereitstellen von Enforcern die folgenden Felder fest:

| Feld          | value         |
| -------------- | ------------- |
| Orchestrator   | OpenShift     |
| ServiceAccount | aqua-account  |
| Project        | aqua-security |

## <a name="product-specific-steps-for-prisma-cloud--twistlock"></a>Produktspezifische Schritte für Prisma Cloud/Twistlock

Die grundlegenden Anweisungen, die wir hier ändern, finden Sie in der [Dokumentation zur Prisma Cloud-Bereitstellung](https://docs.paloaltonetworks.com/prisma/prisma-cloud/19-11/prisma-cloud-compute-edition-admin/install/install_openshift.html).

Beginnen Sie mit der Installation des Tools `twistcli`, wie in den Abschnitten „Installieren von Prisma Cloud“ und „Herunterladen von Prisma Cloud-Software“ beschrieben.

Erstellen eines neuen OpenShift-Projekts
```
oc new-project twistlock
```

Überspringen Sie den optionalen Abschnitt „Pushen der Prisma Cloud-Images in eine private Registrierung“. Bei Azure Red Hat Openshift wird es nicht funktionieren. Verwenden Sie stattdessen die Onlineregistrierung.

Sie können der offiziellen Dokumentation folgen und dabei die unten beschriebenen Korrekturen vornehmen.
Beginnen Sie mit dem Abschnitt „Installieren der Konsole“.

### <a name="install-console"></a>Installieren der Konsole

Bei `oc create -f twistlock_console.yaml` in Schritt 2 erhalten Sie beim Erstellen des Namespace einen Fehler.
Diesen können Sie ignorieren. Der Namespace wurde zuvor mit dem Befehl `oc new-project` erstellt.

Verwenden Sie `azure-disk` für den Speichertyp.

### <a name="create-an-external-route-to-console"></a>Erstellen einer externen Route zur Konsole

Sie können entweder die Dokumentation oder die Anleitung unten befolgen, falls Sie den „oc“-Befehl vorziehen.
Kopieren Sie die folgende Routendefinition in die Datei „twistlock_route.yaml“ auf Ihrem Computer.
```
apiVersion: route.openshift.io/v1
kind: Route
metadata:
  labels:
    name: console
  name: twistlock-console
  namespace: twistlock
spec:
  port:
    targetPort: mgmt-http
  tls:
    insecureEdgeTerminationPolicy: Redirect
    termination: edge
  to:
    kind: Service
    name: twistlock-console
    weight: 100
  wildcardPolicy: None
```
Führen Sie anschließend Folgendes aus:
```
oc create -f twistlock_route.yaml
```

Sie können die URL, die der Twistlock-Konsole zugewiesen ist, mit diesem Befehl abrufen: `oc get route twistlock-console -n twistlock`

### <a name="configure-console"></a>Konfigurieren der Konsole

Befolgen Sie die Anleitung in der Twistlock-Dokumentation.

### <a name="install-defender"></a>Installieren von „Defendern“

Bei `oc create -f defender.yaml` in Schritt 2 erhalten Sie Fehler, wenn Sie die Clusterrolle und die Clusterrollenbindung erstellen.
Sie können diese Fehler ignorieren.

„Defender“ werden nur auf Computeknoten bereitgestellt. Sie müssen sie nicht durch eine Knotenauswahl einschränken.
