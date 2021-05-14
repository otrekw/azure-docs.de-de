---
title: Verbinden von Azure Analysis Services mit Power BI | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von Power BI eine Verbindung mit einem Azure Analysis Services-Server herstellen. Nachdem die Verbindung hergestellt ist, können die Benutzer die Modelldaten untersuchen.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 4/20/2021
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: c5a430c5bb24032a2665ad078311dcb5137d2bb9
ms.sourcegitcommit: 260a2541e5e0e7327a445e1ee1be3ad20122b37e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2021
ms.locfileid: "107816028"
---
# <a name="connect-with-power-bi"></a>Herstellen einer Verbindung mit Power BI

Sobald Sie in Azure einen Server erstellt, und ein tabellarisches Modell bereitgestellt haben, können Benutzer in Ihrer Organisation beginnen, eine Verbindung herzustellen und Daten zu durchsuchen. 

> [!NOTE]
> Wenn Sie ein Power BI Desktop-Modell im Power BI-Dienst veröffentlichen, stellen Sie auf dem Azure Analysis Services-Server sicher, dass die Servereigenschaft „Beachtung der Groß-/Kleinschreibung bei Sortierung“ nicht ausgewählt ist (Standardeinstellung). Die Eigenschaft „Beachtung der Groß-/Kleinschreibung bei Sortierung“ kann mithilfe von SQL Server Management Studio festgelegt werden.
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a>Herstellen einer Verbindung in Power BI Desktop

1. Klicken Sie in Power BI Desktop auf **Daten abrufen** > **Azure** > **Azure Analysis Services-Datenbank**.

2. Geben Sie unter **Server** den Namen des Servers ein. Geben Sie unbedingt die vollständige URL an, beispielsweise „asazure://westcentralus.asazure.windows.net/advworks“.

3. Wenn Sie den Namen der Datenbank für tabellarische Modelle oder die Perspektive kennen, mit der Sie sich verbinden wollen, fügen Sie ihn unter **Datenbanken** ein. Andernfalls können Sie dieses Feld leer lassen und später eine Datenbank oder Perspektive auswählen.

4. Wählen Sie eine Verbindungsoption aus, und klicken Sie dann auf **Verbinden**. 

    Sowohl **Live verbinden** als auch **Importieren** wird unterstützt. Es wird jedoch empfohlen, Liveverbindungen zu verwenden, da beim Importmodus einige Einschränkungen gelten. Insbesondere kann während des Imports die Serverleistung beeinträchtigt sein. Falls das Modell im Power BI-Dienst aktualisiert werden soll, wird darüber hinaus die Einstellung **Zugriff über Power BI zulassen** nur angewendet, wenn **Live verbinden** ausgewählt wird.

5. Geben Sie bei der entsprechenden Aufforderung Ihre Anmeldeinformationen ein. 

   > [!NOTE]
   > Konten mit Einmalkennung (One-Time Passcode, OTP) werden nicht unterstützt. 

6. Erweitern Sie in **Navigator** den Server, wählen Sie das Modell oder die Perspektive aus, mit dem/der Sie eine Verbindung herstellen möchten, und klicken Sie dann auf **Verbinden**. Klicken Sie auf ein Modell oder eine Perspektive, um alle Objekte für diese Ansicht anzuzeigen.

    Das Modell wird in Power BI Desktop mit einem leeren Bericht in der Berichtsansicht geöffnet. In der Felderliste werden alle nicht ausgeblendeten Modellobjekte angezeigt. Der Verbindungsstatus wird in der unteren rechten Ecke angezeigt.

## <a name="connect-in-power-bi-service"></a>Herstellen einer Verbindung in Power BI (Dienst)

1. Erstellen Sie eine Power BI Desktop-Datei, die über eine Liveverbindung zu Ihrem Modell auf dem Server verfügt.
2. Klicken Sie in [Power BI](https://powerbi.microsoft.com) auf **Daten abrufen** > **Dateien**, suchen Sie Ihre PBIX-Datei, und wählen Sie sie dann aus.

## <a name="see-also"></a>Weitere Informationen
[Herstellen einer Verbindung mit Azure Analysis Services](analysis-services-connect.md)   
[Clientbibliotheken](/analysis-services/client-libraries?view=azure-analysis-services-current&preserve-view=true)
