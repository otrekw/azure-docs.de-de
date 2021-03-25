---
title: Übersicht über Apache Kafka für Confluent Cloud – Azure-Partnerlösungen
description: Erfahren Sie mehr über die Verwendung von Apache Kafka für Confluent Cloud im Azure Marketplace.
author: tfitzmac
ms.topic: conceptual
ms.service: partner-services
ms.date: 01/15/2021
ms.author: tomfitz
ms.openlocfilehash: b4aa00b7a2cc7ca6bc968be70957c127a8e00738
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98253254"
---
# <a name="what-is-apache-kafka-for-confluent-cloud"></a>Was ist Apache Kafka für Confluent Cloud?

Apache Kafka für Confluent Cloud ist ein Azure Marketplace-Angebot, das Apache Kafka als Dienst bereitstellt. Es ist vollständig verwaltet, sodass Sie sich statt auf die Clusterverwaltung auf das Entwickeln Ihrer Anwendungen konzentrieren können.

Um die Last der plattformübergreifenden Verwaltung zu verringern, hat Microsoft zusammen mit Confluent Cloud eine integrierte Bereitstellungsschicht von Azure in Confluent Cloud entwickelt. Diese bietet eine konsolidierte Benutzeroberfläche für die Verwendung von Confluent Cloud in Azure. Sie können Confluent Cloud problemlos integrieren und mit Ihren Azure-Anwendungen verwalten.

Zuvor mussten Sie das Confluent Cloud-Angebot im Marketplace erwerben und das Konto separat in Confluent Cloud einrichten. Zur Verwaltung von Konfigurationen und Ressourcen mussten Sie zwischen den Portalen für Azure und Confluent Cloud navigieren.

Jetzt stellen Sie die Confluent Cloud-Ressourcen über einen Ressourcenanbieter namens **Microsoft.Confluent** bereit. Confluent Cloud-Organisationsressourcen werden über das [Azure-Portal](https://portal.azure.com/), die [Azure CLI](/cli/azure/) oder [Azure SDKs](/azure/#languages-and-tools) erstellt und verwaltet. Confluent Cloud besitzt die SaaS-Anwendung (Software-as-a-Service) und führt sie aus, einschließlich der Umgebungen, Cluster, Themen, API-Schlüssel und verwalteten Connectors.

## <a name="capabilities"></a>Funktionen

Die umfassende Integration zwischen Confluent Cloud und Azure ermöglicht die folgenden Funktionen:

- Bereitstellen einer neuen Confluent Cloud-Organisationsressource mit vollständig verwalteter Infrastruktur über das Azure-Portal.
- Optimieren des einmaligen Anmeldens (Single Sign-On, SSO) über Azure bei Confluent Cloud mithilfe von Azure Active Directory (Azure AD). Eine separate Authentifizierung über das Confluent Cloud-Portal ist nicht erforderlich.
- Abrufen einer einheitlichen Abrechnung für den Confluent Cloud-Verbrauch über die Rechnungsstellung für Azure-Abonnements.
- Verwalten von Confluent Cloud-Ressourcen über das Azure-Portal und Verfolgen dieser Ressourcen zusammen mit Ihren anderen Azure-Ressourcen auf der Seite **Alle Ressourcen**.

## <a name="confluent-organization"></a>Confluent-Organisation

Bei einer Confluent-Organisation handelt es sich um eine Ressource, die die Zuordnung zwischen Azure- und Confluent Cloud-Ressourcen ermöglicht. Sie ist die übergeordnete Ressource für andere Confluent Cloud-Ressourcen.

Jedes Azure-Abonnement kann mehrere Confluent-Pläne enthalten. Jeder Confluent-Plan wird einem Benutzerkonto und einer Organisation im Confluent-Portal zugeordnet. In jeder Confluent-Organisation können Sie mehrere Umgebungen, Cluster, Themen und Connectors erstellen.

Wenn Sie in Azure eine Confluent Cloud-Ressource bereitstellen, erhalten Sie eine Confluent-Organisations-ID, eine Standardumgebung und ein Benutzerkonto. Weitere Informationen finden Sie unter [Schnellstart: Erste Schritte mit Confluent Cloud in Azure](create.md).

Bei der Abrechnung wird jedes im Marketplace erworbene Confluent Cloud-Angebot einer eindeutigen Confluent-Organisation zugeordnet.

## <a name="single-sign-on"></a>Einmaliges Anmelden

Wenn Sie sich beim Azure-Portal anmelden, werden Ihre Anmeldeinformationen auch für die Anmeldung beim Confluent Cloud-SaaS-Portal verwendet. Bei dieser Funktion werden [Azure AD](../../active-directory/fundamentals/active-directory-whatis.md) und [Azure AD-SSO](../../active-directory/manage-apps/what-is-single-sign-on.md) eingesetzt, um Ihnen eine sichere und bequeme Möglichkeit zur Anmeldung zu bieten.

Weitere Informationen finden Sie unter [Einmaliges Anmelden](manage.md#single-sign-on).

## <a name="billing"></a>Abrechnung

Es stehen zwei Abrechnungsoptionen zur Verfügung: ein Plan mit monatlicher nutzungsbasierter Bezahlung und ein Plan mit Mindestabnahme.

- Beim Plan für die **monatliche nutzungsbasierte Bezahlung** werden die Verbrauchsgebühren für Confluent Cloud auf Ihrer monatlichen Azure-Rechnung aufgeführt.
- Bei einem **Plan mit Mindestabnahme** registrieren Sie sich für einen Mindestbetrag und erhalten einen Rabatt auf Ihre zugesicherte Nutzung von Confluent Cloud.

Sie entscheiden beim Erstellen des Diensts, welche Abrechnungsoption verwendet werden soll.

## <a name="confluent-links"></a>Confluent-Links

Weitere Hilfe bei der Verwendung von Apache Kafka für Confluent Cloud finden Sie unter den folgenden Links zur [Confluent-Website](https://docs.confluent.io/home/overview.html).

Informationen zu Abrechnungsoptionen finden Sie auf folgenden Seiten:

* [Azure Marketplace mit nutzungsbasierter Bezahlung](https://docs.confluent.io/cloud/current/billing/ccloud-azure-payg.html)
* [Azure Marketplace mit Mindestabnahme](https://docs.confluent.io/cloud/current/billing/ccloud-azure-ubb.html)

Weitere Informationen zum Verwalten der Lösungen finden Sie auf folgenden Seiten:

* [Erstellen eines Clusters in Confluent Cloud](https://docs.confluent.io/cloud/current/clusters/create-cluster.html)
* [Confluent Cloud-Umgebungen](https://docs.confluent.io/current/cloud/using/environments.html)
* [Grundlagen zu Confluent Cloud](https://docs.confluent.io/current/cloud/using/cloud-basics.html)

Support und Bestimmungen finden Sie auf folgenden Seiten:

* [Confluent-Support](https://support.confluent.io)
* [Vertragsbedingungen](https://www.confluent.io/confluent-cloud-tos).

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Erstellen einer Instanz von Apache Kafka für Confluent Cloud finden Sie unter [Schnellstart: Erste Schritte mit Confluent Cloud in Azure](create.md).
