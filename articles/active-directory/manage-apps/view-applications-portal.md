---
title: 'Schnellstart: Anzeigen der Liste mit den Anwendungen in Ihrem Mandanten'
titleSuffix: Azure AD
description: In diesem Schnellstart verwenden Sie das Azure-Portal zum Anzeigen der Liste mit den Anwendungen, die für die Nutzung Ihres Azure AD-Mandanten (Azure Active Directory) für die Identitätsverwaltung registriert sind.
services: active-directory
author: davidmu1
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: quickstart
ms.date: 07/22/2021
ms.author: davidmu
ms.reviewer: alamaral
ms.custom: it-pro
ms.openlocfilehash: b5b9c84a5cbadd7d3661a2109223c43dbff7da7a
ms.sourcegitcommit: 3941df51ce4fca760797fa4e09216fcfb5d2d8f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2021
ms.locfileid: "114605692"
---
# <a name="quickstart-view-the-list-of-applications-in-your-tenant"></a>Schnellstart: Anzeigen der Liste mit den Anwendungen in Ihrem Mandanten

Führen Sie die ersten Schritte aus, um Azure AD als IAM-System (Identity and Access Management) für die Anwendungen zu nutzen, die von Ihrer Organisation verwendet werden. In dieser Schnellstartanleitung zeigen Sie die Anwendungen (auch als Apps bezeichnet) an, für die die Nutzung des Azure AD-Mandanten als Identitätsanbieter (IdP) bereits eingerichtet ist.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um die für Ihren Azure AD-Mandanten registrierten Anwendungen anzuzeigen:

- Ein Azure-Konto. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

>[!IMPORTANT]
>Wir empfehlen Ihnen, zum Testen der Schritte in dieser Schnellstartanleitung keine Produktionsumgebung zu verwenden.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, benötigen Sie mindestens Version 2.0.4 der Azure-Befehlszeilenschnittstelle. Führen Sie `az --version` aus, um die Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli).

## <a name="find-the-list-of-applications-in-your-tenant"></a>Suchen nach der Liste mit den Anwendungen in Ihrem Mandanten

Gehen Sie wie folgt vor, um die unter dem Mandanten registrierten Anwendungen anzuzeigen:

# <a name="portal"></a>[Portal](#tab/azure-portal)

Die Anwendungen, die unter Ihrem Azure AD-Mandanten registriert sind, können im Azure-Portal im Abschnitt **Unternehmens-Apps** angezeigt werden.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Bereich **Azure-Dienste** die Option **Unternehmensanwendungen** aus.
3. Wählen Sie im Dropdownmenü **Anwendungstyp** die Option **Alle Anwendungen** und dann **Anwenden** aus. Eine nach dem Zufallsprinzip ausgewählte Gruppe Ihrer Mandantenanwendungen wird angezeigt.
4. Wenn Sie weitere Anwendungen anzeigen möchten, wählen Sie unten in der Liste **Mehr anzeigen** aus. Falls Ihr Mandant eine große Zahl von Anwendungen enthält, kann es einfacher sein, direkt nach einer bestimmten Anwendung zu suchen, als durch die Liste zu scrollen. Die Vorgehensweise beim Suchen nach einer bestimmten Anwendung wird später in dieser Schnellstartanleitung beschrieben.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Melden Sie sich an, und verwenden Sie Anwendungen mit den [az ad app](/cli/azure/ad/app)-Befehlen.

```azurecli
az login

az ad app list --all
```

---

## <a name="select-viewing-options"></a>Auswählen der Anzeigeoptionen

Wählen Sie entsprechende Optionen für die gesuchten Elemente aus.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Sie können die Anwendungen nach **Anwendungstyp**, **Anwendungsstatus** und **Anwendungssichtbarkeit** anzeigen.
2. Wählen Sie unter **Anwendungstyp** eine der folgenden Optionen aus:
    - Mit der Option **Unternehmensanwendungen** werden nicht von Microsoft stammende Anwendungen angezeigt.
    - Mit der Option **Microsoft-Anwendungen** werden Microsoft-Anwendungen angezeigt.
    - Mit der Option **Alle Anwendungen** werden nicht von Microsoft stammende Anwendungen und Microsoft-Anwendungen angezeigt.
3. Wählen Sie unter **Anwendungsstatus** die Option **Any** (Alle), **Deaktiviert** oder **Aktiviert** aus. Mit der Option **Any** (Alle) werden sowohl deaktivierte als auch aktivierte Anwendungen angezeigt.
4. Wählen Sie unter **Anwendungssichtbarkeit** die Option **Any** (Alle) oder **Ausgeblendet**. Mit der Option **Ausgeblendet** werden Anwendungen angezeigt, die sich zwar im Mandanten befinden, aber nicht für Benutzer sichtbar sind.
5. Klicken Sie nach dem Auswählen der gewünschten Optionen auf **Anwenden**.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az ad app list --filter "displayname eq 'test' and servicePrincipalType eq 'Application'"
```

---

## <a name="search-for-an-application"></a>Suchen einer Anwendung

So suchen Sie nach einer bestimmten Anwendung:

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Wählen Sie im Menü **Anwendungstyp** die Option **Alle Anwendungen** und dann **Anwenden** aus.
2. Geben Sie den Namen der gewünschten Anwendung ein. Wenn die Anwendung zu Ihrem Azure AD-Mandanten hinzugefügt wurde, wird sie in den Suchergebnissen angezeigt. Dieses Beispiel zeigt, dass GitHub nicht zu den Mandantenanwendungen hinzugefügt wurde.
    ![Beispiel: Dem Mandanten wurde keine App hinzugefügt](media/view-applications-portal/search-for-tenant-application.png)
3. Geben Sie die ersten Buchstaben eines Anwendungsnamens ein. Dieses Beispiel zeigt alle Anwendungen, die mit **Office** beginnen.
    ![Beispiel: Anzeige aller Apps, die mit „Sales“ beginnen](media/view-applications-portal/search-by-prefix.png)

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az ad app show --id 710abb12-abeb-40ba-91ab-4b1f44f9ceb8 --query 'objectId' -o json
```

---

> [!TIP]
> Sie können die App-Verwaltung mithilfe der Graph-API automatisieren. Informationen hierzu finden Sie unter [Automatisieren einer SAML-basierten SSO-App-Konfiguration mit Microsoft Graph-API](/graph/application-saml-sso-configure-api).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In dieser Schnellstartanleitung haben Sie keine neuen Ressourcen erstellt, sodass keine Bereinigung ausgeführt werden muss.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel erfahren Sie, wie Sie Azure AD als Identitätsanbieter für eine App verwenden:
> [!div class="nextstepaction"]
> [Hinzufügen einer App](add-application-portal.md)
