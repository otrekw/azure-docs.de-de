---
title: 'ITSM-Connector – Secure Export in Azure Monitor: Azure-Konfiguration'
description: In diesem Artikel erfahren Sie, wie Sie Azure konfigurieren, um Ihre ITSM-Produkte/-Dienste mit Secure Export in Azure Monitor zu verbinden und so ITSM-Arbeitselemente zentral zu überwachen und zu verwalten.
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 01/03/2021
ms.openlocfilehash: 8eb9430e3d280c52cf84c61f0a44cb12152ac054
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102037539"
---
# <a name="configure-azure-to-connect-itsm-tools-using-secure-export"></a>Konfigurieren von Azure für Verbindungen mit ITSM-Tools unter Verwendung von Secure Export

Dieser Artikel enthält Informationen zum Konfigurieren von Azure für die Verwendung von Secure Export.
Führen Sie die folgenden Schritte aus, um Secure Export zu verwenden:

1. [Registrieren Sie Ihre App in Azure AD.](./itsm-connector-secure-webhook-connections-azure-configuration.md#register-with-azure-active-directory)
1. [Definieren Sie einen Dienstprinzipal.](./itsm-connector-secure-webhook-connections-azure-configuration.md#define-service-principal)
1. [Erstellen Sie eine Aktionsgruppe „Sicherer Webhook“.](./itsm-connector-secure-webhook-connections-azure-configuration.md#create-a-secure-webhook-action-group)
1. Konfigurieren Sie Ihre Partnerumgebung.
    Von Secure Export werden Verbindungen mit den folgenden ITSM-Tools unterstützt:
    * [ServiceNow](./itsmc-secure-webhook-connections-servicenow.md)
    * [BMC Helix](./itsmc-secure-webhook-connections-bmc.md)

## <a name="register-with-azure-active-directory"></a>Registrieren bei Azure Active Directory

Führen Sie diese Schritte aus, um die Anwendung bei Azure AD zu registrieren:

1. Befolgen Sie die unter [Registrieren einer Anwendung bei Microsoft Identity Platform](../../active-directory/develop/quickstart-register-app.md) beschriebenen Schritte.
2. Wählen Sie in Azure AD die Option **Anwendung verfügbar machen** aus.
3. Wählen Sie für **Anwendungs-ID-URI** die Option **Festlegen** aus.

   [![Screenshot der Option zum Festlegen des Anwendungs-ID-URIs.](media/itsm-connector-secure-webhook-connections-azure-configuration/azure-ad.png)](media/itsm-connector-secure-webhook-connections-azure-configuration/azure-ad-expand.png#lightbox)
4. Wählen Sie **Speichern** aus.

## <a name="define-service-principal"></a>Definieren eines Dienstprinzipals

Der Aktionsgruppendienst ist eine Erstanbieteranwendung und verfügt daher über die Berechtigung zum Abrufen von Authentifizierungstoken von Ihrer AAD-Anwendung, um eine Authentifizierung mit ServiceNow durchführen zu können.
Als optionalen Schritt können Sie im Manifest der erstellten App eine Anwendungsrolle definieren, um den Zugriff so einzuschränken, dass nur bestimmte Anwendungen mit dieser Rolle Nachrichten senden können. Diese Rolle muss dann dem Dienstprinzipal der Aktionsgruppe zugewiesen werden (erfordert Mandantenadministratorrechte).

Dieser Schritt kann mithilfe derselben [PowerShell-Befehle](../alerts/action-groups.md#secure-webhook-powershell-script) ausgeführt werden.

## <a name="create-a-secure-webhook-action-group"></a>Erstellen einer Aktionsgruppe „Sicherer Webhook“

Nachdem Ihre Anwendung bei Azure AD registriert wurde, können Sie basierend auf Azure-Warnungen Arbeitselemente in Ihrem ITSM-Tool erstellen. Dazu verwenden Sie die Aktion „Sicherer Webhook“ in den Aktionsgruppen.

Aktionsgruppen bieten eine modulare und wiederverwendbare Methode zum Auslösen von Aktionen für Azure-Warnungen. Sie können Aktionsgruppen im Azure-Portal mit Metrikwarnungen, Aktivitätsprotokollwarnungen und Azure Log Analytics-Warnungen verwenden.
Weitere Informationen zu Aktionsgruppen finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](../alerts/action-groups.md).

Befolgen Sie diese Anweisungen für sichere Webhooks, um einen Webhook zu einer Aktion hinzuzufügen:

1. Suchen Sie im [Azure-Portal](https://portal.azure.com/) nach dem Eintrag **Überwachen**, und wählen Sie ihn aus. Im Bereich **Überwachen** sind alle Ihre Überwachungseinstellungen und -daten an einem zentralen Ort zusammengefasst.
2. Wählen Sie **Warnungen** > **Aktionen verwalten** aus.
3. Wählen Sie [Aktionsgruppe hinzufügen](../alerts/action-groups.md#create-an-action-group-by-using-the-azure-portal), und füllen Sie die Felder aus.
4. Geben Sie jeweils einen Namen in die Felder **Aktionsgruppenname** und **Kurzname** ein. Der Kurzname wird anstelle eines vollständigen Aktionsgruppennamens verwendet, wenn Benachrichtigungen mithilfe dieser Gruppe gesendet werden.
5. Wählen Sie **Sicherer Webhook** aus.
6. Legen Sie folgende Einstellungen fest:
   1. Wählen Sie die Objekt-ID der Azure Active Directory-Instanz aus, die Sie registriert haben.
   2. Fügen Sie für den URI die Webhook-URL ein, die Sie aus der [ITSM-Toolumgebung](#configure-the-itsm-tool-environment) kopiert haben.
   3. Legen Sie für **Allgemeines Warnungsschema aktivieren** die Option **Ja** fest. 

   Die folgende Abbildung zeigt die Konfiguration einer „Sicherer Webhook“-Beispielaktion:

   ![Screenshot einer Aktion „Sicherer Webhook“.](media/itsm-connector-secure-webhook-connections-azure-configuration/secure-webhook.png)

## <a name="configure-the-itsm-tool-environment"></a>Konfigurieren der ITSM-Toolumgebung

Die Konfiguration umfasst zwei Schritte:

1. Abrufen des URI für die Definition des sicheren Exports
2. Definitionen gemäß dem Flow des ITSM-Tools

## <a name="next-steps"></a>Nächste Schritte

* [ServiceNow-Konfiguration für Secure Export](./itsmc-secure-webhook-connections-servicenow.md)
* [BMC-Konfiguration für Secure Export](./itsmc-secure-webhook-connections-bmc.md)
