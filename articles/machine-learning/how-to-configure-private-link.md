---
title: Konfigurieren eines privaten Endpunkts (Vorschau)
titleSuffix: Azure Machine Learning
description: Verwenden Sie Azure Private Link, um sicher von einem virtuellen Netzwerk aus auf Ihren Azure Machine Learning-Arbeitsbereich zuzugreifen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 09/03/2020
ms.openlocfilehash: 83927c9df9a4f1a6ab32c15c481898ec68f53c4c
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90898154"
---
# <a name="configure-azure-private-link-for-an-azure-machine-learning-workspace-preview"></a>Konfigurieren von Azure Private Link für einen Azure Machine Learning-Arbeitsbereich (Vorschauversion)

In diesem Dokument erfahren Sie, wie Sie Azure Private Link mit Ihrem Azure Machine Learning-Arbeitsbereich verwenden. Informationen zum Einrichten eines virtuellen Netzwerks für Azure Machine Learning finden Sie unter [Übersicht über die Isolation virtueller Netzwerke und Datenschutz](how-to-network-security-overview.md).

> [!IMPORTANT]
> Die Verwendung von Azure Private Link mit dem Azure Machine Learning-Arbeitsbereich befindet sich derzeit in der öffentlichen Vorschau. Diese Funktionalität ist nur in den Regionen **USA, Osten**, **USA, Süden-Mitte** und **USA, Westen 2** verfügbar. Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Private Link ermöglicht Ihnen das Herstellen einer Verbindung mit Ihrem Arbeitsbereich über einen privaten Endpunkt. Bei einem privaten Endpunkt handelt es sich um eine Gruppe privater IP-Adressen in Ihrem virtuellen Netzwerk. Sie können dann den Zugriff auf Ihren Arbeitsbereich so einschränken, dass er nur über die privaten IP-Adressen erfolgt. Private Link hilft dabei, das Risiko einer Datenexfiltration zu verringern. Weitere Informationen zu privaten Endpunkten finden Sie im Artikel zu [Azure Private Link](/azure/private-link/private-link-overview).

> [!IMPORTANT]
> Azure Private Link hat keine Auswirkungen auf die Azure-Steuerungsebene (Verwaltungsvorgänge) aus, etwa das Löschen des Arbeitsbereichs oder das Verwalten von Computeressourcen. Beispiele: Erstellen, Aktualisieren oder Löschen eines Computeziels. Diese Vorgänge werden ganz normal über das öffentliche Internet ausgeführt. Bei Vorgängen auf Datenebene, etwa bei der Verwendung von Azure Machine Learning Studio, APIs (einschließlich veröffentlichter Pipelines) oder des SDK, wird der private Endpunkt verwendet.
>
> Wenn Sie Mozilla Firefox verwenden, treten möglicherweise Probleme beim Zugriff auf den privaten Endpunkt für Ihren Arbeitsbereich auf. Dieses Problem kann im Zusammenhang mit DNS über HTTPS in Mozilla auftreten. Die Verwendung von Microsoft Edge von Google Chrome wird als Problemumgehung empfohlen.

> [!TIP]
> Die Azure Machine Learning-Computeinstanz kann mit einem Arbeitsbereich und einem privaten Endpunkt verwendet werden. Diese Funktion befindet sich derzeit in den Regionen **USA, Osten**, **USA, Süden-Mitte** und **USA, Westen 2** in der öffentlichen Vorschau.

## <a name="create-a-workspace-that-uses-a-private-endpoint"></a>Erstellen eines Arbeitsbereichs, der einen privaten Endpunkt verwendet

> [!IMPORTANT]
> Derzeit wird das Aktivieren eines privaten Endpunkts nur beim Erstellen eines neuen Azure Machine Learning-Arbeitsbereichs unterstützt.

Die Vorlage unter [https://github.com/Azure/azure-quickstart-templates/tree/master/201-machine-learning-advanced](https://github.com/Azure/azure-quickstart-templates/tree/master/201-machine-learning-advanced) kann zur Erstellung eines Arbeitsbereichs mit einem privaten Endpunkt verwendet werden.

Informationen zur Verwendung dieser Vorlage, einschließlich privater Endpunkte, finden Sie unter [Verwenden einer Azure Resource Manager-Vorlage zum Erstellen eines Arbeitsbereichs für Azure Machine Learning](how-to-create-workspace-template.md).

## <a name="using-a-workspace-over-a-private-endpoint"></a>Verwenden eines Arbeitsbereichs über einen privaten Endpunkt

Da die Kommunikation mit dem Arbeitsbereich nur aus dem virtuellen Netzwerk zulässig ist, müssen alle Entwicklungsumgebungen, in denen der Arbeitsbereich verwendet wird, Mitglieder des virtuellen Netzwerks sein. Beispiel: ein virtueller Computer im virtuellen Netzwerk.

> [!IMPORTANT]
> Um eine vorübergehende Unterbrechung der Konnektivität zu vermeiden, empfiehlt Microsoft, den DNS-Cache auf Computern, die eine Verbindung mit dem Arbeitsbereich herstellen, nach dem Aktivieren von Private Link zu leeren. 

Weitere Informationen zu Azure Virtual Machines finden Sie in der [Dokumentation zu Virtual Machines](/azure/virtual-machines/).


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Schützen Ihres Azure Machine Learning-Arbeitsbereichs finden Sie im Artikel [Übersicht über die Isolation virtueller Netzwerke und Datenschutz](how-to-network-security-overview.md).
