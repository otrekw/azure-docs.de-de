---
title: Häufig gestellte Fragen zum Azure Web PubSub-Dienst
description: Hier finden Sie Antworten auf häufig gestellte Fragen zu Azure Web PubSub.
author: yjin81
ms.author: yajin1
ms.service: azure-web-pubsub
ms.topic: overview
ms.date: 04/26/2021
ms.openlocfilehash: e53823539194fe1082621f458e8b5b3d493c191d
ms.sourcegitcommit: 62e800ec1306c45e2d8310c40da5873f7945c657
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108166884"
---
# <a name="azure-web-pubsub-service-faq"></a>Häufig gestellte Fragen zum Azure Web PubSub-Dienst

Hier finden Sie häufig gestellte Fragen zum Azure Web PubSub-Dienst. 

## <a name="is-azure-web-pubsub-service-ready-for-production-use"></a>Ist Azure Web PubSub bereit für den Einsatz in der Produktion?

Der Azure Web PubSub-Dienst befindet sich in der öffentlichen Vorschau und erfüllt nicht die verbindliche SLA. 

##  <a name="where-does-my-data-reside"></a>Wo befinden sich meine Daten?

Azure Web PubSub ist ein Datenverarbeitungsdienst. Inhalte von Kunden werden nicht gespeichert, und Data Residency wird durch den Entwurf garantiert. Wenn Sie Azure Web PubSub in Verbindung mit anderen Azure-Diensten (z. B. Azure Storage) zu Diagnosezwecken verwenden, finden Sie in [diesem Whitepaper](https://azure.microsoft.com/resources/achieving-compliant-data-residency-and-security-with-azure/) Informationen zum Aufrechterhalten von Data Residency in Azure-Regionen.