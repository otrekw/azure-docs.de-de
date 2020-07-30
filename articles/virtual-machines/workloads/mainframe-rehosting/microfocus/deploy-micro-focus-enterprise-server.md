---
title: Bereitstellen von Micro Focus Enterprise Server 5.0 für AKS | Microsoft-Dokumentation
description: Hosten Sie Ihre IBM z/OS-Mainframe-Workloads mithilfe der Micro Focus-Entwicklungs- und Testumgebung neu auf virtuellen Azure-Computern (VMs).
services: virtual-machines-linux
documentationcenter: ''
author: maggsl
ms.author: edprice
manager: edprice
editor: edprice
ms.topic: conceptual
ms.date: 06/29/2020
tags: ''
keywords: ''
ms.service: multiple
ms.openlocfilehash: 6780942d922f885c7afebd8e64f4f28654c3800e
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87042540"
---
# <a name="deploy-micro-focus-enterprise-server-50-to-aks"></a>Bereitstellen von Micro Focus Enterprise Server 5.0 für AKS

In einem anderen [Artikel](./run-enterprise-server-container.md) habe ich die Schritte zum Ausführen von Micro Focus Enterprise Server 5.0 in einem Docker-Container beschrieben. Als Nächstes möchte ich Ihnen nun zeigen, wie Sie das erstellte Docker-Image für Azure Kubernetes Service (AKS) bereitstellen können.

Azure Kubernetes Service ist ein verwalteter Orchestrierungsdienst, der auf Kubernetes basiert. Er ermöglicht die Bereitstellung, Skalierung und Verwaltung von Docker-Containern (und anderen auf Containern basierenden Anwendungen) in einem Cluster aus Containerhosts.

Dieser Prozess umfasst drei Schritte. Sie müssen folgende Schritte durchführen:

1.  Erstellen Sie eine Azure Container Registry-Instanz zum Speichern des Docker-Images.

2.  Erstellen Sie einen Azure Kubernetes-Cluster zum Ausführen des Docker-Images.

3.  Führen Sie die Anwendung aus.

Dadurch können Sie die Modernisierungsworkloads für Ihre Mainframes in Azure aufskalieren (und herunterskalieren) und somit die Cloudplattform optimal nutzen.

Sind Sie bereit? Dann legen wir los!

## <a name="create-the-azure-container-registry"></a>Erstellen der Azure Container Registry-Instanz

Wählen Sie im Azure-Portal oben links **Ressource erstellen** aus. Wählen Sie im Marketplace-Dashboard **Container** und dann **Container Registry** aus. Dadurch gelangen Sie zum Bereich **Containerregistrierung erstellen**. Hier müssen Sie den **Registrierungsnamen**, das **Azure-Abonnement**, die **Ressourcengruppe** und den **Ort** angeben. Der **Registrierungsname** muss aufgelöst werden können und muss daher eindeutig sein. Wählen Sie die im vorherigen Blogbeitrag verwendete **Ressourcengruppe** und den entsprechenden **Ort** aus. Wählen Sie **Aktivieren** für **Administratorbenutzer** und **Basic** für die **SKU** aus. Nachdem Sie alles angegeben haben, wählen Sie **Erstellen** aus.

![Erstellen einer Schnittstelle für die Containerregistrierung](media/deploy-image-1.png)

Nachdem Sie die Registrierung bereitgestellt haben, wählen Sie **Zu Ressource wechseln** aus. Dadurch gelangen Sie zum Hauptblatt für die Azure Container Registry. Hier finden Sie eine nützliche Funktion, nämlich die Menüoption **Schnellstart**. Wählen Sie sie aus, um eine Anleitung zum Pushen und Pullen von Images in und aus der Registrierung anzuzeigen. Sehen wir uns die einzelnen Schritte näher an:

1.  **Docker installieren**: Darum brauchen Sie sich nicht zu kümmern, da dies bereits geschehen ist.

2.  **Basisimage „Hallo Welt“ ausführen**: Auch diese Option müssen Sie nicht ausführen, können sie aber gerne ausprobieren.

3.  **Bei der Containerregistrierung anmelden**: Diesen Schritt müssen Sie über den virtuellen Computer (VM) ausführen. Kopieren Sie den Befehl in die Zwischenablage oder in den Editor.

    Für die Registrierung, die ich erstellt habe, lautet der Befehl **docker login acrmf50.azurecr.io**

4.  **In die Registrierung pushen**: Diesen Schritt müssen Sie für das Micro Focus-Image durchführen, nachdem Sie sich beim virtuellen Computer angemeldet haben.

5.  **Aus der Registrierung pullen**: Dieser Schritt ist für diese Übung nicht relevant. Es ist jedoch gut, ihn zu kennen, falls Sie einmal ein anderes Docker-Image ausführen müssen.

Bevor Sie das Portal verlassen, müssen Sie die Anmeldeinformationen für die Registrierung abrufen, damit Sie sich anmelden können. Schließen Sie das Blatt **Schnellstart**, und wählen Sie im Menü „Registrierung“ die Option **Zugriffsschlüssel** aus. Kopieren Sie den **Benutzernamen** und eines der beiden **Kennwörter** in die Zwischenablage oder in den Editor. Diese werden später zum Anmelden benötigt werden.

Nachdem Sie nun wissen, was Sie tun müssen, melden Sie sich beim virtuellen Computer an.

## <a name="rdp-to-the-virtual-machine-you-used-to-create-the-docker-image"></a>RDP an den virtuellen Computer, den Sie zum Erstellen des Docker-Images verwendet haben

Da Sie das Docker-Image bereits auf einem Windows 2016-Server erstellt haben, müssen Sie sich nun bei diesem virtuellen Computer anmelden. Über diesen virtuellen Computer können Sie das Image an die eben erstellte Azure Container Registry pushen. Navigieren Sie im Azure-Portal zum virtuellen Computer, und wählen Sie **Übersicht** und dann **Verbinden** aus. Dadurch werden Sie über das Remotedesktopprotokoll (RDP) mit dem virtuellen Computer verbunden. Verwenden Sie die Anmeldeinformationen aus dem Schritt, bei dem Sie den virtuellen Computer erstellt haben.

## <a name="log-in-and-push-the-image-to-the-registry"></a>Anmelden und Pushen des Images an die Registrierung

Nachdem Sie sich angemeldet haben, öffnen Sie eine Eingabeaufforderung, und führen Sie die folgenden Docker-Befehle aus:

-   **docker images**: Mit diesem Befehl wird eine Liste mit allen derzeit auf dem virtuellen Computer installierten Images angezeigt. Notieren Sie sich **microfocus/es-acctdemo**, da Sie damit weiterarbeiten werden.

-   **docker login acrmf50.azurecr.io**: Das richtige Format hier sieht wie folgt aus: *docker login \<registry name\>* . Geben Sie anstelle des Platzhalters den Namen ein, den Sie beim Erstellen der Registrierung verwendet haben.

    -   Sie benötigen den **Benutzernamen** und das **Kennwort**, das bzw. den Sie im Azure-Portal kopiert haben. Die Abbildung sollte wie folgt aussehen.

    ![Screenshot: Administratoreingabeaufforderung](media/deploy-image-2.png)

-   **docker tag microfocus/es-acctdemo acrmf50.azurecr.io/es-acctdemo**: Mit diesem Befehl wird das entsprechende Image mit dem Namen des Repositorys versehen. **HINWEIS:** Wenn der Name \<microfocus/es-acctdemo\> nicht funktioniert, versuchen Sie es mit der vollständigen Image-ID. Nachdem Sie den Befehl ausgeführt haben, geben Sie **docker images –no-trunc** ein. Die nächste Abbildung sieht wie folgt aus. Wie Sie sehen, ist das Image ordnungsgemäß gekennzeichnet.

    ![Screenshot zum Auswählen einer Administratoreingabeaufforderung](media/deploy-image-3.png)

-   **docker push acrmf50.azurecr.io/es-acctdemo**: Mit diesem Befehl wird der eigentliche Pushvorgang an das Repository gestartet. Da die Größe 15 GB beträgt, dauert der Vorgang einige Minuten. Wenn alles ordnungsgemäß verläuft, sollte das Ganze nun wie folgt aussehen.

    ![Screenshot: Administratoreingabeaufforderung](media/deploy-image-4.png)

Kehren Sie nun zum **Repository** im Azure-Portal zurück. Wählen Sie im Menü für das **Repository** die Option **Repositorys** aus. Daraufhin wird **es-acctdemo** in einer Liste angezeigt. Erstellen Sie nun das AKS-Cluster.

## <a name="create-the-azure-kubernetes-aks-cluster"></a>Erstellen des AKS-Clusters (Azure Kubernetes Service)

Wählen Sie im Azure-Portal **Ressource erstellen** und anschließend im Menü **Marketplace** die Option **Container/Kubernetes Service** aus. Als Nächstes müssen Sie das Blatt **Kubernetes-Cluster erstellen** ausfüllen. Der Cluster muss sich in der bereits verwendeten Region und Ressourcengruppe befinden. Sie können die restlichen Standardwerte übernehmen, außer dem Wert für die **Knotenanzahl**. Dieser muss lediglich 1 betragen. Wählen Sie abschließend **Überprüfen + Erstellen** aus.

![Screenshot: Erstellen des Kubernetes-Clusters](media/deploy-image-5.png)

Nach Abschluss des Vorgangs werden die **Kubernetes Service**-Artefakte der auf dem Blatt ausgewählten **Ressourcengruppe** hinzugefügt. Für den eigentlichen Cluster wird während der Bereitstellung jedoch eine eigene Ressourcengruppe erstellt. Wenn Sie im Menü auf der linken Seite **Ressourcengruppen** auswählen, finden Sie ihn anhand der Namenskonvention. Im Folgenden ist meine Ressourcengruppe abgebildet. Es ist die letzte in der Liste.

![Screenshot mit Ressourcengruppen](media/deploy-image-6.png)

**Ausführen des Images**

Jetzt ist es an der Zeit, das Image zu pullen und in AKS auszuführen. Dabei verwenden Sie am besten die Cloud Shell im Azure-Portal. Das entsprechende Symbol finden Sie oben rechts im Azure-Portal. Für diese exemplarische Vorgehensweise verwende ich die Bash-Shell.

![Screenshot: Symbol „Cloud Shell“](media/deploy-image-7.png)

Nachdem die Shell geladen wurde, geben Sie den folgenden Befehl ein:

**kubectl run es-acctdemo --image acrmf50.azurecr.io/es-acctdemo --port=9040**

Mit diesem Befehl wird das Image aus dem Repository **acrmf50.azurecr.io** gepullt und in AKS geladen. Anschließend wird es ausgeführt, wobei Port 9040 geöffnet ist. Vielleicht erinnern Sie sich noch: Das ist der Port, den Sie für das Docker-Image geöffnet haben. Sie benötigen ihn für den Zugriff auf Enterprise Server.

Kubernetes sollte mit einer Meldung antworten, dass die Bereitstellung erstellt wurde.

![Screenshot einer Bereitstellungsmeldung](media/deploy-image-8.jpg)

Wenn Sie überprüfen möchten, ob der Container tatsächlich ausgeführt wird, geben Sie Folgendes ein: **kubectl get pods**.

„es-acctdemo“ sollte wie in der folgenden Abbildung als Pod angezeigt werden, der gerade ausgeführt wird.

![Screenshot: „es-acctdemo“ als Pod, der gerade ausgeführt wird](media/deploy-image-9.png)

Glückwunsch! Nun wird Enterprise Server in Azure Kubernetes Service ausgeführt. Im nächsten Artikel zeige ich Ihnen, wie Sie die Verwaltungskonsole von Enterprise Server aufrufen und Ihre Bereitstellung mit Kubernetes aufskalieren.
