---
title: Generieren der ARM-Vorlage für Cloud Services (erweiterter Support) mithilfe des Azure-Portals
description: Generieren Sie die ARM-Vorlage und die Parameterdatei für Cloud Services (erweiterter Support) mithilfe des Azure-Portals.
ms.topic: how-to
ms.service: cloud-services-extended-support
author: surbhijain
ms.author: surbhijain
ms.reviewer: gachandw
ms.date: 03/07/2021
ms.custom: ''
ms.openlocfilehash: a4f206d68df3cd8dd4dd5b1b411d316e7aacde92
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106077102"
---
# <a name="generate-arm-template-for-cloud-services-extended-support-using-the-azure-portal"></a>Generieren der ARM-Vorlage für Cloud Services (erweiterter Support) mithilfe des Azure-Portals

Dieser Artikel erklärt, wie Sie die ARM-Vorlage und die Parameterdatei aus dem [Azure-Portal](https://portal.azure.com) für Ihren Cloud-Dienst herunterladen. Die ARM-Vorlage und die Parameterdatei können in Bereitstellungen über Powershell verwendet werden, um einen Cloud-Dienst zu erstellen oder zu aktualisieren

## <a name="get-arm-template-via-portal"></a>Abrufen der ARM-Vorlage über das Portal

  1. Wechseln Sie zum Azure-Portal, und [erstellen Sie einen neuen Cloud-Dienst](deploy-portal.md). Fügen Sie Ihre Clouddienst-Konfigurationsdatei, Ihre Paketdatei und Ihre Definitionsdatei hinzu. 
    :::image type="content" source="media/deploy-portal-4.png" alt-text="Abbildung: Uploadabschnitt der Registerkarte „Basiseinstellungen“ bei der Erstellung":::
  
  2. Sobald alle Felder ausgefüllt sind, wechseln Sie zur Registerkarte Überprüfen und Erstellen, um Ihre Bereitstellungskonfiguration zu validieren, und klicken Sie auf **Vorlage Herunterladen für die Automatisierung** Ihres Cloud-Dienstes (erweiterter Support).
    :::image type="content" source="media/download-template-portal-1.png" alt-text="Die Abbildung zeigt das Herunterladen der Vorlage unter Cloud-Service (erweiterter Support) auf dem Azure-Portal.":::
  
  3. Laden Sie die Vorlage und die Parameterdateien herunter. 
    :::image type="content" source="media/generate-template-portal-3.png" alt-text="Abbildung des Herunterladens der Vorlagendatei aus dem Azure-Portal":::
  
  4. Kopieren Sie das Paket-SAS-URI und die Konfigurations-SAS-URI aus der Registerkarte Überprüfen und Erstellen und fügen Sie die Pakete der Datei parameter.json hinzu. Diese Dateien können jetzt verwendet werden, um einen neuen Cloud-Dienst über PowerShell zu erstellen.
    :::image type="content" source="media/download-template-portal-2.png" alt-text="Die Abbildung zeigt den SAS-URI des Pakets und die SAS-URI-Parameter der Konfiguration im Azure-Portal an.":::
  
## <a name="next-steps"></a>Nächste Schritte 
- Sehen Sie sich die [häufig gestellten Fragen](faq.md) zu Cloud Services (erweiterter Support) an.
- Bereitstellen eines Clouddiensts (erweiterter Support) über das [Azure-Portal](deploy-portal.md)
  
