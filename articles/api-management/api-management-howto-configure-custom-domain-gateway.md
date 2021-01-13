---
title: Konfigurieren eines benutzerdefinierten Domänennamens für Ihr selbstgehostete Azure API Management-Gateway | Microsoft-Dokumentation
description: In diesem Thema werden die Schritte zum Konfigurieren eines benutzerdefinierten Domänennamens für ein selbstgehostetes Azure API Management-Gateway beschrieben.
services: api-management
documentationcenter: ''
author: vladvino
manager: gwallace
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 03/31/2020
ms.author: apimpm
ms.openlocfilehash: 0894203be4867e305c8e15467a2a867b9bfdc727
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86506807"
---
# <a name="configure-a-custom-domain-name"></a>Konfigurieren eines benutzerdefinierten Domänennamens

Wenn Sie ein [selbstgehostetes Azure API Management-Gateway](self-hosted-gateway-overview.md) bereitstellen, wird ihm kein Hostname zugewiesen, und es muss über seine IP-Adresse darauf verwiesen werden. In diesem Artikel erfahren Sie, wie Sie einem selbstgehosteten Gateway einen vorhandenen benutzerdefinierten DNS-Namen (auch als Hostname bezeichnet) zuordnen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen der in diesem Artikel beschriebenen Schritte benötigen Sie Folgendes:

-   Ein aktives Azure-Abonnement.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

-   Eine API Management-Instanz. Weitere Informationen finden Sie unter [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md).
- Ein selbstgehostetes Gateway. Weitere Informationen finden Sie unter [Bereitstellen eines selbstgehosteten Gateways in Azure API Management](api-management-howto-provision-self-hosted-gateway.md).
-   Einen benutzerdefinierten Domänennamen, der sich in Ihrem Besitz oder im Besitz Ihrer Organisation befindet. Dieses Thema enthält keine Anleitung zum Erwerben eines benutzerdefinierten Domänennamens.
-   Ein DNS-Eintrag, der auf einem DNS-Server gehostet wird, der den benutzerdefinierten Domänennamen der IP-Adresse des selbstgehosteten Gateways zuordnet. Dieses Thema enthält keine Anleitung zum Hosten eines DNS-Eintrags.
-   Sie benötigen ein gültiges Zertifikat mit einem öffentlichen und privaten Schlüssel (.pfx). Der Antragstellername oder der alternative Antragstellername (Subject Alternative Name, SAN) muss dem Domänennamen entsprechen, damit die API Management-Instanz auf sichere Weise URLs über TLS verfügbar machen kann.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="add-custom-domain-certificate-to-your-api-management-service"></a>Hinzufügen eines benutzerdefinierten Domänenzertifikats zum API Management-Dienst

1. Wählen Sie **Zertifikate** unter **Sicherheit** aus.
2. Wählen Sie **+ Hinzufügen**.
3. Geben Sie in das Feld **ID** einen Ressourcennamen für das Zertifikat ein.
4. Wählen Sie die Datei aus, die das Zertifikat (.PFX) enthält, indem Sie das Feld **Zertifikat** oder das Ordnersymbol daneben auswählen.
5. Geben Sie das Kennwort für das Zertifikat in das Feld **Kennwort** ein.
6. Wählen Sie **Erstellen** aus, um das Zertifikat dem API Management-Dienst hinzuzufügen.

## <a name="use-the-azure-portal-to-set-a-custom-domain-name-for-your-self-hosted-gateway"></a>Festlegen eines benutzerdefinierten Domänennamens für Ihr selbstgehostetes Gateway über das Azure-Portal

1. Wählen Sie die **Gateways** unter **Bereitstellung und Infrastruktur** aus.
2. Wählen Sie das selbstgehostete Gateway aus, für das Sie den Domänennamen konfigurieren möchten.
3. Wählen Sie **Hostnamen** unter **Einstellungen** aus.
4. Wählen Sie **+ Hinzufügen** aus.
5. Geben Sie den Ressourcennamen für den Hostnamen in das Feld **Name** ein.
6. Geben Sie in das Feld **Hostname** den Domänennamen ein.
7. Wählen Sie ein Zertifikat in der Dropdownliste **Zertifikat** aus.
8. Aktivieren Sie das Kontrollkästchen **Clientzertifikat aushandeln**, wenn eine der APIs, die über dieses Gateway verfügbar gemacht werden, die Clientzertifikatsauthentifizierung verwendet.
    > [!WARNING]
    > Diese Einstellung wird von allen Domänennamen gemeinsam genutzt, die für das Gateway konfiguriert sind.
9. Wählen Sie **Hinzufügen** aus, um den benutzerdefinierten Domänennamen dem ausgewählten selbstgehosteten Gateway zuzuweisen.

## <a name="next-steps"></a>Nächste Schritte

[Upgrade and scale an API Management instance](upgrade-and-scale.md) (Upgraden und Skalieren einer API Management-Instanz)
