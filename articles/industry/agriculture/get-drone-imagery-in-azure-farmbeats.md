---
title: Abrufen von Drohnenbildern
description: In diesem Artikel erfahren Sie, wie Sie Drohnenbilder von Partnern abrufen.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-ummehabiba
ms.openlocfilehash: 18932d2ddb60242b4d7874dddf0349a62cd5c738
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102177605"
---
# <a name="get-drone-imagery-from-drone-partners"></a>Abrufen der Drohnenbilder von Drohnenpartnern

In diesem Artikel erfahren Sie, wie Sie Orthofotodaten von Ihren Drohnenbildpartnern in den Azure FarmBeats-Datenhub übernehmen können. Orthofotos sind Luftbilder oder Luftaufnahmen, die geometrisch korrigiert und aus den von einer Drohne gesammelten Daten zusammengefügt werden.

Aktuell werden folgende Bildpartner unterstützt.

  ![FarmBeats-Partner für Drohnenbilder](./media/get-drone-imagery-from-drone-partner/drone-partner-1.png)

Durch die Integration von Drohnenbilddaten in Azure FarmBeats können Sie Orthofotodaten von Drohnenflügen über Ihren landwirtschaftlichen Betrieb in den Datenhub übernehmen. Die verfügbaren Daten können dann im FarmBeats Accelerator angezeigt werden. Die Daten können für die Datenfusion und für die Entwicklung von Modellen genutzt werden, die auf künstlicher Intelligenz oder Machine Learning basieren.

## <a name="before-you-begin"></a>Voraussetzungen

  - Vergewissern Sie sich, dass Sie Azure FarmBeats installiert haben. Informationen zur Installation von FarmBeats finden Sie unter [Installieren von Azure FarmBeats](install-azure-farmbeats.md).
  - Vergewissern Sie sich, dass der landwirtschaftliche Betrieb, für den Sie Drohnenbilder verwenden möchten, in Ihrem FarmBeats-System definiert ist.

## <a name="enable-drone-imagery-integration-with-farmbeats"></a>Ermöglichen der Integration von Drohnenbildern in FarmBeats

Geben Sie die folgenden Informationen an Ihren Geräteanbieter weiter, um die Integration in FarmBeats zu ermöglichen:
 - API-Endpunkt
 - Mandanten-ID
 - Client-ID
 - Geheimer Clientschlüssel

Führen Sie die folgenden Schritte aus.

1. Laden Sie dieses [Skript](https://aka.ms/farmbeatspartnerscript) herunter, und extrahieren Sie es auf Ihr lokales Laufwerk. In der ZIP-Datei befinden sich zwei Dateien.
2. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und öffnen Sie Azure Cloud Shell. Diese Option ist auf der Symbolleiste in der rechten oberen Ecke des Portals verfügbar.

    ![Öffnen Sie Azure Cloud Shell über die Leiste in der oberen rechten Ecke des Portals.](./media/get-drone-imagery-from-drone-partner/navigation-bar-1.png)

3. Vergewissern Sie sich, dass die Umgebung auf **PowerShell** festgelegt ist.

    ![Einstellung „PowerShell“](./media/get-drone-imagery-from-drone-partner/power-shell-new-1.png)

4. Laden Sie die beiden Dateien, die Sie in Schritt 1 heruntergeladen haben, in Ihre Cloud Shell-Instanz hoch.

    ![Hochladen von Dateien](./media/get-drone-imagery-from-drone-partner/power-shell-two-1.png)

5. Navigieren Sie zum Uploadverzeichnis der Dateien. Die Dateien werden standardmäßig unter dem Benutzernamen in das Basisverzeichnis hochgeladen.
6. Führen Sie folgendes Skript aus:

    ```azurepowershell-interactive

    ./generateCredentials.ps1

    ```

7. Befolgen Sie die Anweisungen auf dem Bildschirm, um die Werte für API-Endpunkt, Mandanten-ID, Client-ID, geheimer Clientschlüssel und EventHub-Verbindungszeichenfolge zu erfassen.

    Nachdem Sie die erforderlichen Anmeldeinformationen in das Drohnensoftwaresystem des Partners eingegeben haben, können Sie alle landwirtschaftlichen Betriebe aus dem FarmBeats-System importieren. Anschließend können Sie die Details des landwirtschaftlichen Betriebs für Ihre Flugroutenplanung und die Sammlung von Drohnenbildern verwenden.

    Nachdem die Rohbilder durch die Software des Drohnenanbieters verarbeitet wurden, lädt das Drohnensoftwaresystem die zusammengesetzten Orthofotos sowie andere verarbeitete Bilder in den Datenhub hoch.

## <a name="view-drone-imagery"></a>Anzeigen von Drohnenbildern

Nachdem die Daten an den FarmBeats-Datenhub gesendet wurden, können Sie den Szenenspeicher über die FarmBeats-Datenhub-APIs abfragen.

Alternativ können Sie das neueste Drohnenbild auf der Seite **Details für den landwirtschaftlichen Betrieb** anzeigen. Gehen Sie zum Anzeigen des Bilds wie folgt vor.

1. Wählen Sie den landwirtschaftlichen Betrieb aus, in den Ihre Bilder hochgeladen wurden. Die Detailseite für den **landwirtschaftlichen** Betrieb wird angezeigt.
2. Scrollen Sie nach unten zum neuesten Abschnitt für **Präzisionskarten**.
3. Sie können sich das Bild im Abschnitt **Drone Imagery** (Drohnenbilder) ansehen.

    ![Abschnitt „Drone Imagery“ (Drohnenbilder)](./media/get-drone-imagery-from-drone-partner/drone-imagery-1.png)

## <a name="download-drone-imagery"></a>Herunterladen von Drohnenbildern

Wenn Sie den Abschnitt „Drone Imagery“ (Drohnenbilder) auswählen, wird ein Popupfenster geöffnet, um ein hochauflösendes Bild der Orthofotodaten der Drohne anzuzeigen.

![Orthofotos mit hoher Auflösung](./media/get-drone-imagery-from-drone-partner/download-drone-imagery-1.png)

## <a name="view-all-drone-maps"></a>Anzeigen aller Drohnenkarten

Vom Drohnenanbieter hochgeladene Dateien und Bilder werden im Abschnitt **Karten** angezeigt. Wählen Sie den Abschnitt **Karten** aus, filtern Sie nach **landwirtschaftlichem Betrieb**, und wählen Sie die entsprechenden Dateien aus, um sie anzuzeigen und herunterzuladen.

  ![Abschnitt „Karten“](./media/get-drone-imagery-from-drone-partner/view-drone-maps-1.png)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie Ihre Drohnenbilder unter Verwendung der [APIs](rest-api-in-azure-farmbeats.md) des FarmBeats-Datenhubs abrufen.
