---
title: Bereitstellen einer Anwendung aus dem Quellcode in Azure Red Hat OpenShift
description: Erfahren Sie, wie Sie eine Anwendung aus dem Quellcode unter Anwendung der S2I-Buildstrategie (Source-to-Image) in Azure Red Hat OpenShift bereitstellen.
author: sabbour
ms.author: asabbour
ms.service: azure-redhat-openshift
ms.topic: conceptual
ms.date: 4/5/2021
keywords: ARO, OpenShift, Red Hat, S2I, Source-to-Image
ms.openlocfilehash: 270e5d915e2a77cdb4377a616abc97a836a09710
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107558702"
---
# <a name="deploy-an-application-from-source-to-azure-red-hat-openshift"></a>Bereitstellen einer Anwendung aus dem Quellcode in Azure Red Hat OpenShift

In diesem Artikel stellen Sie unter Verwendung eines S2I-Builds (Source-to-Image) eine Anwendung aus dem Quellcode in einem Azure Red Hat OpenShift-Cluster bereit. [Source-to-Image (S2I)](https://docs.openshift.com/container-platform/4.6/builds/understanding-image-builds.html#builds-strategy-s2i-build_understanding-image-builds) ist ein Buildprozess zum Erstellen reproduzierbarer Containerimages aus dem Quellcode. S2I erzeugt sofort einsatzbereite Images, indem Quellcode in ein Containerimage injiziert wird und der Container diesen Quellcode für die Ausführung vorbereitet. Sie können mithilfe von OpenShift eine Anwendung für die Bereitstellung über den Quellcode erstellen, damit Sie nicht bei jeder Änderung manuell einen Container erstellen müssen. OpenShift kann anschließend bei der Benachrichtigung über Quellcodeänderungen neue Versionen automatisch erstellen und bereitstellen.

## <a name="before-you-begin"></a>Voraussetzungen

[!INCLUDE [aro-howto-beforeyoubegin](includes/aro-howto-before-you-begin.md)]

## <a name="create-a-project"></a>Erstellen eines Projekts

Führen Sie zum Erstellen eines neuen Projekts mit dem Namen `demoproject` diesen Befehl aus:

```azurecli-interactive
oc new-project demoproject
```

Daraufhin sollte eine Ausgabe ähnlich der folgenden angezeigt werden:

```output
Now using project "demoproject" on server "https://api.wzy5hg7x.eastus.aroapp.io:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app django-psql-example

to build a new example application in Python. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=gcr.io/hello-minikube-zero-install/hello-node
```

## <a name="launch-the-web-console"></a>Starten der Webkonsole

Ermitteln Sie die URL der Cluster-Webkonsole, indem Sie den folgenden Befehl ausführen:

```azurecli-interactive
 az aro show \
    --name <cluster name> \
    --resource-group <resource group> \
    --query "consoleProfile.url" -o tsv
```

Die Ausgabe sollte eine URL wie die folgende enthalten.

```output
https://console-openshift-console.apps.wzy5hg7x.eastus.aroapp.io/
```

Starten Sie die Konsolen-URL in einem Browser, und melden Sie sich mit den `kubeadmin`-Anmeldeinformationen an.

:::image type="content" source="media/login.png" alt-text="Azure Red Hat OpenShift-Anmeldebildschirm":::

Wechseln Sie im links angezeigten Menü von der Ansicht *Administrator* zur Ansicht *Entwickler*, und wählen Sie in der Liste der Projekte `demoproject` aus. Sie sollten zur Seite *Topologie* für das Projekt umgeleitet werden.

:::image type="content" source="media/s2i/project-topology.png" alt-text="Azure Red Hat OpenShift-Projekttopologie":::

Da das Projekt leer ist, sollten keine Workloads vorhanden sein, und Ihnen werden verschiedene Optionen zum Bereitstellen einer Anwendung angezeigt.

## <a name="deploying-using-the-web-console"></a>Bereitstellung über die Webkonsole

Wählen Sie aus den angezeigten Optionen für das Bereitstellen einer Anwendung die Option *Aus Git* aus. Hierdurch gelangen Sie zur Seite *Import aus Git*. Verwenden Sie `https://github.com/sclorg/django-ex.git` als **Git-Repository-URL**. Die Beispielwebanwendung wird unter Verwendung der Programmiersprache Python implementiert.

:::image type="content" source="media/s2i/from-git.png" alt-text="Azure Red Hat OpenShift-Projekt aus Git":::

> [!NOTE]
> OpenShift erkennt, dass es sich um ein Python-Projekt handelt und wählt das geeignete Builderimage aus.

Scrollen Sie nach unten zu *Erweiterte Optionen*, und stellen Sie sicher, dass **Route zur Anwendung erstellen** aktiviert ist. Durch diese Aktion wird eine OpenShift-Route erstellt. Diese ermöglicht es, einen Dienst durch die Zuweisung eines extern erreichbaren Hostnamens verfügbar zu machen.

:::image type="content" source="media/s2i/from-git-route.png" alt-text="Azure Red Hat OpenShift-Projekt aus Git: Route einrichten":::

Wenn Sie fertig sind, klicken Sie unten auf der Seite auf **Erstellen**. Hierdurch werden Ressourcen erstellt, die zum Verwalten des Builds und zur Bereitstellung der Anwendung benötigt werden. Anschließend werden Sie zur Topologieübersicht für das Projekt umgeleitet.

:::image type="content" source="media/s2i/demo-app-topology.png" alt-text="Azure Red Hat OpenShift-Projekt aus Git: Topologie":::

Die Topologieübersicht liefert eine visuelle Darstellung der Anwendung, die Sie bereitgestellt haben. Diese Ansicht zeigt die Gesamtstruktur der Anwendung.

Wenn Sie auf das Git-Symbol klicken, gelangen Sie zu dem Git-Repository, aus dem der Quellcode für die Anwendung kompiliert wurde. Das unten links angezeigte Symbol zeigt den Buildstatus der Anwendung. Wenn Sie auf dieses Symbol klicken, gelangen Sie zum Abschnitt mit den Builddetails. Wenn die Anwendung Routen verfügbar gemacht hat, können Sie auf das Symbol oben rechts klicken, um die URL für die erstellte Anwendungsroute zu öffnen.

Während die Anwendung herauf- oder herunterskaliert wird, beim Start von Rollouts und während der Neuerstellung von Pods wird die Anwendungsdarstellung in der Topologieansicht animiert, um Ihnen eine Echtzeitanzeige der ausgeführten Aufgaben zu bieten.

Wenn Sie auf das Anwendungssymbol klicken, werden weitere Details angezeigt (siehe unten).

:::image type="content" source="media/s2i/demo-app-details.png" alt-text="Azure Red Hat OpenShift-Projekt aus Git: Details":::

## <a name="viewing-the-builder-logs"></a>Anzeigen der Builderprotokolle

Klicken Sie nach dem Start des Builds im Bereich *Ressourcen* auf den Link *Protokolle anzeigen*.

:::image type="content" source="media/s2i/demo-app-build-logs.png" alt-text="Azure Red Hat OpenShift-Projekt aus Git: Buildprotokolle":::

Auf diese Weise können Sie den Fortschritt des Builds während der Ausführung überwachen. Das Builderimage, in diesem Fall Python, injiziert den Anwendungsquellcode in das endgültige Image ein, bevor es in die interne OpenShift-Imageregistrierung gepusht wird. Der Build wurde erfolgreich erstellt, wenn die Abschlussmeldung „Push erfolgreich“ angezeigt wird.

## <a name="accessing-the-application"></a>Zugreifen auf die Anwendung

Nachdem der Build für das Anwendungsimage erstellt wurde, wird er bereitgestellt.

Klicken Sie im links angezeigten Menü auf *Topologie*, um zur Topologieübersicht für das Projekt zurückzukehren. Wenn Sie die Anwendung mithilfe der Webkonsole erstellt haben, wurde für die Anwendung automatische eine *Route* erstellt, und diese wird außerhalb des Clusters verfügbar gemacht. Die URL, mit der über einen Webbrowser auf die Anwendung zugegriffen kann, entspricht der URL, die zuvor auf der Registerkarte *Ressourcen* angezeigt wurde.

Sie können von der Topologieansicht zur URL für die bereitgestellte Anwendung wechseln, indem Sie auf das Symbol oben rechts am Ring klicken. Wenn Sie nach Abschluss der Bereitstellung auf das Symbol klicken, sollte Ihnen die bereitgestellte Anwendung angezeigt werden.

:::image type="content" source="media/s2i/demo-app-browse.png" alt-text="Azure Red Hat OpenShift-Projekt aus Git: App anzeigen":::

## <a name="deploying-using-the-command-line"></a>Bereitstellung über die Befehlszeile

Sie wissen bereits, wie Sie eine Anwendung mithilfe der Webkonsole bereitstellen. Stellen wir jetzt dieselbe Webanwendung erneut bereit, aber dieses Mal unter Verwendung des Befehlszeilentools `oc`.

Führen Sie den folgenden Befehl aus, um das Projekt zu löschen und von vorn zu beginnen:

```azurecli-interactive
oc delete project demoproject
```

Sie sollten in einer Bestätigungsmeldung darüber informiert werden, dass `demoproject` gelöscht wurde.

```output
project.project.openshift.io "demoproject" deleted
```

Erstellen Sie `demoproject` erneut, indem Sie den folgenden Befehl ausführen:

```azurecli-interactive
oc new-project demoproject
```

Erstellen Sie innerhalb des Projekts eine neue Anwendung aus dem Quellcode auf GitHub, und geben Sie dabei den S2I-Builder für die neueste Version von Python an.

```azurecli-interactive
oc new-app python:latest~https://github.com/sclorg/django-ex.git
```

Dies sollte zu einer Ausgabe ähnlich der folgenden führen:

```output
--> Found image 8ec6f0d (4 weeks old) in image stream "openshift/python" under tag "latest" for "python:latest"

    Python 3.8
    ----------
   [...]

    Tags: builder, python, python38, python-38, rh-python38

    * A source build using source code from https://github.com/sclorg/django-ex.git will be created
      * The resulting image will be pushed to image stream tag "django-ex:latest"
      * Use 'oc start-build' to trigger a new build
    * This image will be deployed in deployment config "django-ex"
    * Port 8080/tcp will be load balanced by service "django-ex"
      * Other containers can access this service through the hostname "django-ex"

--> Creating resources ...
    imagestream.image.openshift.io "django-ex" created
    buildconfig.build.openshift.io "django-ex" created
    deploymentconfig.apps.openshift.io "django-ex" created
    service "django-ex" created
--> Success
    Build scheduled, use 'oc logs -f bc/django-ex' to track its progress.
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose svc/django-ex'
    Run 'oc status' to view your app.
```

OpenShift verwendet den Namen des Git-Repositorys als Name für die Anwendung. Überprüfen Sie durch Ausführung des folgenden Befehls den Status von Build und Bereitstellung:

```azurecli-interactive
oc status
```

Wenn Build und Bereitstellung abgeschlossen sind, sollte eine Ausgabe ähnlich der folgenden angezeigt werden.

```output
In project demoproject on server https://api.wzy5hg7x.eastus.aroapp.io:6443

svc/django-ex - 172.30.200.50:8080
  dc/django-ex deploys istag/django-ex:latest <-
    bc/django-ex source builds https://github.com/sclorg/django-ex.git on openshift/python:latest
    deployment #1 deployed about a minute ago - 1 pod


2 infos identified, use 'oc status --suggest' to see details.
```

Um die Anwendung außerhalb des OpenShift-Clusters verfügbar zu machen, müssen Sie eine Route erstellen. Führen Sie dazu den folgenden Befehl aus:

```azurecli-interactive
oc expose service/django-ex
```

Sie sollten eine Bestätigung erhalten.

```output
route.route.openshift.io/django-ex exposed
```

Rufen Sie die URL ab, indem Sie den folgenden Befehl ausführen:

```azurecli-interactive
oc get route django-ex
```

Als Rückgabe sollte der Hostname angezeigt werden, der der Route zugewiesen ist, mit deren Hilfe Sie zur bereitgestellten Anwendung navigieren können.

```output
NAME        HOST/PORT                                              PATH   SERVICES    PORT       TERMINATION   WILDCARD
django-ex   django-ex-demoproject.apps.wzy5hg7x.eastus.aroapp.io          django-ex   8080-tcp                 None
```

## <a name="triggering-a-new-binary-build"></a>Auslösen eines neuen binären Builds

Während Sie an der Anwendung arbeiten, werden Sie wahrscheinlich Änderungen vornehmen und möchten diese bereitstellen. Sie können ganz einfach einen Webhook einrichten, der bei jedem Codecommit einen neuen Build und eine neue Bereitstellung auslöst. Wenn Sie die Änderungen hingegen sehen möchten, ohne jede Codeänderung in das Repository pushen zu müssen, ist dieses Verhalten möglicherweise nicht erwünscht.

In Fällen, in denen Sie Änderungen schnell durchlaufen, können Sie einen sogenannten „binären Build“ verwenden. Um dieses Szenario zu veranschaulichen, klonen Sie über den folgenden Befehl das Git-Repository lokal für die Anwendung:

```azurecli-interactive
git clone https://github.com/sclorg/django-ex.git
```

Es wird ein Unterverzeichnis mit dem Namen `django-ex` erstellt, das den Quellcode für die Anwendung enthält:

```output
Cloning into 'django-ex'...
remote: Enumerating objects: 980, done.
remote: Total 980 (delta 0), reused 0 (delta 0), pack-reused 980
Receiving objects: 100% (980/980), 276.23 KiB | 4.85 MiB/s, done.
Resolving deltas: 100% (434/434), done.
```

Wechseln Sie in das Unterverzeichnis:

```azurecli-interactive
cd django-ex
```

Öffnen Sie den integrierten Azure Cloud Shell-Editor:

```azurecli-interactive
code welcome/templates/welcome/index.html
```

Scrollen Sie nach unten, und ändern Sie die Zeile `Welcome to your Django application on OpenShift` in `Welcome to Azure Red Hat OpenShift`. Speichern Sie die Datei, und schließen Sie den Editor über das rechts oben angezeigte Menü `...`.

:::image type="content" source="media/s2i/cloudshell-editor.png" alt-text="Azure Red Hat OpenShift-Projekt aus Git: Anwendung im Azure Cloud Shell-Editor bearbeiten":::

Starten Sie einen neuen Build, indem Sie den folgenden Befehl ausführen:

```azurecli-interactive
oc start-build django-ex --from-dir=. --wait
```

Wenn Sie das Flag `--from-dir=.` übergeben, lädt die OpenShift-Befehlszeile den Quellcode aus dem angegebenen Verzeichnis hoch und leitet den Build- und Bereitstellungsvorgang ein. Sie sollten eine Ausgabe ähnlich der folgenden erhalten, und nach einigen Minuten sollte der Buildvorgang abgeschlossen sein.

```output
Uploading directory "." as binary input for the build ...
.
Uploading finished
build.build.openshift.io/django-ex-2 started
```

Wenn Sie den Browser mit der Anwendung aktualisieren, sollte der aktualisierte Titel angezeigt werden.

:::image type="content" source="media/s2i/demo-app-browse-updated.png" alt-text="Azure Red Hat OpenShift-Projekt aus Git: Aktualisierte App anzeigen":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit der Anwendung fertig sind, können Sie den folgenden Befehl ausführen, um das Projekt zu löschen:

```azurecli-interactive
oc delete project demoproject
```

Sie können außerdem den Cluster löschen, indem Sie die Anweisungen in [Tutorial: Löschen eines Azure Red Hat OpenShift 4-Clusters](./tutorial-delete-cluster.md) befolgen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Leitfaden haben Sie Folgendes gelernt:
> [!div class="checklist"]
>
> * Erstellen eines Projekts
> * Bereitstellen einer Anwendung aus dem Quellcode unter Verwendung der Webkonsole
> * Bereitstellen einer Anwendung aus dem Quellcode unter Verwendung der OpenShift-Befehlszeile
> * Auslösen eines binären Builds unter Verwendung der OpenShift-Befehlszeile

Weitere Informationen zum Erstellen und Bereitstellen von Anwendungen mithilfe der S2I- und [anderer Buildstrategien](https://docs.openshift.com/container-platform/4.6/builds/understanding-image-builds.html).
