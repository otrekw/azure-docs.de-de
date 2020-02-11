---
title: Behandeln von Problemen mit Azure Data Share
description: Hier erfahren Sie, wie Sie Probleme mit Einladungen und Fehlern beim Erstellen oder Empfangen von Datenfreigaben mit Azure Data Share behandeln.
services: data-share
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: troubleshooting
ms.date: 07/10/2019
ms.openlocfilehash: 901f2b56bc045dc9a9837dd18b2e6ce7169aa3b9
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2020
ms.locfileid: "76964225"
---
# <a name="troubleshoot-common-issues-in-azure-data-share"></a>Behandeln allgemeiner Probleme in Azure Data Share 

In diesem Artikel erfahren Sie, wie Sie allgemeine Probleme mit Azure Data Share behandeln. 

## <a name="azure-data-share-invitations"></a>Azure Data Share-Einladungen 

Wenn ein neuer Benutzer in der gesendeten E-Mail-Einladung auf **Einladung akzeptieren** klickt, kann es vorkommen, dass eine leere Einladungsliste angezeigt wird. 

![Keine Einladungen](media/no-invites.png)

Dies könnte die folgenden Gründe haben:

* **Der Azure Data Share-Dienst ist im Azure-Mandanten nicht als Ressourcenanbieter eines Azure-Abonnements registriert.** Dieses Problem tritt auf, wenn es in Ihrem Azure-Mandanten keine Data Share-Ressource gibt. Wenn Sie eine Azure Data Share-Ressource erstellen, wird der Ressourcenanbieter in Ihrem Azure-Abonnement automatisch registriert. Sie können den Data Share-Dienst auch manuell registrieren, indem Sie die nachstehenden Schritte ausführen. Für diese Schritte benötigen Sie die Rolle „Azure-Mitwirkender“.

    1. Navigieren Sie im Azure-Portal zu **Abonnements**.
    1. Wählen Sie das Abonnement aus, das Sie zum Erstellen einer Azure Data Share-Ressource verwenden möchten.
    1. Klicken Sie auf **Ressourcenanbieter**.
    1. Suchen Sie nach **Microsoft.DataShare**.
    1. Klicken Sie auf **Registrieren**. 

    Für diese Schritte wird die [Azure-RBAC-Rolle „Mitwirkender“](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor) benötigt. 

* **Die Einladung wird an Ihren E-Mail-Alias statt an Ihre E-Mail-Adresse für die Anmeldung bei Azure gesendet.** Wenn Sie den Azure Data Share-Dienst registriert oder bereits eine Data Share-Ressource im Azure-Mandanten erstellt haben, die Einladung aber immer noch nicht angezeigt wird, liegt dies möglicherweise daran, dass der Anbieter Ihren E-Mail-Alias statt Ihrer E-Mail-Adresse für die Anmeldung bei Azure als Empfänger eingegeben hat. Wenden Sie sich an Ihren Datenanbieter, und stellen Sie sicher, dass die Einladung an Ihre E-Mail-Adresse für die Anmeldung bei Azure und nicht an Ihren E-Mail-Alias gesendet wurde.

* **Die Einladung wurde bereits akzeptiert.** Der Link in der E-Mail führt Sie im Azure-Portal zur Data Share-Einladungsseite, auf der nur ausstehende Einladungen aufgeführt sind. Wenn Sie die Einladung bereits akzeptiert haben, wird sie auf der Data Share-Einladungsseite nicht mehr angezeigt. Fahren Sie mit Ihrer Data Share-Ressource fort, mit der Sie die Einladung zum Anzeigen von empfangenen Freigaben und Konfigurieren Ihrer Zieleinstellung für den Azure Data Explorer-Cluster akzeptiert haben.

## <a name="error-when-creating-or-receiving-a-new-share"></a>Fehler beim Erstellen oder Empfangen einer neuen Freigabe

„Fehler beim Hinzufügen von Datasets“

„Fehler beim Zuordnen von Datasets“

„Data Share-Ressource ‚x‘ kann kein Zugriff auf ‚y‘ gewährt werden.“

„Sie besitzen nicht die erforderlichen Berechtigungen für ‚x‘.“

„Es konnten keine Schreibberechtigungen für das Azure Data Share-Konto mindestens einer Ihrer ausgewählten Ressourcen hinzugefügt werden.“

Wenn beim Erstellen einer neuen Freigabe oder beim Zuordnen von Datasets einer der vorstehenden Fehler angezeigt wird, könnte dies daran liegen, dass die Berechtigungen für den Azure-Datenspeicher nicht ausreichen. Informationen zu den erforderlichen Berechtigungen finden Sie unter [Rollen und Anforderungen](concepts-roles-permissions.md). 

Sie benötigen eine Schreibberechtigung zum Freigeben oder Empfangen von Daten aus einem Azure-Datenspeicher, die in der Rolle „Mitwirkender“ normalerweise enthalten ist. 

Wenn Sie zum ersten Mal Daten aus dem Azure-Datenspeicher freigeben oder empfangen, benötigen Sie außerdem die Berechtigung *Microsoft.Authorization/role assignments/write*, die normalerweise in der Rolle „Besitzer“ enthalten ist. Selbst wenn Sie die Azure Data Store-Ressource erstellt haben, werden Sie dadurch NICHT automatisch zu deren Besitzer. Bei der entsprechenden Berechtigung gewährt der Azure Data Share-Dienst der verwalteten Identität der Data Share-Ressource automatisch Zugriff auf den Datenspeicher. Dieser Vorgang könnte einige Minuten dauern. Wenn aufgrund dieser Verzögerung ein Fehler auftritt, versuchen Sie es in einigen Minuten erneut.

Für SQL-basierte Freigabe sind zusätzliche Berechtigungen erforderlich. Ausführliche Informationen finden Sie unter „Behandeln allgemeiner Probleme bei der SQL-basierten Freigabe“.

## <a name="troubleshooting-sql-based-sharing"></a>Behandeln allgemeiner Probleme bei der SQL-basierten Freigabe

„Der Benutzer ‚x‘ ist in der SQL-Datenbank nicht vorhanden.“

Wenn Ihnen diese Fehlermeldung beim Hinzufügen eines Datasets aus einer SQL-basierten Quelle angezeigt wird, liegt dies möglicherweise daran, dass Sie keinen Benutzer für die verwaltete Identität von Azure Data Share auf Ihrer SQL Server-Instanz erstellt haben.  Führen Sie zum Beheben dieses Problems das folgende Skript aus:

```sql
    create user "<share_acct_name>" from external provider; 
    exec sp_addrolemember db_datareader, "<share_acct_name>";
```      
Wenn Ihnen diese Fehlermeldung beim Zuordnen eines Datasets zu einem SQL-basierten Ziel angezeigt wird, liegt dies möglicherweise daran, dass Sie keinen Benutzer für die verwaltete Identität von Azure Data Share auf Ihrer SQL Server-Instanz erstellt haben.  Führen Sie zum Beheben dieses Problems das folgende Skript aus:

```sql
    create user "<share_acc_name>" from external provider; 
    exec sp_addrolemember db_datareader, "<share_acc_name>"; 
    exec sp_addrolemember db_datawriter, "<share_acc_name>"; 
    exec sp_addrolemember db_ddladmin, "<share_acc_name>";
```
Beachten Sie, dass *<share_acc_name>* der Name Ihrer Data Share-Ressource ist.      

Stellen Sie sicher, dass Sie alle Voraussetzungen befolgt haben, die im Tutorial [Freigeben Ihrer Daten](share-your-data.md) und [Akzeptieren und Empfangen von Daten](subscribe-to-data-share.md) aufgeführt sind.

## <a name="snapshot-failed"></a>Fehler bei Momentaufnahme
Die Momentaufnahme kann aus einer Vielzahl von Gründen fehlschlagen. Die ausführliche Fehlermeldung können Sie finden, indem Sie auf die Startzeit der Momentaufnahme und dann auf den Status jedes Datasets klicken. 

Wenn sich die Fehlermeldung auf eine Berechtigung bezieht, überprüfen Sie, ob der Data Share-Dienst über die erforderliche Berechtigung verfügt. Ausführliche Informationen finden Sie unter [Rollen und Anforderungen](concepts-roles-permissions.md). Wenn Sie zum ersten Mal eine Momentaufnahme erstellen, könnte es einige Minuten dauern, bis die Data Share-Ressource Zugriff auf den Azure-Datenspeicher erhält. Warten Sie einige Minuten, und versuchen Sie es erneut.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Freigeben von Daten finden Sie im Tutorial zum [Freigeben Ihrer Daten](share-your-data.md). 

Weitere Informationen zum Empfangen von Daten finden Sie im Tutorial [Akzeptieren und Empfangen von Daten](subscribe-to-data-share.md).

