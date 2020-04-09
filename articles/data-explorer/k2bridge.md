---
title: Visualisieren von Daten über Azure Data Explorer mit Kibana
description: In diesem Artikel erfahren Sie, wie Sie Azure Data Explorer als Datenquelle für Kibana einrichten.
author: orspod
ms.author: orspodek
ms.reviewer: guregini
ms.service: data-explorer
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: fac9c78607e50dca384670bf4cc08b50f723312b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80065612"
---
# <a name="visualize-data-from-azure-data-explorer-in-kibana-with-the-k2bridge-open-source-connector"></a>Visualisieren von Daten aus Azure Data Explorer in Kibana mit dem Open-Source-Connector K2Bridge

K2Bridge (Kibana-Kusto Bridge) ermöglicht es Ihnen, Azure Data Explorer als Datenquelle zu verwenden und diese Daten in Kibana zu visualisieren. K2Bridge ist eine [Open-Source](https://github.com/microsoft/K2Bridge)-Containeranwendung, die als Proxy zwischen einer Kibana-Instanz und einem Azure Data Explorer-Cluster fungiert. In diesem Artikel wird beschrieben, wie Sie diese Verbindung mit K2Bridge erstellen.

K2Bridge übersetzt Kibana-Abfragen in die Kusto-Abfragesprache (KQL) und sendet die Azure Data Explorer-Ergebnisse zurück an Kibana. 

   ![Diagramm](media/k2bridge/k2bridge-chart.png)

K2Bridge unterstützt die Kibana-Registerkarte „Entdecken“, auf der Sie folgende Möglichkeiten haben:
* Daten suchen und untersuchen
* Ergebnisse filtern
* Felder im Ergebnisraster hinzufügen oder entfernen
* Datensatzinhalt anzeigen
* Suchvorgänge speichern und freigeben

Die folgende Abbildung zeigt eine Kibana-Instanz, die über K2Bridge an Azure Data Explorer gebunden ist. Die Benutzeroberfläche in Kibana ist unverändert.

   [![Kibana-Seite](media/k2bridge/k2bridge-kibana-page.png)](media/k2bridge/k2bridge-kibana-page.png#lightbox)

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie Daten aus Azure Data Explorer in Kibana visualisieren können, halten Sie Folgendes bereit:

* [Helm V3](https://github.com/helm/helm#install), den Kubernetes-Paket-Manager.
* Azure Kubernetes Service-Cluster (AKS) oder einen beliebigen anderen Kubernetes-Cluster (Version 1.14 bis Version 1.16 wurden getestet und überprüft). Wenn Sie einen AKS-Cluster benötigen, finden Sie weitere Informationen unter „Bereitstellen eines AKS-Clusters [mithilfe der Azure CLI](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough) oder [über das Azure-Portal](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal)“.
* Ein [Azure Data Explorer-Cluster](create-cluster-database-portal.md), einschließlich:
    * URL des Azure Data Explorer-Clusters 
    * Datenbankname
    
* Azure AD-Dienstprinzipal, der berechtigt ist, Daten in Azure Data Explorer anzuzeigen, einschließlich:
    * Client-ID 
    * Geheimer Clientschlüssel

    Ein Dienstprinzipal mit „Viewer“-Berechtigung wird empfohlen. Es wird davon abgeraten, höhere Berechtigungen zu verwenden.

    * [Legen Sie die Anzeigeberechtigung des Clusters für den Azure AD-Dienstprinzipal fest](https://docs.microsoft.com/azure/data-explorer/manage-database-permissions#manage-permissions-in-the-azure-portal).

    Weitere Informationen über den Azure AD-Dienstprinzipal finden Sie unter [Erstellen eines Azure AD-Dienstprinzipals](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#create-an-azure-active-directory-application).

## <a name="run-k2bridge-on-azure-kubernetes-service-aks"></a>Ausführen von K2Bridge für Azure Kubernetes Service (AKS)

Standardmäßig verweist das Helm-Diagramm von K2Bridges auf ein öffentlich verfügbares Image, das sich in der Containerregistrierung von Microsoft (MCR) befindet. MCR erfordert keine Anmeldeinformationen und ist sofort einsatzbereit.

1. Laden Sie die erforderlichen Helm-Diagramme herunter.

1. Fügen Sie die Elasticsearch-Abhängigkeit zu Helm hinzu. 
    Der Grund für die Elasticsearch-Abhängigkeit liegt darin, dass K2Bridge eine interne kleine Elasticsearch-Instanz verwendet, um metadatenbezogene Anforderungen (wie Indexmuster und gespeicherte Abfragen) zu bedienen. In dieser internen Instanz werden keine Geschäftsdaten gespeichert, und sie kann als ein Implementierungsdetail betrachtet werden. 

    1. So fügen Sie die Elasticsearch-Abhängigkeit zu Helm hinzu

        ```bash
        helm repo add elastic https://helm.elastic.co
        helm repo update
        ```

    1. So rufen Sie das K2Bridge-Diagramm aus dem Diagrammverzeichnis des GitHub-Repositorys ab
        1. Klonen Sie das Repository von [GitHub](https://github.com/microsoft/K2Bridge).
        1. Wechseln Sie zum Stammverzeichnis des K2Bridges-Repositorys.
        1. Führen Sie folgenden Befehl aus:

            ```bash
            helm dependency update charts/k2bridge
            ```

1. Bereitstellen von K2Bridge:

    1. Richten Sie die Variablen mit den richtigen Werten für Ihre Umgebung ein:

        ```bash
        ADX_URL=[YOUR_ADX_CLUSTER_URL] #For example, https://mycluster.westeurope.kusto.windows.net
        ADX_DATABASE=[YOUR_ADX_DATABASE_NAME]
        ADX_CLIENT_ID=[SERVICE_PRINCIPAL_CLIENT_ID]
        ADX_CLIENT_SECRET=[SERVICE_PRINCIPAL_CLIENT_SECRET]
        ADX_TENANT_ID=[SERVICE_PRINCIPAL_TENANT_ID]
        ```

    1. (Optional) Aktivieren Sie Azure Application Insights-Telemetrie. 
        Wenn Sie Azure Application Insights zum ersten Mal verwenden, sollten Sie zunächst eine [Application Insights-Ressource erstellen](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource). Sie müssen den [Instrumentierungsschlüssel](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource#copy-the-instrumentation-key) in eine Variable kopieren: 

        ```bash
        APPLICATION_INSIGHTS_KEY=[INSTRUMENTATION_KEY]
        COLLECT_TELEMETRY=true
        ```

    1. <a name="install-k2bridge-chart"></a>Installieren Sie das K2Bridge-Diagramm:

        ```bash
        helm install k2bridge charts/k2bridge -n k2bridge --set image.repository=$REPOSITORY_NAME/$CONTAINER_NAME --set settings.adxClusterUrl="$ADX_URL" --set settings.adxDefaultDatabaseName="$ADX_DATABASE" --set settings.aadClientId="$ADX_CLIENT_ID" --set settings.aadClientSecret="$ADX_CLIENT_SECRET" --set settings.aadTenantId="$ADX_TENANT_ID" [--set image.tag=latest] [--set privateRegistry="$IMAGE_PULL_SECRET_NAME"] [--set settings.collectTelemetry=$COLLECT_TELEMETRY]
        ```

        Unter [Konfiguration](https://github.com/microsoft/K2Bridge/blob/master/docs/configuration.md) finden Sie den vollständigen Satz von Konfigurationsoptionen.

    1. Die Befehlsausgabe schlägt den nächsten Helm-Befehl vor, der zur Bereitstellung von Kibana ausgeführt werden soll. Führen Sie optional Folgendes aus:

        ```bash
        helm install kibana elastic/kibana -n k2bridge --set image=docker.elastic.co/kibana/kibana-oss --set imageTag=6.8.5 --set elasticsearchHosts=http://k2bridge:8080
        ```
        
    1. Verwenden Sie Portweiterleitung, um über Localhost auf Kibana zuzugreifen: 

        ```bash
        kubectl port-forward service/kibana-kibana 5601 --namespace k2bridge
        ```
        
    1. Stellen Sie eine Verbindung mit Kibana her, indem Sie zu http://127.0.0.1:5601 navigieren.

    1. Machen Sie Kibana für die Endbenutzer verfügbar. Dazu gibt es mehrere Möglichkeiten. Die von Ihnen verwendete Methode hängt weitgehend von Ihrem Anwendungsfall ab.

        Beispiel:

        Machen Sie den Dienst als LoadBalancer-Dienst verfügbar. Fügen Sie dazu den Parameter `--set service.type=LoadBalancer` zum Installationsbefehl von K2Bridge Helm hinzu ([oben](#install-k2bridge-chart)).        
    
        Führen Sie dann Folgendes aus:
        
        ```bash
        kubectl get service -w -n k2bridge
        ```
        
        Die Ausgabe sollte wie folgt aussehen: 

        ```bash
        NAME            TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
        kibana-kibana   LoadBalancer   xx.xx.xx.xx   <pending>      5601:30128/TCP   4m24s
        ```
 
        Sie können dann die angezeigte generierte EXTERNAL-IP verwenden, um auf Kibana zuzugreifen, indem Sie einen Browser mit `<EXTERNAL-IP>:5601` öffnen.

1. Konfigurieren Sie Indexmuster für den Zugriff auf Ihre Daten:  
In einer neuen Kibana-Instanz:
     1. Öffnen Sie Kibana.
     1. Navigieren Sie zu „Verwaltung“.
     1. Wählen Sie **Indexmuster** aus. 
     1. Erstellen Sie ein Indexmuster.
Der Name des Index muss genau mit dem Tabellennamen oder dem Funktionsnamen übereinstimmen, ohne Sternchen. Sie können die entsprechende Zeile aus der Liste kopieren.

> [!Note]
> Um unter anderen Kubernetes-Anbietern ausgeführt werden zu können, ändern Sie den „storageClassName“ von Elasticsearch in `values.yaml`, damit er zu dem vom Anbieter vorgeschlagenen passt.

## <a name="visualize-data"></a>Visualisieren von Daten

Wenn Azure Data Explorer als Datenquelle für Kibana konfiguriert ist, können Sie die Daten mit Kibana untersuchen. 

1. Wählen Sie in Kibana im linken Menü die Registerkarte **Entdecken** aus.

1. Wählen Sie aus der linken Dropdownliste ein Indexmuster (in diesem Fall eine Azure Data Explorer-Tabelle), das die zu untersuchende Datenquelle definiert.
    
   ![Auswählen eines Indexmusters](media/k2bridge/k2bridge-select-an-index-pattern.png)

1. Wenn Ihre Daten ein Zeitfilterfeld aufweisen, können Sie den Zeitbereich angeben. Legen Sie oben rechts auf der Seite einen Zeitfilter fest. Standardmäßig zeigt „Entdecken“ Daten für die letzten 15 Minuten an.

   ![Zeitfilter](media/k2bridge/k2bridge-time-filter.png)
    
1. Die Ergebnistabelle zeigt die ersten 500 Datensätze. Sie können ein Dokument erweitern, um seine Felddaten entweder in JSON- oder Tabellenformaten zu untersuchen.

   ![Erweitern eines Datensatzes](media/k2bridge/k2bridge-expand-record.png)

1. Standardmäßig enthält die Ergebnistabelle Spalten für die Dokumentquelle und das Zeitfeld (falls vorhanden). Sie können bestimmte Spalten auswählen, die der Ergebnistabelle hinzugefügt werden sollen, indem Sie **Hinzufügen** neben dem Feldnamen in der linken Seitenleiste auswählen.

   ![Bestimmte Spalten](media/k2bridge/k2bridge-specific-columns.png)
    
1. In der Abfrageleiste können Sie die Daten wie folgt durchsuchen:
    * Eingeben eines Suchbegriffs
    * Verwenden der Lucene-Abfragesyntax 
    Beispiel:
        * Suchen Sie nach „Fehler“, um alle Datensätze zu finden, die diesen Wert enthalten. 
        * Suchen Sie nach „Status: 200“, um alle Datensätze mit dem Statuswert 200 abzurufen. 
    * Verwenden logischer Operatoren (AND, OR, NOT)
    * Verwenden von Platzhalterzeichen (Sternchen „\*“ oder Fragezeichen „?“). Beispiel:
        * Die Abfrage `"destination_city: L*"` stimmt mit Datensätzen überein, bei denen der Wert des Zielorts mit „l“ beginnt (K2Bridge unterscheidet nicht zwischen Groß- und Kleinschreibung).

    ![Abfrage ausführen](media/k2bridge/k2bridge-run-query.png)
    
    > [!Tip]
    > In [Suchen](https://github.com/microsoft/K2Bridge/blob/master/docs/searching.md) finden Sie weitere Suchregeln und Logik.

1. Verwenden Sie die **Feldliste** in der rechten Seitenleiste der Seite, um Ihre Suchergebnisse zu filtern. 
    In der Feldliste können Sie Folgendes sehen:
    * Die fünf wichtigsten Werte für das Feld.
    * Die Anzahl der Datensätze, die das Feld enthalten.
    * Der Prozentsatz der Datensätze, die die einzelnen Werte enthalten. 
    
    >[!Tip]
    > Verwenden Sie das Symbol „(+)“, um alle Datensätze zu suchen, die einen bestimmten Wert aufweisen.
    
    ![Feldliste](media/k2bridge/k2bridge-field-list.png)
   
    Sie können die Ergebnisse auch filtern, indem Sie das (+)-Lupensymbol in der Formatansicht der Ergebnistabelle für die einzelnen Datensätze in der Ergebnistabelle verwenden.
    
     ![Tabellenliste](media/k2bridge/k2bridge-table-list.png)
    
1. Wählen Sie entweder **Speichern** oder **Freigeben** für Ihre Suche aus.

     ![Suche speichern](media/k2bridge/k2bridge-save-search.png)
