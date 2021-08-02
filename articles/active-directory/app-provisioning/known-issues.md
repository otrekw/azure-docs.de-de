---
title: Bekannte Probleme bei der Anwendungsbereitstellung in Azure Active Directory
description: Hier finden Sie Informationen über bekannte Probleme bei der Arbeit mit der automatisierten Anwendungsbereitstellung in Azure Active Directory.
author: kenwith
ms.author: kenwith
manager: mtillman
services: active-directory
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.topic: troubleshooting
ms.date: 05/28/2021
ms.reviewer: arvinh
ms.openlocfilehash: 1674e3aae978c16b8ef736dc6605bd30e7201e10
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111962027"
---
# <a name="known-issues-for-application-provisioning-in-azure-active-directory"></a>Bekannte Probleme bei der Anwendungsbereitstellung in Azure Active Directory
In diesem Artikel werden bekannte Probleme behandelt, die Sie bei der App-Bereitstellung berücksichtigen sollten. Sie können Ihr Feedback zum Anwendungsbereitstellungsdienst über UserVoice angeben. Weitere Informationen finden Sie auf der [UserVoice-Seite zur Azure AD-Anwendungsbereitstellung](https://aka.ms/appprovisioningfeaturerequest). Wir beobachten UserVoice genau, damit wir den Dienst verbessern können. 

> [!NOTE]
> Hierbei handelt es sich nicht um eine vollständige Liste bekannter Probleme. Wenn Sie ein Problem kennen, das hier nicht aufgeführt wird, teilen Sie Ihr Feedback unten auf der Seite mit.

## <a name="authorization"></a>Authorization 

**Speichern ist nach erfolgreichem Verbindungstest nicht möglich**

Wenn Sie eine Verbindung erfolgreich testen können, aber anschließend keinen Speichervorgang durchführen können, haben Sie die zulässige Speicherbeschränkung für Anmeldeinformationen überschritten. Weitere Informationen finden Sie unter [Problem beim Speichern der Administratoranmeldeinformationen](./user-provisioning.md).

**Speichervorgang kann nicht ausgeführt werden**

Die Mandanten-URL, das Geheimnistoken und die Benachrichtigungs-E-Mail-Adresse müssen angegeben sein, damit der Speichervorgang ausgeführt werden kann. Sie können nicht nur eine der Informationen angeben. 

**Bereitstellungsmodus kann nicht wieder in „manuell“ geändert werden**

Nachdem Sie die Bereitstellung zum ersten Mal konfiguriert haben, werden Sie feststellen, dass der Bereitstellungsmodus von „manuell“ in „automatisch“ geändert wurde. Sie können ihn nicht wieder in „manuell“ ändern. Allerdings können Sie die Bereitstellung über die Benutzeroberfläche deaktivieren. Indem Sie die Bereitstellung über die Benutzeroberfläche deaktivieren, erzielen Sie dasselbe Ergebnis, wie durch das Festlegen der Dropdownoption auf „manuell“.  


## <a name="attribute-mappings"></a>Attributzuordnungen 

**Das Attribut „SamAccountName“ oder „userType“ ist nicht als Quellattribut verfügbar**

Die Attribute „SamAccountName“ und „userType“ sind nicht standardmäßig als Quellattribut verfügbar. Erweitern Sie Ihr Schema, um das Attribut hinzuzufügen. Sie können die Attribute zur Liste der verfügbaren Quellattribute hinzufügen, indem Sie Ihr Schema erweitern. Weitere Informationen finden Sie unter [Fehlendes Quellattribut](user-provisioning-sync-attributes-for-mapping.md). 

**Dropdownmenü für Quellattribute fehlt für die Schemaerweiterung**

Erweiterungen an Ihrem Schema fehlen manchmal im Dropdownmenü für Quellattribute in der Benutzeroberfläche. Öffnen Sie die erweiterten Einstellungen Ihrer Attributzuordnungen, und fügen Sie die Attribute manuell hinzu. Weitere Informationen finden Sie unter [Anpassen von Attributzuordnungen](customize-application-attributes.md).

**NULL-Attribut kann nicht bereitgestellt werden**

Azure AD kann derzeit keine NULL-Attribute bereitstellen. Wenn ein Attribut für das Benutzerobjekt NULL lautet, wird es übersprungen. 

**Maximale Zeichenanzahl für Ausdrücke für Attributzuordnungen**

Ausdrücke für die Attributzuordnung können über maximal 10.000 Zeichen verfügen. 

**Nicht unterstützte Bereichsfilter**

Verzeichniserweiterungen, appRoleAssignments, userType und accountExpires werden nicht als Bereichsfilter unterstützt.


## <a name="service-issues"></a>Dienstprobleme 

**Nicht unterstützte Szenarien**

- Das Bereitstellen von Kennwörtern wird nicht unterstützt. 
- Das Bereitstellen von geschachtelten Gruppen wird nicht unterstützt. 
- Die Bereitstellung von B2C-Mandanten wird aufgrund der Größe der Mandanten nicht unterstützt.
- Nicht alle Bereitstellungs-Apps sind in allen Clouds verfügbar. Beispielsweise ist Atlassian noch nicht in der Government-Cloud verfügbar. Wir arbeiten mit App-Entwicklern daran, ihre Apps in alle Clouds zu integrieren.

**Die automatische Bereitstellung ist in meiner OICD-basierten Anwendung nicht verfügbar**

Wenn Sie eine App-Registrierung erstellen, wird der entsprechende Dienstprinzipal in Unternehmens-Apps nicht für die automatische Benutzerbereitstellung aktiviert. Sie müssen entweder das Hinzufügen der App zum Katalog anfordern, wenn sie von mehreren Organisationen genutzt werden soll, oder eine zweite Nicht-Katalog-App für die Bereitstellung erstellen. 

**Das Bereitstellungsintervall ist festgelegt**

Die [Zeit](./application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users) zwischen den Bereitstellungszyklen ist derzeit nicht konfigurierbar. 

**Änderungen werden nicht von der Ziel-App zu Azure AD migriert**

Der App-Bereitstellungsdienst erkennt keine Änderungen an externen Apps. Daher wird keine Rollbackaktion ausgeführt. Der App-Bereitstellungsdienst verlässt sich auf Änderungen, die in Azure AD vorgenommen werden. 

**Wechsel von „Sync All“ (Alle synchronisieren) zu „Sync Assigned“ (Zugewiesene synchronisieren) nicht möglich**

Nachdem Sie den Bereich von „Sync All“ (Alle synchronisieren) in „Sync Assigned“ (Zugewiesene synchronisieren) geändert haben, müssen Sie einen Neustart durchführen, damit die Änderung wirksam wird. Sie können den Neustart über die Benutzeroberfläche durchführen.

**Bereitstellungszyklus wird bis zum Abschluss fortgesetzt**

Beim Festlegen von `enabled = off` für die Bereitstellung oder beim Beenden wird der aktuelle Bereitstellungszyklus bis zum Abschluss fortgesetzt. Der Dienst führt keine zukünftigen Zyklen aus, bis Sie die Bereitstellung wieder aktivieren.

**Mitglied der Gruppe wird nicht bereitgestellt**

Wenn sich eine Gruppe im Gültigkeitsbereich und ein Mitglied außerhalb des Gültigkeitsbereichs befindet, wird die Gruppe bereitgestellt. Der unzulässige Benutzer wird nicht bereitgestellt. Wenn das Mitglied den Gültigkeitsbereich wieder betritt, erkennt der Dienst die Änderung nicht sofort. Dieses Problem wird durch einen Neustart der Bereitstellung behoben. Es wird empfohlen, den Dienst regelmäßig neu zu starten, um sicherzustellen, dass alle Benutzer ordnungsgemäß bereitgestellt werden.  

**Verwalter nicht bereitgestellt**

Wenn sich ein Benutzer und der entsprechende Verwalter im Gültigkeitsbereich für die Bereitstellung befinden, stellt der Dienst den Benutzer bereit und aktualisiert dann den Verwalter. Sollte sich jedoch am ersten Tag der Benutzer im Gültigkeitsbereich befinden, der Verwalter aber nicht, stellen wir den Benutzer ohne Verwalterverweis bereit. Wenn der Verwalter sich dann zu einem späteren Zeitpunkt im Gültigkeitsbereich befindet, wird der Verwalterverweis erst aktualisiert, wenn Sie die Bereitstellung neu starten und den Dienst veranlassen, alle Benutzer erneut zu bewerten. 

## <a name="on-premises-application-provisioning"></a>Bereitstellung lokaler Anwendungen
Die folgenden Informationen enthalten eine aktuelle Liste sämtlicher bekannter Einschränkungen für den Azure AD ECMA-Connectorhost und für die Bereitstellung von lokalen Anwendungen.

### <a name="application-and-directories"></a>Anwendungen und Verzeichnisse
Die folgenden Anwendungen und Verzeichnisse werden noch nicht unterstützt.

**AD DS: (Benutzer-/Gruppenrückschreiben von Azure AD mithilfe der lokalen Bereitstellungsvorschau)**
   - Wenn ein Benutzer von Azure AD Connect verwaltet wird, ist die zugehörige Autoritätsquelle das lokale Active Directory. Deshalb können Benutzerattribute in Azure AD nicht geändert werden. Diese Vorschau kann die Autoritätsquelle für Benutzer, die von der Azure AD Connect verwaltet werden, nicht ändern.
   - Der Versuch, Azure AD Connect und die lokale Bereitstellung zu nutzen, um Gruppen/Benutzer in AD DS bereitzustellen, kann zur Entstehung einer Schleife führen, bei der Azure AD Connect Änderungen überschreibt, die vom Cloud-Bereitstellungsdienst vorgenommen wurden. Microsoft arbeitet an der Entwicklung einer dedizierten Funktion für das Gruppen-/Benutzerrückschreiben.  Stimmen Sie [hier](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/16887037-enable-user-writeback-to-on-premise-ad-from-azure) über das UserVoice-Feedback ab, um den Status der Vorschau zu verfolgen. Alternativ können Sie [Microsoft Identity Manager](/microsoft-identity-manager/microsoft-identity-manager-2016) für das Benutzer-/Gruppenrückschreiben von Azure AD zu AD verwenden.

**Andere Connectors als SQL**
   - Der Azure AD ECMA-Connectorhost wird offiziell für generische SQL-Connectors (GSQL) unterstützt. Es ist zwar möglich, andere Connectors wie den Webdienstconnector oder benutzerdefinierte ECMA-Connectors zu verwenden, aber dies wird **noch nicht unterstützt**.

**Azure Active Directory**
   - Mit der lokalen Bereitstellung können Sie einen Benutzer aus Azure AD in einer Drittanbieteranwendung bereitstellen. **Es ist jedoch nicht möglich, einen Benutzer aus einer Drittanbieteranwendung in das Verzeichnis zu verschieben.** Kunden müssen sich auf unsere nativen HR-Integrationen, Azure AD Connect, MIM oder Microsoft Graph verlassen, um Benutzer in das Verzeichnis zu integrieren.

### <a name="attributes-and-objects"></a>Attribute und Objekte 
Die folgenden Attribute und Objekte werden nicht unterstützt:
   - Mehrwertige Attribute
   - Verweisattribute (z. B. manager).
   - Gruppen
   - Komplexe Anker (z. B. ObjectTypeName+UserName).
   - Lokale Anwendungen befinden sich manchmal nicht im Verbund mit Azure AD und benötigen deshalb lokale Kennwörter. Die lokale Bereitstellungsvorschau **unterstützt nicht die Bereitstellung einmaliger Kennwörter oder das Synchronisieren von Kennwörtern** zwischen Azure AD und Drittanbieteranwendungen.
   - Das Attribut export_password sowie die Vorgänge SetPassword und ChangePassword werden nicht unterstützt.

#### <a name="ssl-certificates"></a>SSL-Zertifikate
   - Für den Azure AD ECMA-Connectorhost muss derzeit entweder das SSL-Zertifikat von Azure als vertrauenswürdig eingestuft oder der Bereitstellungs-Agent verwendet werden. Das Subjekt des Zertifikats muss mit dem Namen des Hosts übereinstimmen, auf dem der Azure AD ECMA-Connectorhost installiert ist.

#### <a name="anchor-attributes"></a>Ankerattribute
   - Der Azure AD ECMA-Connectorhost unterstützt derzeit keine Änderungen von Ankerattributen (Umbenennungen) oder Zielsysteme, die mehrere Attribute erfordern, um einen Anker zu bilden. 

#### <a name="attribute-discovery-and-mapping"></a>Attributermittlung und -zuordnung
   - Die von der Zielanwendung unterstützten Attribute werden zunächst ermittelt und anschließend im Azure-Portal im Bereich Attributzuordnungen angezeigt. Auch neu hinzugefügte Attribute werden ermittelt. Wenn sich jedoch der Typ eines Attributs ändert, das bereits Teil der Zuordnung ist (z. B. eine Zeichenfolge in einen booleschen Wert), dann wird dieser Typ nicht automatisch im Azure-Portal geändert. Kunden müssen im Bereich Zuordnungen die erweiterten Einstellungen aufrufen und den Attributtyp manuell aktualisieren.

## <a name="next-steps"></a>Nächste Schritte
- [Funktionsweise der Bereitstellung](how-provisioning-works.md)