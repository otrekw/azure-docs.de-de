---
title: Bereitstellen einer Anwendung in Azure Red Hat OpenShift mit OpenShift Serverless
description: Hier erfahren Sie, wie Sie eine Anwendung mithilfe von OpenShift Serverless in Azure Red Hat OpenShift bereitstellen.
author: sabbour
ms.author: asabbour
ms.service: azure-redhat-openshift
ms.topic: conceptual
ms.date: 4/5/2021
keywords: ARO, OpenShift, Red Hat, serverlos
ms.openlocfilehash: 6db87e752745e3df919c93b2ffc8144e5886b319
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107532809"
---
# <a name="deploy-an-application-to-azure-red-hat-openshift-using-openshift-serverless"></a>Bereitstellen einer Anwendung in Azure Red Hat OpenShift mit OpenShift Serverless

In diesem Artikel stellen Sie mithilfe von [OpenShift Serverless](https://www.openshift.com/learn/topics/serverless) eine Anwendung in einem Azure Red Hat OpenShift-Cluster bereit. OpenShift Serverless ermöglicht Entwicklern das Bereitstellen und Ausführen von Anwendungen, die bei Bedarf hoch- oder auf Null herunterskaliert werden, sodass keine Ressourcen verbraucht werden, wenn die Anwendung nicht genutzt wird.

Der Anwendungscode kann zusammen mit den entsprechenden Runtimes in einem Container gepackt werden, und die serverlose Funktion startet die Anwendungscontainer, wenn sie durch ein Ereignis ausgelöst werden. Anwendungen können durch Ereignisse aus verschiedenen Quellen ausgelöst werden, z. B. durch Ereignisse Ihrer eigenen Anwendungen, von Clouddiensten mehrerer Anbieter, SaaS-Systemen (Software-as-a-Service) und anderen Diensten.

Die Funktionen zur Verwaltung aller Aspekte der serverlosen Bereitstellung eines Containers sind direkt in die OpenShift-Schnittstelle integriert. Entwickler können visuell erkennen, durch welche Ereignisse ihre containerisierten Anwendungen gestartet werden, und haben mehrere Möglichkeiten zum Ändern der Ereignisparameter. OpenShift Serverless-Anwendungen können in andere OpenShift-Dienste wie OpenShift Pipelines, Service Mesh und Monitoring integriert werden und ermöglichen so eine vollständige serverlose Anwendungsentwicklung und -bereitstellung.

## <a name="before-you-begin"></a>Voraussetzungen

[!INCLUDE [aro-howto-beforeyoubegin](includes/aro-howto-before-you-begin.md)]

### <a name="install-the-knative-command-line-tool-kn"></a>Installieren des Knative-Befehlszeilentools (kn)

Sie können die für Ihren Computer geeignete aktuelle Version der Befehlszeilenschnittstelle (Command Line Interface, CLI) von <https://github.com/knative/client/releases/> herunterladen.

Falls Sie die Befehle in Azure Cloud Shell ausführen, laden Sie die aktuelle Knative-CLI für Linux herunter.

```azurecli-interactive
cd ~
wget https://github.com/knative/client/releases/download/v0.22.0/kn-linux-amd64

mkdir knative
chmod +x kn-linux-amd64
mv kn-linux-amd64 knative/kn
echo 'export PATH=$PATH:~/knative' >> ~/.bashrc && source ~/.bashrc
```

### <a name="launch-the-web-console"></a>Starten der Webkonsole

Führen Sie den folgenden Befehl aus, um die URL der Clusterwebkonsole zu ermitteln:

```azurecli-interactive
 az aro show \
    --name <cluster name> \
    --resource-group <resource group> \
    --query "consoleProfile.url" -o tsv
```

Bei der Ausgabe sollte es sich um eine URL wie die folgende handeln.

```output
https://console-openshift-console.apps.wzy5hg7x.eastus.aroapp.io/
```

Starten Sie die Konsolen-URL in einem Browser, und melden Sie sich mit den `kubeadmin`-Anmeldeinformationen an.

:::image type="content" source="media/login.png" alt-text="Azure Red Hat OpenShift-Anmeldebildschirm":::

## <a name="install-the-openshift-serverless-operator"></a>Installieren des OpenShift Serverless-Operators

Vergewissern Sie sich nach der Anmeldung bei der OpenShift-Webkonsole, dass die *Administratoransicht* angezeigt wird. Öffnen Sie den *OperatorHub*, suchen Sie nach dem **OpenShift Serverless**-Operator, und wählen Sie ihn aus.

![Suchen nach dem OpenShift Serverless-Operator](media/serverless/serverless-operatorhub.png)

Klicken Sie auf **Installieren**, um die Installationsseite des Operators zu öffnen.

![Auf „Installieren“ klicken, um den Operator zu installieren](media/serverless/serverless-clickinstall.png)

Wählen Sie den entsprechenden *Updatekanal* für die Version Ihres Azure Red Hat OpenShift-Clusters aus, und installieren Sie den Operator im Namespace `openshift-serverless`. Scrollen Sie nach unten, und klicken Sie auf **Installieren**.

![Seite für die Operatorinstallation](media/serverless/serverless-installpage.png)

Nach ein paar Minuten sollte auf der Statusseite angezeigt werden, dass der Operator installiert wurde und einsatzbereit ist. Klicken Sie auf die Schaltfläche **Operator anzeigen**, um fortzufahren.

![Der Operator wurde installiert und ist einsatzbereit.](media/serverless/serverless-installed.png)

## <a name="install-knative-serving"></a>Installieren von Knative Serving

Upstream-Knative ermöglicht die serverlose Ausführung beliebiger Container in OpenShift Serverless. Knative erweitert Kubernetes um eine Reihe von Komponenten für die serverlose Bereitstellung, Ausführung und Verwaltung moderner Anwendungen.

### <a name="create-an-instance-of-the-knative-serving"></a>Erstellen einer Instanz von Knative Serving

Wechseln Sie zum Namespace `knative-serving`, indem Sie oben links auf die Dropdownliste „Projekt“ klicken und dann unter *Provided APIs* (Bereitgestellte APIs) auf der Karte *Knative Serving* die Option **Instanz erstellen** auswählen.

![Klicken, um eine Knative-Dienstinstanz zu erstellen](media/serverless/serverless-createknativeserving.png)

Übernehmen Sie die Standardwerte, scrollen Sie auf der Seite *Create Knative Serving* (Knative Serving-Instanz erstellen) nach unten, und klicken Sie auf die Schaltfläche **Erstellen**.

![Standardwerte übernehmen und auf „Erstellen“ klicken](media/serverless/serverless-createknativeserving2.png)

Warten Sie, bis in der Spalte *Status* der Status **Bereit** angezeigt wird. Nun sollte OpenShift Serverless installiert sein, und Sie können ein OpenShift Serverless-Projekt erstellen.

![Knative Serving bereit](media/serverless/serverless-createknativeserving3.png)

## <a name="create-a-serverless-project"></a>Erstellen eines serverlosen Projekts

Führen Sie den folgenden Befehl aus, um ein neues Projekt mit dem Namen `demoserverless` zu erstellen:

```azurecli-interactive
oc new-project demoserverless
```

Die Ausgabe sollte in etwa wie folgt aussehen:

```output
Now using project "demoserverless" on server "https://api.wzy5hg7x.eastus.aroapp.io:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app django-psql-example

to build a new example application in Python. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=gcr.io/hello-minikube-zero-install/hello-node
```

Wechseln Sie im Menü auf der linken Seite von der *Administratoransicht* zur *Entwickleransicht*, und wählen Sie in der Liste der Projekte `demoserverless` aus. Nun sollte die Seite *Topologie* für das Projekt angezeigt werden.

![Azure Red Hat OpenShift-Projekttopologie](media/serverless/serverless-topology.png)

## <a name="deploying-using-the-web-console"></a>Bereitstellung über die Webkonsole

Wählen Sie in den Optionen, die für die Bereitstellung einer Anwendung angezeigt werden, die Option *From Git* (Aus Git) aus. Dadurch gelangen Sie zur Seite *Import from Git* (Aus Git importieren). Verwenden Sie `https://github.com/sclorg/django-ex.git` als **Git-Repository-URL**. Die Beispielwebanwendung wird mit der Programmiersprache Python implementiert.

![Azure Red Hat OpenShift-Projekt aus Git](media/serverless/serverless-from-git.png)

> [!NOTE]
> OpenShift erkennt, dass es sich um ein Python-Projekt handelt, und wählt das entsprechende Generatorimage aus.

Scrollen Sie nach unten zum Abschnitt *Ressourcen*, und vergewissern Sie sich, dass **Knative Service** (Knative-Dienst) als zu generierender Ressourcentyp ausgewählt ist. Durch diese Aktion wird ein Knative-Dienst erstellt, ein Bereitstellungstyp, der es OpenShift Serverless ermöglicht, im Leerlauf auf Null herunterzuskalieren.

![Azure Red Hat OpenShift-Projekt: Knative](media/serverless/serverless-knative.png)


Wenn Sie fertig sind, klicken Sie unten auf der Seite auf **Erstellen**. Hierdurch werden Ressourcen erstellt, die zum Verwalten des Builds und Bereitstellen der Anwendung benötigt werden. Anschließend werden Sie zur Topologieübersicht für das Projekt umgeleitet.

Die Topologieübersicht ist eine visuelle Darstellung der von Ihnen bereitgestellten Anwendung. In dieser Ansicht sehen Sie die Gesamtstruktur der Anwendung.

Warten Sie, bis der Buildvorgang abgeschlossen wird. Dies kann einige Minuten dauern. Nach Abschluss des Buildvorgangs wird in der unteren linken Ecke des Diensts ein grünes Häkchen angezeigt.

![Azure Red Hat OpenShift-Projekttopologie: Build erfolgreich](media/serverless/serverless-ready.png)

## <a name="see-your-application-scale"></a>Anzeigen der Skalierung Ihrer Anwendung

Klicken Sie oben in der Topologieansicht in der Liste *Anzeigeoptionen* auf *Podanzahl*. Warten Sie, bis die Podanzahl auf 0 Pods herunterskaliert wurde. Das Herunterskalieren kann einige Minuten dauern.

![Azure Red Hat OpenShift-Projekttopologie: Skaliert auf Null](media/serverless/serverless-scaledtozero.png)

Klicken Sie in der oberen rechten Ecke des Bereichs für den Knative-Dienst auf das Symbol *URL öffnen*. Die Anwendung wird auf einer neuen Registerkarte geöffnet. Schließen Sie die neue Browserregisterkarte, und kehren Sie zur Topologieansicht zurück. In der Topologieansicht können Sie sehen, dass die Anwendung auf einen Pod hochskaliert wurde, um Ihre Anforderung zu erfüllen. Nach einigen Minuten wird Ihre Anwendung wieder auf 0 Pods herunterskaliert.

![Azure Red Hat OpenShift-Projekttopologie: Skaliert auf 1](media/serverless/serverless-scaledtoone.png)

## <a name="forcing-a-new-revision-and-setting-traffic-distribution"></a>Erzwingen einer neuen Revision und Festlegen der Datenverkehrsverteilung

Bei jeder Aktualisierung der Konfiguration eines Diensts wird eine neue Revision für den Dienst erstellt. Die Dienstroute leitet den gesamten Datenverkehr standardmäßig zur neusten Revision, die bereit ist. Sie können dieses Verhalten ändern, indem Sie definieren, welcher Revision ein Teil des Datenverkehrs zugeordnet wird. Knative-Dienste ermöglichen die Datenverkehrszuordnung, d. h. Revisionen eines Diensts können einem zugewiesenen Teil des Datenverkehrs zugeordnet werden. Die Datenverkehrszuordnung bietet auch die Möglichkeit, eindeutige URLs für bestimmte Revisionen zu erstellen.

Klicken Sie in der *Topologie* auf die Revision in Ihrem Dienst, um die Details anzuzeigen. Die Badges unter dem Pod-Ring und oben im Detailbereich sollten `(REV)` lauten. Scrollen Sie im Seitenbereich auf der Registerkarte *Ressourcen* nach unten, und klicken Sie auf die Konfiguration, die Ihrem Dienst zugeordnet ist.

![Azure Red Hat OpenShift-Projekttopologie: Revision](media/serverless/serverless-revdetails.png)

Erzwingen Sie ein Konfigurationsupdate, indem Sie zur Registerkarte *YAML* wechseln, nach unten scrollen und den Wert von `timeoutSeconds` in `301` ändern. Klicken Sie auf **Speichern**, um die aktualisierte Konfiguration zu speichern. In der Praxis kann ein solches Konfigurationsupdate auch durch Aktualisieren des Containerimagetags ausgelöst werden.

![Erzwingen einer neuen Revision durch Aktualisieren der Konfiguration](media/serverless/serverless-confupdate.png)

Gehen Sie zurück zur *Topologieansicht*. Dort können Sie sehen, dass eine neue Revision bereitgestellt wurde. Klicken Sie auf den Dienst, der mit dem Badge `(KSVC)`endet, und dann auf die Schaltfläche **Set Traffic Distribution** (Datenverkehrsverteilung festlegen). Nun sollten Sie den Datenverkehr zwischen den verschiedenen Revisionen des Diensts aufteilen können.

![Festlegen der Datenverkehrsverteilung](media/serverless/serverless-trafficdist.png)

In der *Topologieansicht* wird jetzt angezeigt, wie der Datenverkehr zwischen den beiden Revisionen verteilt wird.

![Überprüfen der Datenverkehrsverteilung](media/serverless/serverless-trafficdist2.png)

## <a name="using-the-knative-command-line-tool-kn"></a>Verwenden des Knative-Befehlszeilentools (kn)

In den vorherigen Schritten haben Sie die OpenShift-Webkonsole verwendet, um eine Anwendung zu erstellen und in OpenShift Serverless bereitzustellen. Da OpenShift Serverless Knative im Hintergrund ausführt, können Sie Knative-Dienste auch mithilfe des Knative-Befehlszeilentools (kn) erstellen.

> [!NOTE]
> Falls Sie die `kn`-CLI noch nicht installiert haben, führen Sie die Schritte im Abschnitt „Voraussetzungen“ dieses Artikels aus. Stellen Sie auch sicher, dass Sie sich mit dem OpenShift-Befehlszeilentool `oc` angemeldet haben.

In diesem Artikel verwenden wir ein Containerimage, das bereits unter `quay.io/rhdevelopers/knative-tutorial-greeter` erstellt wurde.

### <a name="deploy-a-service"></a>Bereitstellen eines Diensts

Führen Sie den folgenden Befehl aus, um den Dienst bereitzustellen:

```azurecli-interactive
kn service create greeter \
--image quay.io/rhdevelopers/knative-tutorial-greeter:quarkus \
--namespace demoserverless \
--revision-name greeter-v1
```

Die Ausgabe sollte in etwa wie folgt aussehen.

```output
Creating service 'greeter' in namespace 'demoserverless':

  0.044s The Route is still working to reflect the latest desired specification.
  0.083s ...
  0.114s Configuration "greeter" is waiting for a Revision to become ready.
 10.420s ...
 10.489s Ingress has not yet been reconciled.
 10.582s Waiting for load balancer to be ready
 10.763s Ready to serve.

Service 'greeter' created to latest revision 'greeter-v1' is available at URL:
http://greeter-demoserverless.apps.wzy5hg7x.eastus.aroapp.io
```

Mit dem folgenden Befehl können Sie eine Liste der Routen im Projekt abrufen:

```azurecli-interactive
kn route list
```

In der Ausgabe wird eine Liste der Routen im Namespace zurückgegeben. Öffnen Sie die URL in einem Browser, um den bereitgestellten Dienst anzuzeigen.

```output
NAME      URL                                                            READY
greeter   http://greeter-demoserverless.apps.wzy5hg7x.eastus.aroapp.io   True
```

### <a name="deploy-a-new-version-of-the-service"></a>Bereitstellen einer neuen Version des Diensts

Stellen Sie eine neue Version der Anwendung bereit, indem Sie den folgenden Befehl ausführen und dabei das Imagetag `:latest` sowie eine Umgebungsvariable `MESSAGE_PREFIX` übergeben:

```azurecli-interactive
kn service update greeter \
 --image quay.io/rhdevelopers/knative-tutorial-greeter:latest \
 --namespace demoserverless \
 --env MESSAGE_PREFIX=GreeterV2 \
 --revision-name greeter-v2
```

Sie erhalten eine Bestätigung, dass eine neue Revision `greeter-v2` bereitgestellt wurde.

```output
Updating Service 'greeter' in namespace 'demoserverless':

  5.029s Traffic is not yet migrated to the latest revision.
  5.086s Ingress has not yet been reconciled.
  5.190s Waiting for load balancer to be ready
  5.332s Ready to serve.

Service 'greeter' updated to latest revision 'greeter-v2' is available at URL:
http://greeter-demoserverless.apps.wzy5hg7x.eastus.aroapp.io
```

Führen Sie den folgenden Befehl aus, um eine Liste aller Revisionen und deren Datenverkehrsverteilung anzuzeigen:

```azurecli-interactive
kn revision list
```

Sie erhalten eine Liste, die in etwa wie das folgende Beispiel aussieht. Beachten Sie, dass der neuen Revision 100 % des Datenverkehrs zugeordnet wird.

```output
NAME            SERVICE   TRAFFIC   TAGS   GENERATION   AGE     CONDITIONS   READY   REASON
greeter-v2      greeter   100%             2            90s     3 OK / 4     True
greeter-v1      greeter                    1            5m32s   3 OK / 4     True
```

### <a name="bluegreen-and-canary-deployments"></a>Blue/Green- und Canary-Bereitstellungen

Wenn eine neue Revision bereitgestellt wird, wird dieser standardmäßig 100 % des Datenverkehrs zugeordnet. Angenommen, Sie möchten eine Blue/Green-Bereitstellungsstrategie implementieren, bei der Sie schnell ein Rollback auf die ältere Version der Anwendung ausführen können. Mit Knative ist dies ganz einfach.

Aktualisieren Sie den Dienst, um drei Datenverkehrstags zu erstellen und 100 % des Datenverkehrs entsprechend zu senden.

- **current**: Zeigt auf die aktuell bereitgestellte Version.
- **prev**: Zeigt auf die vorherige Version.
- **latest**: Zeigt immer auf die aktuelle Version.

```azurecli-interactive
kn service update greeter \
   --tag greeter-v2=current \
   --tag greeter-v1=prev \
   --tag @latest=latest
```

Sie erhalten eine Bestätigung, die in etwa wie folgt aussieht.

```output
Updating Service 'greeter' in namespace 'demoserverless':

  0.037s Ingress has not yet been reconciled.
  0.121s Waiting for load balancer to be ready
  0.287s Ready to serve.

Service 'greeter' with latest revision 'greeter-v2' (unchanged) is available at URL:
http://greeter-demoserverless.apps.wzy5hg7x.eastus.aroapp.io
```

Listen Sie die Routen mit dem folgenden Befehl auf:

```azurecli-interactive
kn route describe greeter
```

Sie erhalten eine Ausgabe, in der die URLs für jedes der Tags zusammen mit der Datenverkehrsverteilung angezeigt werden.

```output
Name:       greeter
Namespace:  demoserverless
Age:        10m
URL:        http://greeter-demoserverless.apps.wzy5hg7x.eastus.aroapp.io
Service:    greeter

Traffic Targets:
  100%  @latest (greeter-v2) #latest
        URL:  http://latest-greeter-demoserverless.apps.wzy5hg7x.eastus.aroapp.io
    0%  greeter-v1 #prev
        URL:  http://prev-greeter-demoserverless.apps.wzy5hg7x.eastus.aroapp.io
    0%  greeter-v2 #current
        URL:  http://current-greeter-demoserverless.apps.wzy5hg7x.eastus.aroapp.io

[..]
```

Angenommen, Sie möchten schnell ein Rollback auf die vorherige Version ausführen. Dazu können Sie die Datenverkehrsverteilung so aktualisieren, dass 100 % des Datenverkehrs an das Tag für die vorherige Version gesendet werden:

```azurecli-interactive
kn service update greeter --traffic current=0 --traffic prev=100
```

Führen Sie den folgenden Befehl aus, um die Routen aufzulisten und erneut zu überprüfen:

```azurecli-interactive
kn route describe greeter
```

Sie erhalten eine Ausgabe, die zeigt, dass der Datenverkehr zu 100 % der vorherigen Version zugeordnet wird.

```output
Name:       greeter
Namespace:  demoserverless
Age:        19m
URL:        http://greeter-demoserverless.apps.wzy5hg7x.eastus.aroapp.io
Service:    greeter

Traffic Targets:
    0%  @latest (greeter-v2) #latest
        URL:  http://latest-greeter-demoserverless.apps.wzy5hg7x.eastus.aroapp.io
  100%  greeter-v1 #prev
        URL:  http://prev-greeter-demoserverless.apps.wzy5hg7x.eastus.aroapp.io
    0%  greeter-v2 #current
        URL:  http://current-greeter-demoserverless.apps.wzy5hg7x.eastus.aroapp.io

[..]
```

Experimentieren Sie mit der Datenverkehrsverteilung, und aktualisieren Sie dabei die Hauptroute (in diesem Fall `http://greeter-demoserverless.apps.wzy5hg7x.eastus.aroapp.io`) in Ihrem Browser.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Anwendung nicht mehr benötigen, können Sie das Projekt mit dem folgenden Befehl löschen:

```azurecli-interactive
oc delete project demoserverless
```

Sie können auch den Cluster löschen, indem Sie die Anweisungen in [Tutorial: Löschen eines Azure Red Hat OpenShift 4-Clusters](./tutorial-delete-cluster.md) befolgen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Leitfaden haben Sie Folgendes gelernt:
> [!div class="checklist"]
>
> * Installieren des OpenShift Serverless-Operators und von Knative Serving
> * Bereitstellen eines serverlosen Projekts mithilfe der Webkonsole
> * Bereitstellen eines serverlosen Projekts mithilfe der Knative-CLI (kn)
> * Konfigurieren von Blue/Green- und Canary-Bereitstellungen mit der Knative-CLI (kn)

Weitere Informationen zum Erstellen und Bereitstellen serverloser, ereignisgesteuerter Anwendungen in Azure Red Hat OpenShift mithilfe von [OpenShift Serverless](https://www.openshift.com/learn/topics/serverless) finden Sie in den Dokumentationen zu den [ersten Schritten mit OpenShift Serverless](https://docs.openshift.com/container-platform/4.6/serverless/serverless-getting-started.html) und zum [Erstellen und Verwalten serverloser Anwendungen](https://docs.openshift.com/container-platform/4.6/serverless/serving-creating-managing-apps.html).
