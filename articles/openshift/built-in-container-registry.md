---
title: Konfigurieren der integrierten Containerregistrierung für Azure Red Hat OpenShift 4
description: Konfigurieren der integrierten Containerregistrierung für Azure Red Hat OpenShift 4
author: jiangma
ms.author: jiangma
ms.service: container-service
ms.topic: conceptual
ms.date: 10/15/2020
ms.openlocfilehash: 917da58c7f5ac2294fc30cd385a4834fde3f5f0b
ms.sourcegitcommit: 17b36b13857f573639d19d2afb6f2aca74ae56c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2020
ms.locfileid: "94413552"
---
# <a name="configure-built-in-container-registry-for-azure-red-hat-openshift-4"></a>Konfigurieren der integrierten Containerregistrierung für Azure Red Hat OpenShift 4

In diesem Artikel konfigurieren Sie die integrierte Containerimageregistrierung für einen Azure Red Hat OpenShift 4-Cluster (ARO). Sie lernen Folgendes:

> [!div class="checklist"]
> * Einrichten von Azure AD
> * Einrichten von OpenID Connect
> * Zugreifen auf die integrierte Containerimageregistrierung

## <a name="prerequisites"></a>Voraussetzungen

* Erstellen Sie einen Cluster, indem Sie die Schritte in [Erstellen eines Azure Red Hat OpenShift 4-Clusters](/azure/openshift/tutorial-create-cluster) ausführen. Erstellen Sie den Cluster unbedingt mit dem `--pull-secret`-Argument für `az aro create`.  Dies ist erforderlich, wenn Sie Azure AD für die Anmeldung einrichten möchten, wie in diesem Artikel erforderlich.
* Stellen Sie eine Verbindung mit dem Cluster her, indem Sie die Schritte in [Tutorial: Herstellen einer Verbindung mit einem Azure Red Hat OpenShift 4-Cluster](/azure/openshift/tutorial-connect-cluster) ausführen.
   * Stellen Sie sicher, dass Sie die Schritte unter „Installieren der OpenShift-CLI“ ausführen, da der `oc`-Befehl weiter unten in diesem Artikel verwendet wird.
   * Notieren Sie sich die URL der Clusterkonsole, die wie `https://console-openshift-console.apps.<random>.<region>.aroapp.io/`aussieht. Die Werte für `<random>` und `<region>` werden später in diesem Artikel verwendet.
   * Notieren Sie sich die `kubeadmin`-Anmeldeinformationen. Sie werden später in diesem Artikel verwendet.

## <a name="set-up-azure-active-directory"></a>Einrichten von Azure Active Directory

Azure Active Directory (Azure AD) implementiert OpenID Connect (OIDC). Mit OIDC können Sie Azure AD für die Anmeldung beim ARO-Cluster verwenden. Gehen Sie folgendermaßen vor, um Azure AD einzurichten.

1. Richten Sie einen Azure AD-Mandanten ein, indem Sie die Schritte in [Schnellstart: Einrichten eines Mandanten](/azure/active-directory/develop/quickstart-create-new-tenant) ausführen. Ihr Azure-Konto verfügt möglicherweise bereits über einen Mandanten. Wenn dies der Fall ist, können Sie die Erstellung überspringen, wenn Sie über ausreichende Berechtigungen im Mandanten verfügen, um die Schritte auszuführen. Notieren Sie sich Ihre **Mandanten-ID**. Dieser Wert wird später noch verwendet.
2. Erstellen Sie mindestens einen Azure AD-Benutzer, indem Sie die Schritte in [Hinzufügen oder Löschen von Benutzern in Azure Active Directory](/azure/active-directory/fundamentals/add-users-azure-active-directory) ausführen. Mit diesen Konten oder Ihren eigenen können Sie die Anwendung testen. Notieren Sie sich die E-Mail-Adressen und Kennwörter dieser Konten für die Anmeldung.
3. Erstellen Sie eine neue Anwendungsregistrierung in Ihrem Azure AD-Mandanten, indem Sie die Schritte in [Schnellstart: Registrieren einer Anwendung bei Microsoft Identity Platform](/azure/active-directory/develop/quickstart-register-app) ausführen. 
   1. Beachten Sie den Hinweis in den Voraussetzungen zu den Werten für `<random>` und `<region>`. Verwenden Sie diese Werte, um einen URI gemäß der folgenden Formel zu erstellen:

      `https://oauth-openshift.apps.<random>.<region>.aroapp.io/oauth2callback/openid`
   1. Wenn Sie zu dem Schritt kommen, der das Feld **Umleitungs-URI** betrifft, geben Sie den URI aus dem vorherigen Schritt ein.
   1. Wählen Sie **Registrieren**.
   1. Beachten Sie auf der Seite **Übersicht** der App den Wert, der für **Anwendungs-ID (Client)** angezeigt wird, wie hier gezeigt.
      :::image type="content" source="media/built-in-container-registry/azure-ad-app-overview-client-id.png" alt-text="Übersichtsseite der Azure AD-Anwendung.":::

4. Erstellen Sie einen neuen geheimen Clientschlüssel. 
   1. Wählen Sie in der neu erstellten Anwendungsregistrierung im linken Navigationsbereich **Zertifikate und Geheimnisse** aus.
   1. Wählen Sie **Neuer geheimer Clientschlüssel**.
   1. Fügen Sie **eine Beschreibung** hinzu, und wählen Sie **Hinzufügen** aus.
   1. Speichern Sie den generierten Wert **Geheimer Clientschlüssel**. Dieser Wert wird später in diesem Artikel verwendet.

### <a name="add-openid-connect-identity-provider"></a>Hinzufügen des OpenID Connect-Identitätsanbieters

ARO bietet eine integrierte Containerregistrierung.  Um darauf zuzugreifen, konfigurieren Sie einen Identitätsanbieter (Identity Provider, IDP) mithilfe von Azure AD OIDC, indem Sie die folgenden Schritte ausführen.

1. Melden Sie sich über Ihren Browser bei der OpenShift-Webkonsole als `kubeadmin` an.  Dasselbe Verfahren wird beim Ausführen der Schritte in [Tutorial: Herstellen einer Verbindung mit einem Azure Red Hat OpenShift 4-Cluster](/azure/openshift/tutorial-connect-cluster) verwendet.
1. Wählen Sie im linken Navigationsbereich **Verwaltung** > **Clustereinstellungen** aus.
1. Wählen Sie in der Mitte der Seite **Globale Konfiguration** aus.
1. Wählen Sie **OAuth** in der Spalte **Konfigurationsressource** der Tabelle aus.
1. Wählen Sie unter **Identitätsanbieter** die Option **Hinzufügen** und **OpenID Connect** aus.
1. Legen Sie **openid** für **Name** fest.  Dieser Wert ist möglicherweise bereits eingetragen.
1. Legen Sie **Client-ID** und **Geheimer Clientschlüssel** als Werte aus dem vorherigen Schritt fest.
1. Führen Sie diesen Schritt aus, um den Wert für **Aussteller-URL** zu ermitteln.
   1. Ersetzen Sie **\<tenant-id>** durch die im Abschnitt **Einrichten von Azure Active Directory** in der URL `https://login.microsoftonline.com/<tenant-id>/v2.0/.well-known/openid-configuration` gespeicherte.
   1. Öffnen Sie die URL im gleichen Browser, den Sie für die Interaktion mit dem Azure-Portal verwendet haben.
   1. Kopieren Sie den Wert der Eigenschaft **Aussteller** in den zurückgegebenen JSON-Text, und fügen Sie ihn in das Textfeld mit der Bezeichnung **Aussteller-URL** ein.  Der Wert **Aussteller** sieht dann etwa wie `https://login.microsoftonline.com/44p4o433-2q55-474q-on88-4on94469o74n/v2.0` aus.
1. Wählen Sie unten auf der Seite **Hinzufügen** aus.
   :::image type="content" source="media/built-in-container-registry/openid-connect-identity-provider.png" alt-text="OpenID Connect in OpenShift.":::
1. Melden Sie sich von der OpenShift-Webkonsole ab, indem Sie oben rechts im Browserfenster die Schaltfläche **kube:admin** und dann **Abmelden** auswählen.

### <a name="access-the-built-in-container-image-registry"></a>Zugreifen auf die integrierte Containerimageregistrierung

Die folgenden Anweisungen ermöglichen den Zugriff auf die integrierte Registrierung.

#### <a name="define-the-azure-ad-user-to-be-an-administrator"></a>Definieren des Azure AD-Benutzers als Administrator

1. Melden Sie sich mit den Anmeldeinformationen eines Azure AD-Benutzers über Ihren Browser bei der OpenShift-Webkonsole an.

   1. Verwenden Sie eine InPrivate, Incognito- oder andere entsprechende Browserfensterfunktion, um sich bei der Konsole anzumelden.
   1. Das Fenster wird nach dem Aktivieren von OIDC anders aussehen.
   :::image type="content" source="media/built-in-container-registry/oidc-enabled-login-window.png" alt-text="OpenID Connect-aktiviertes Anmeldefenster.":::
   1. Wählen Sie **openid** aus.

   > [!NOTE]
   > Notieren Sie sich den Benutzernamen und das Kennwort, die Sie hier zum Anmelden verwenden. Dieser Benutzername und dieses Kennwort fungieren in diesem und anderen Artikeln als Administrator für andere Aktionen.
1. Melden Sie sich mit der OpenShift-CLI an, indem Sie die folgenden Schritte ausführen.  Für die Diskussion wird dieser Prozess als `oc login` bezeichnet.
   1. Erweitern Sie rechts oben in der Webkonsole das Kontextmenü des angemeldeten Benutzers, und wählen Sie dann **Anmeldebefehl kopieren** aus.
   1. Melden Sie sich bei Bedarf als derselbe Benutzer bei einem neuen Registerkartenfenster an.
   1. Wählen Sie **Token anzeigen** aus.
   1. Kopieren Sie den unter **Mit diesem Token anmelden** aufgeführten Wert in die Zwischenablage, und führen Sie ihn wie hier gezeigt in einer Shell aus.

       ```bash
       oc login --token=XOdASlzeT7BHT0JZW6Fd4dl5EwHpeBlN27TAdWHseob --server=https://api.aqlm62xm.rnfghf.aroapp.io:6443
       Logged into "https://api.aqlm62xm.rnfghf.aroapp.io:6443" as "kube:admin" using the token provided.

       You have access to 57 projects, the list has been suppressed. You can list all projects with 'oc projects'

       Using project "default".
       ```

1. Führen Sie `oc whoami` in der Konsole aus, und notieren Sie sich die Ausgabe als **\<aad-user>** .  Wir werden diesen Wert später im Artikel verwenden.
1. Melden Sie sich von der OpenShift-Webkonsole ab. Wählen Sie die Schaltfläche oben rechts im Browserfenster mit der Bezeichnung **\<aad-user>** und dann **Abmelden** aus.


#### <a name="grant-the-azure-ad-user-the-necessary-roles-for-registry-interaction"></a>Gewähren der erforderlichen Rollen für die Registrierungsinteraktion für den Azure AD-Benutzer

1. Melden Sie sich mit den `kubeadmin`-Anmeldeinformationen über Ihren Browser bei der OpenShift-Webkonsole an.
1. Melden Sie sich mit dem Token für `kubeadmin` bei der OpenShift-CLI an, indem Sie die oben für `oc login` beschriebenen Schritte ausführen, aber erst nach der Anmeldung bei der Webkonsole mit `kubeadmin`.
1. Führen Sie die folgenden Befehle aus, um den Zugriff auf die integrierte Registrierung für den **aad-user** zu aktivieren.

   ```bash
   # Switch to project "openshift-image-registry"
   oc project openshift-image-registry
   ```

   Die Ausgabe sollte in etwa wie folgt aussehen:

   ```bash
   Now using project "openshift-image-registry" on server "https://api.x8xl3f4y.eastus.aroapp.io:6443".
   ```

   ```bash
   # Expose the registry using "DefaultRoute"
   oc patch configs.imageregistry.operator.openshift.io/cluster --patch '{"spec":{"defaultRoute":true}}' --type=merge
   ```

   Die Ausgabe sollte in etwa wie folgt aussehen:

   ```bash
   config.imageregistry.operator.openshift.io/cluster patched
   ```

   ```bash
   # Add roles to "aad-user" for pulling and pushing images
   # Note: replace "<aad-user>" with the one you wrote down before
   oc policy add-role-to-user registry-viewer <aad-user>
   ```

   Die Ausgabe sollte in etwa wie folgt aussehen:

   ```bash
   clusterrole.rbac.authorization.k8s.io/registry-viewer added: "kaaIjx75vFWovvKF7c02M0ya5qzwcSJ074RZBfXUc34"
   ```

   ```bash
   oc policy add-role-to-user registry-editor <aad-user>
   ```

   Die Ausgabe sollte in etwa wie folgt aussehen:

   ```bash
   clusterrole.rbac.authorization.k8s.io/registry-editor added: "kaaIjx75vFWovvKF7c02M0ya5qzwcSJ074RZBfXUc34"
   ```

#### <a name="obtain-the-container-registry-url"></a>Abrufen der URL der Containerregistrierung

Verwenden Sie den Befehl `oc get route` wie im Folgenden gezeigt, um die URL der Containerregistrierung abzurufen.

```bash
# Note: the value of "Container Registry URL" in the output is the fully qualified registry name.
HOST=$(oc get route default-route --template='{{ .spec.host }}')
echo "Container Registry URL: $HOST"
```

   > [!NOTE]
   > Beachten Sie die Konsolenausgabe von **Container Registry URL**. Sie wird als voll qualifizierter Registrierungsname für dieses Handbuch und nachfolgende Informationen verwendet.

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die integrierte Containerimageregistrierung, indem Sie eine Anwendung auf OpenShift bereitstellen.  Befolgen Sie für Java-Anwendungen den Leitfaden [Bereitstellen einer Java-Anwendung mit Open Liberty/WebSphere Liberty in Azure Red Hat OpenShift 4-Clustern](howto-deploy-java-liberty-app.md).
