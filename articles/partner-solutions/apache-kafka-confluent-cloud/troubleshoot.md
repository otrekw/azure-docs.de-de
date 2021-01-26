---
title: 'Problembehandlung bei Apache Kafka in Confluent Cloud: Azure-Partnerlösungen'
description: Dieser Artikel enthält Informationen zur Problembehandlung und Antworten auf häufig gestellte Fragen (FAQ) zur Confluent Cloud in Azure.
ms.service: partner-services
ms.topic: conceptual
ms.date: 01/15/2021
author: tfitzmac
ms.author: tomfitz
ms.openlocfilehash: cbf166086a489165e8100dafd7c212ab6c298b41
ms.sourcegitcommit: 25d1d5eb0329c14367621924e1da19af0a99acf1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2021
ms.locfileid: "98253253"
---
# <a name="troubleshooting-apache-kafka-for-confluent-cloud-solutions"></a>Problembehandlung bei Apache Kafka für Confluent Cloud-Lösungen

Dieses Dokument enthält Informationen zur Problembehandlung bei Lösungen, die Apache Kafka für Confluent Cloud verwenden.

Wenn Sie hier keine Antwort finden oder Ihr Problem nicht beheben können, [erstellen Sie eine Anfrage über das Azure-Portal](manage.md#get-support), oder wenden Sie sich an den [Confluent-Support](https://support.confluent.io).

## <a name="cant-find-offer-in-the-marketplace"></a>Angebot kann im Marketplace nicht gefunden werden

Um das Angebot im Azure Marketplace zu finden, führen Sie die folgenden Schritte aus:

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Ressource erstellen**.
1. Suchen Sie nach _Apache Kafka in Confluent Cloud_.
1. Wählen Sie die Anwendungskachel aus.

Wenn das Angebot nicht angezeigt wird, wenden Sie sich an den [Confluent-Support](https://support.confluent.io). Die ID Ihres Azure Active Directory Mandanten muss in der Liste der zulässigen Mandanten enthalten sein. Informationen zum Suchen Ihrer Mandanten-ID finden Sie unter [Ermitteln Ihrer Azure Active Directory-Mandanten-ID](../../active-directory/fundamentals/active-directory-how-to-find-tenant.md).

## <a name="conflict-error"></a>Konfliktfehler

Wenn Sie sich bereits für Confluent Cloud registriert haben, müssen Sie eine neue E-Mail-Adresse verwenden, um eine weitere Confluent Cloud-Organisationsressource zu erstellen. Falls Sie eine bereits registrierte E-Mail-Adresse verwenden, erhalten Sie einen Fehler zu einem **Konflikt**. Registrieren Sie sich erneut, aber diesmal mit einer anderen E-Mail-Adresse.

## <a name="deploymentfailed-error"></a>Fehler „DeploymentFailed“

Wenn Sie den Fehler **DeploymentFailed** erhalten, überprüfen Sie den Status Ihres Azure-Abonnements. Stellen Sie sicher, dass es nicht gesperrt wurde und keine Abrechnungsprobleme aufgetreten sind.

## <a name="resource-isnt-displayed"></a>Ressource wird nicht angezeigt.

Wenn die Blätter **Übersicht** oder **Löschen** für Confluent Cloud im Portal nicht angezeigt werden, aktualisieren Sie die Seite. Dieser Fehler kann durch ein vorübergehendes Problem mit dem Portal verursacht werden. Wenn dies nicht funktioniert, wenden Sie sich an den [Confluent-Support](https://support.confluent.io).

Wenn die Confluent Cloud-Ressource nicht in der Liste **Alle Ressourcen** in Azure enthalten ist, wenden Sie sich an den [Confluent-Support](https://support.confluent.io).

## <a name="resource-creation-takes-long-time"></a>Ressourcenerstellung dauert lange.

Sollte der Bereitstellungsprozess länger als drei Stunden dauern, wenden Sie sich an den Support.

Wenn bei der Bereitstellung ein Fehler auftritt und die Confluent Cloud-Ressource den Status `Failed` aufweist, löschen Sie die Ressource. Versuchen Sie nach dem Löschen, die Ressource erneut zu erstellen.

## <a name="offer-plan-doesnt-load"></a>Angebotsplan wird nicht geladen.

Dieser Fehler kann durch ein vorübergehendes Problem mit dem Azure-Portal verursacht werden. Versuchen Sie erneut, das Angebot bereitzustellen.

## <a name="unable-to-delete"></a>Der

Wenn Sie keine Confluent-Ressourcen löschen können, vergewissern Sie sich, dass Sie über die Berechtigungen zum Löschen der Ressource verfügen. Sie müssen berechtigt sein, Aktionen vom Typ „Microsoft.Confluent/*/Delete“ auszuführen. Weitere Informationen zum Anzeigen von Berechtigungen finden Sie unter [Auflisten von Azure-Rollenzuweisungen über das Azure-Portal](../../role-based-access-control/role-assignments-list-portal.md).

Wenn Sie über die erforderlichen Berechtigungen verfügen, aber die Ressource trotzdem nicht löschen können, wenden Sie sich an den [Confluent-Support](https://support.confluent.io). Dieser Zustand hängt möglicherweise mit der Aufbewahrungsrichtlinie von Confluent zusammen. Der Confluent-Support kann die Organisation und die E-Mail-Adresse für Sie löschen.

## <a name="unable-to-use-single-sign-on"></a>Einmaliges Anmelden nicht möglich

Wenn einmaliges Anmelden (Single Sign-On, SSO) im SaaS-Portal von Confluent Cloud nicht funktioniert, vergewissern Sie sich, dass Sie die richtige Azure Active Directory-E-Mail-Adresse verwenden. Außerdem müssen Sie eingewilligt haben, den Zugriff für das SaaS-Portal (Software-as-a-Service) von Confluent Cloud zuzulassen. Weitere Informationen finden Sie im [Leitfaden zum einmaligen Abmelden](manage.md#single-sign-on).

Wenn das Problem weiterhin besteht, wenden Sie sich an den [Confluent-Support](https://support.confluent.io).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das [Verwalten Ihrer Instanz](manage.md) von Apache Kafka für Confluent Cloud.
