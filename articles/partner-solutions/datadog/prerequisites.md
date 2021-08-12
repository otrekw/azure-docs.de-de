---
title: Voraussetzungen für Datadog in Azure – Azure-Partnerlösungen
description: In diesem Artikel wird beschrieben, wie Sie Ihre Azure-Umgebung konfigurieren, um eine Instanz von Datadog zu erstellen.
ms.service: partner-services
ms.topic: conceptual
ms.date: 05/28/2021
author: tfitzmac
ms.author: tomfitz
ms.openlocfilehash: 637d3cfb3baf8a53cb62943419077fad0f70d33d
ms.sourcegitcommit: 1b698fb8ceb46e75c2ef9ef8fece697852c0356c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110656871"
---
# <a name="configure-environment-before-datadog-deployment"></a>Konfigurieren der Umgebung vor der Datadog-Bereitstellung

In diesem Artikel wird beschrieben, wie Sie Ihre Umgebung einrichten, bevor Sie Ihre erste Instanz von Datadog bereitstellen. Diese Bedingungen sind Voraussetzungen für die Durchführung der Schnellstarts.

## <a name="access-control"></a>Zugriffssteuerung

Zum Einrichten der Azure-Datadog-Integration müssen Sie über die Zugriffsberechtigung **Besitzer** für das Azure-Abonnement verfügen. [Vergewissern Sie sich, dass Sie über den richtigen Zugriff verfügen](../../role-based-access-control/check-access.md), bevor Sie mit der Einrichtung beginnen.

## <a name="add-enterprise-application"></a>Hinzufügen einer Unternehmensanwendung
 
Wenn Sie die Funktion „Einmaliges Anmelden“ (SSO, Single-Sign-On) für Security Assertion Markup Language (SAML) in der Datadog-Ressource verwenden möchten, müssen Sie eine Unternehmensanwendung einrichten. Zum Hinzufügen einer Unternehmensanwendung benötigen Sie eine der folgenden Rollen: globaler Administrator, Cloudanwendungsadministrator, Anwendungsadministrator oder Besitzer des Dienstprinzipals.

Gehen Sie folgendermaßen vor, um die Unternehmensanwendung einzurichten:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com). Wählen Sie **Azure Active Directory** aus.
1. Wählen Sie im linken Bereich **Unternehmensanwendungen** aus.
1. Wählen Sie **Neue Anwendung**.
1. Suchen Sie unter **Aus Katalog hinzufügen** nach *datadog*. Wählen Sie das Suchergebnis und dann **Hinzufügen** aus.

   :::image type="content" source="media/prerequisites/datadog-azure-ad-app-gallery.png" alt-text="Datadog-Anwendung im AAD-Unternehmenskatalog." border="true":::

1. Nachdem die App erstellt wurde, wechseln Sie im Seitenbereich zu Eigenschaften. Legen Sie **Benutzerzuweisung erforderlich?** auf **Nein** fest, und wählen Sie **Speichern** aus.

   :::image type="content" source="media/prerequisites/user-assignment-required.png" alt-text="Festlegen von Eigenschaften für die Datadog-Anwendung" border="true":::

1. Wechseln Sie im Seitenbereich zu **einmaliges Anmelden**. Wählen Sie **SAML** aus.

   :::image type="content" source="media/prerequisites/saml-sso.png" alt-text="SAML-Authentifizierung." border="true":::

1. Wählen Sie **Ja**, wenn Sie aufgefordert werden, die **SSO-Einstellungen zu speichern**.

   :::image type="content" source="media/prerequisites/save-sso.png" alt-text="Einmaliges Anmelden für die Datadog-App speichern" border="true":::

1. Die Einrichtung des einmaligen Anmeldens ist nun abgeschlossen.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Erstellen einer Datadog-Instanz finden Sie unter [Schnellstart: Erste Schritte mit Datadog](create.md).
