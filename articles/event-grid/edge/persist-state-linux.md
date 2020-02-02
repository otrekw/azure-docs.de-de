---
title: Beibehalten des Zustands in Linux – Azure Event Grid IoT Edge | Microsoft-Dokumentation
description: Beibehalten von Metadaten in Linux
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/06/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 39b16c6cfd5b94d412827ed88197edbef2da1453
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76844631"
---
# <a name="persist-state-in-linux"></a>Beibehalten des Zustands in Linux

Themen und Abonnements, die im Event Grid-Modul erstellt wurden, werden standardmäßig im Containerdateisystem gespeichert. Ohne Persistenz gehen beim erneuten Bereitstellen des Moduls alle erstellten Metadaten verloren. Um die Daten für mehrere Bereitstellungen und über Neustarts hinweg beizubehalten, müssen Sie die Daten außerhalb des Containerdateisystems persistent speichern.

Standardmäßig werden nur Metadaten persistent gespeichert, und Ereignisse werden weiterhin im Arbeitsspeicher gespeichert, um die Leistung zu steigern. Befolgen Sie den Abschnitt zum persistenten Speichern von Ereignissen, um auch Ereignispersistenz zu aktivieren.

In diesem Artikel werden die Schritte beschrieben, die zum Bereitstellen des Event Grid-Moduls mit Persistenz in Linux-Bereitstellungen erforderlich sind.

> [!NOTE]
>Das Event Grid-Modul wird als Benutzer mit geringfügigen Rechten mit der UID `2000` und den Namen `eventgriduser` ausgeführt.

## <a name="persistence-via-volume-mount"></a>Persistenz über Volumeeinbindung

 [Docker-Volumes](https://docs.docker.com/storage/volumes/) werden verwendet, um die Daten über Bereitstellungen hinweg beizubehalten. Sie können veranlassen, dass Docker automatisch ein benanntes Volume als Teil der Bereitstellung des Event Grid Moduls erstellt. Diese Option ist die einfachste. Sie können den Namen des zu erstellenden Volumes im Abschnitt **Bindungen** wie folgt angeben:

```json
  {
     "HostConfig": {
          "Binds": [
                 "<your-volume-name-here>:/app/metadataDb"
           ]
      }
   }
```

>[!IMPORTANT]
>Ändern Sie nicht den zweiten Teil des Bindungswerts. Er verweist auf eine bestimmte Adresse innerhalb des Moduls. Für das Event Grid-Modul unter Linux muss dies **/app/metadataDb** sein.

Die folgende Konfiguration führt z. B. zur Erstellung des Volumes **egmetadataDbVol**, wo die Metadaten persistent gespeichert werden.

```json
 {
  "Env": [
    "inbound:serverAuth:tlsPolicy=strict",
    "inbound:serverAuth:serverCert:source=IoTEdge",
    "inbound:clientAuth:sasKeys:enabled=false",
    "inbound:clientAuth:clientCert:enabled=true",
    "inbound:clientAuth:clientCert:source=IoTEdge",
    "inbound:clientAuth:clientCert:allowUnknownCA=true",
    "outbound:clientAuth:clientCert:enabled=true",
    "outbound:clientAuth:clientCert:source=IoTEdge",
    "outbound:webhook:httpsOnly=true",
    "outbound:webhook:skipServerCertValidation=false",
    "outbound:webhook:allowUnknownCA=true"
  ],
  "HostConfig": {
    "Binds": [
      "egmetadataDbVol:/app/metadataDb",
      "egdataDbVol:/app/eventsDb"
    ],
    "PortBindings": {
      "4438/tcp": [
        {
          "HostPort": "4438"
        }
      ]
    }
  }
}
```

Anstatt ein Volume einzubinden, können Sie auch ein Verzeichnis auf dem Hostsystem erstellen und dieses Verzeichnis einbinden.

## <a name="persistence-via-host-directory-mount"></a>Persistenz über Einbindung des Hostverzeichnisses

Anstelle eines Docker-Volumes haben Sie auch die Möglichkeit, einen Hostordner einzubinden.

1. Erstellen Sie zunächst einen Benutzer mit dem Namen **eventgriduser** und der ID **2000** auf dem Hostcomputer, indem Sie den folgenden Befehl ausführen:

    ```sh
    sudo useradd -u 2000 eventgriduser
    ```
1. Erstellen Sie ein Verzeichnis im Hostdateisystem, indem Sie den folgenden Befehl ausführen.

   ```sh
   md <your-directory-name-here>
   ```

    Beispielsweise erstellt die Ausführung des folgenden Befehls ein Verzeichnis namens **myhostdir**.

    ```sh
    md /myhostdir
    ```
1. Machen Sie als Nächstes **eventgriduser** zum Besitzer dieses Ordners, indem Sie den folgenden Befehl ausführen.

   ```sh
   sudo chown eventgriduser:eventgriduser -hR <your-directory-name-here>
   ```

    Beispiel:

    ```sh
    sudo chown eventgriduser:eventgriduser -hR /myhostdir
    ```
1. Verwenden Sie **Bindungen**, um das Verzeichnis einzubinden und das Event Grid-Modul über das Azure-Portal erneut bereitzustellen.

    ```json
    {
         "HostConfig": {
            "Binds": [
                "<your-directory-name-here>:/app/metadataDb"
             ]
         }
    }
    ```

    Beispiel:

    ```json
    {
          "Env": [
            "inbound:serverAuth:tlsPolicy=strict",
            "inbound:serverAuth:serverCert:source=IoTEdge",
            "inbound:clientAuth:sasKeys:enabled=false",
            "inbound:clientAuth:clientCert:enabled=true",
            "inbound:clientAuth:clientCert:source=IoTEdge",
            "inbound:clientAuth:clientCert:allowUnknownCA=true",
            "outbound:clientAuth:clientCert:enabled=true",
            "outbound:clientAuth:clientCert:source=IoTEdge",
            "outbound:webhook:httpsOnly=true",
            "outbound:webhook:skipServerCertValidation=false",
            "outbound:webhook:allowUnknownCA=true"
          ],
          "HostConfig": {
                "Binds": [
                  "/myhostdir:/app/metadataDb",
                  "/myhostdir2:/app/eventsDb"
                ],
                "PortBindings": {
                      "4438/tcp": [
                        {
                          "HostPort": "4438"
                        }
                      ]
                }
          }
    }
    ```

    >[!IMPORTANT]
    >Ändern Sie nicht den zweiten Teil des Bindungswerts. Er verweist auf eine bestimmte Adresse innerhalb des Moduls. Für das Event Grid-Modul unter Linux muss dies **/app/metadata** sein.


## <a name="persist-events"></a>Persistentes Speichern von Ereignissen

Um Ereignispersistenz zu aktivieren, müssen Sie zunächst Metadatenpersistenz über Volumeeinbindung oder Hostverzeichniseinbindung aktivieren.

Wichtige Punkte beim persistenten Speichern von Ereignissen:

* Das persistente Speichern von Ereignissen wird pro Ereignisabonnement aktiviert und kann wahlweise verwendet werden, sobald ein Volume oder Verzeichnis eingebunden wurde.
* Ereignispersistenz wird zum Erstellungszeitpunkt für ein Ereignisabonnement konfiguriert und kann nicht geändert werden, nachdem das Ereignisabonnement erstellt wurde. Zum Umschalten von Ereignispersistenz müssen Sie das Ereignisabonnement löschen und dann neu erstellen.
* Das persistente Speichern von Ereignissen ist fast immer langsamer als Vorgänge im Arbeitsspeicher, aber der Geschwindigkeitsunterschied hängt stark von den Merkmalen des Laufwerks ab. Der Kompromiss zwischen Geschwindigkeit und Zuverlässigkeit ist allen Messagingsystemen inhärent, wird jedoch im Allgemeinen nur bei großer Skalierung bemerkt.

Legen Sie `persistencePolicy` auf `true` fest, um Ereignispersistenz für ein Ereignisabonnement zu aktivieren:

 ```json
        {
          "properties": {
            "persistencePolicy": {
              "isPersisted": "true"
            },
            "destination": {
              "endpointType": "WebHook",
              "properties": {
                "endpointUrl": "<your-webhook-url>"
              }
            }
          }
        }
 ```