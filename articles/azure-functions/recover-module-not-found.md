---
title: Behandeln von Python-Fehlern vom Typ „ModuleNotFoundError“ in Azure Functions
description: Erfahren Sie, wie Sie Fehler aufgrund von nicht gefundenen Azure Functions-Modulen in Python-Funktionen beheben.
author: Hazhzeng
ms.topic: article
ms.date: 05/12/2020
ms.author: hazeng
ms.openlocfilehash: 191cde0f90b4968ca230db72bad68cca8b1db3fd
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83690186"
---
# <a name="troubleshoot-python-module-errors-in-azure-functions"></a>Problembehandlung von Python-Modulfehlern in Azure Functions

Dieser Artikel unterstützt Sie dabei, modulbezogene Fehler in Ihrer Python-Funktions-App zu beheben. Diese Fehler führen typischerweise zu der folgenden Azure Functions-Fehlermeldung:

> `Exception: ModuleNotFoundError: No module named 'module_name'.`

Dieser Fehler wird ausgelöst, wenn eine Python-Funktions-App ein Python-Modul nicht laden kann. Die Ursache für diesen Fehler ist einer der folgenden:

- [Das Paket wurde nicht gefunden](#the-package-cant-be-found)
- [Das Paket wird nicht mit der richtigen Linux-Wheeldatei aufgelöst](#the-package-isnt-resolved-with-proper-linux-wheel)
- [Das Paket ist nicht mit der Version des Python-Interpreters kompatibel](#the-package-is-incompatible-with-the-python-interpreter-version)
- [Das Paket führt zu einem Konflikt mit anderen Paketen](#the-package-conflicts-with-other-packages)
- [Das Paket unterstützt nur Windows- oder macOS-Plattformen](#the-package-only-supports-windows-or-macos-platforms)

## <a name="view-project-files"></a>Anzeigen von Projektdateien

Um die tatsächliche Ursache Ihres Problems zu ermitteln, müssen Sie die Python-Projektdateien abrufen, die für Ihre Funktions-App ausgeführt werden. Wenn die Projektdateien nicht auf Ihrem lokalen Computer vorliegen, haben Sie zu deren Abruf folgende Möglichkeiten:

- Wenn die Funktions-App die App-Einstellung `WEBSITE_RUN_FROM_PACKAGE` aufweist und der zugehörige Wert eine URL ist, laden Sie die Datei herunter, indem Sie die URL kopieren und in Ihren Browser einfügen.
- Wenn die Funktions-App die Einstellung `WEBSITE_RUN_FROM_PACKAGE` umfasst und diese auf `1` festgelegt ist, navigieren Sie zu `https://<app-name>.scm.azurewebsites.net/api/vfs/data/SitePackages`, und laden Sie die Datei über die aktuelle `href`-URL herunter.
- Wenn die Funktions-App die oben genannte App-Einstellung nicht aufweist, navigieren Sie zu `https://<app-name>.scm.azurewebsites.net/api/settings`, und suchen Sie unter `SCM_RUN_FROM_PACKAGE` nach der URL. Laden Sie die Datei herunter, indem Sie die URL kopieren und in Ihren Browser einfügen.
- Wenn keine dieser Methoden anwendbar ist, navigieren Sie zu `https://<app-name>.scm.azurewebsites.net/DebugConsole`, und zeigen Sie den Inhalt unter `/home/site/wwwroot` an.

In den verbleibenden Abschnitten des vorliegenden Artikels erhalten Sie Informationen zur Behebung der potenziellen Ursachen für diesen Fehler, indem Sie die Inhalte Ihrer Funktions-App untersuchen, die Problemursache beseitigen und so das Problem lösen.

## <a name="diagnose-modulenotfounderror"></a>Diagnose von „ModuleNotFoundError“

In diesem Abschnitt werden die potenziellen Ursachen modulbezogener Fehler beschrieben. Nachdem Sie die wahrscheinliche Ursache für einen Fehler kennen, können Sie ihn beheben.

### <a name="the-package-cant-be-found"></a>Das Paket wurde nicht gefunden

Navigieren Sie zu `.python_packages/lib/python3.6/site-packages/<package-name>` oder `.python_packages/lib/site-packages/<package-name>`. Wenn der Dateipfad nicht vorhanden ist, wird der Fehler wahrscheinlich durch den fehlenden Pfad ausgelöst.

Die Verwendung eines Drittanbietertools oder eines veralteten Tools während der Bereitstellung kann zu diesem Fehler führen.

Informationen zur Problembeseitigung finden Sie unter [Aktivieren der Remotekompilierung](#enable-remote-build) oder [Erstellen nativer Abhängigkeiten](#build-native-dependencies).

### <a name="the-package-isnt-resolved-with-proper-linux-wheel"></a>Das Paket wird nicht mit der richtigen Linux-Wheeldatei aufgelöst

Wechseln Sie zu `.python_packages/lib/python3.6/site-packages/<package-name>-<version>-dist-info` oder `.python_packages/lib/site-packages/<package-name>-<version>-dist-info`. Verwenden Sie Ihren bevorzugten Text-Editor, um die **Wheeldatei** zu öffnen und den Abschnitt **Tag:** zu überprüfen. Wenn der Wert des Tags nicht **linux** enthält, kann dies die Ursache des Problems sein.

Python-Funktionen können nur unter Linux in Azure ausgeführt werden: Die Functions-Runtime v2.x wird unter Debian Stretch, die Runtime v3.x unter Debian Buster ausgeführt. Das Artefakt muss die richtigen Linux-Binärdateien enthalten. Die Verwendung des `--build local`-Flags in Core- oder Drittanbietertools oder veralteten Tools kann dazu führen, dass ältere Binärdateien verwendet werden.

Informationen zur Problembeseitigung finden Sie unter [Aktivieren der Remotekompilierung](#enable-remote-build) oder [Erstellen nativer Abhängigkeiten](#build-native-dependencies).

### <a name="the-package-is-incompatible-with-the-python-interpreter-version"></a>Das Paket ist nicht mit der Version des Python-Interpreters kompatibel

Wechseln Sie zu `.python_packages/lib/python3.6/site-packages/<package-name>-<version>-dist-info` oder `.python_packages/lib/site-packages/<package-name>-<version>-dist-info`. Öffnen Sie mit einem Text-Editor die METADATA-Datei, überprüfen Sie den Abschnitt **Classifiers:** . Wenn der Abschnitt weder `Python :: 3`, `Python :: 3.6`, `Python :: 3.7` noch `Python :: 3.8` enthält, ist entweder die Paketversion zu alt, oder das Paket wird nicht mehr gewartet.

Sie können die Python-Version Ihrer Funktions-App im [Azure-Portal](https://portal.azure.com) überprüfen. Navigieren Sie zu Ihrer Funktions-App, wählen Sie den **Ressourcen-Explorer** aus, und klicken Sie auf **Los**.

:::image type="content" source="media/recover-module-not-found/resource-explorer.png" alt-text="Öffnen des Ressourcen-Explorers für die Funktions-App im Portal":::

Nachdem der Explorer geladen wurde, suchen Sie nach **LinuxFxVersion**. Diese Einstellung zeigt die Python-Version an.

Informationen zur Problembehebung finden Sie unter [Aktualisieren Ihres Pakets auf die aktuelle Version](#update-your-package-to-the-latest-version) oder [Ersetzen des Pakets durch äquivalente Pakete](#replace-the-package-with-equivalents).

### <a name="the-package-conflicts-with-other-packages"></a>Das Paket führt zu einem Konflikt mit anderen Paketen

Wenn Sie sichergestellt haben, dass das Paket mit der geeigneten Linux-Wheeldatei korrekt aufgelöst wird, liegt möglicherweise ein Konflikt mit anderen Paketen vor. In bestimmten Paketen kann die PyPi-Dokumentation auf nicht kompatible Module verweisen. In [`azure 4.0.0`](https://pypi.org/project/azure/4.0.0/) ist beispielsweise der folgende Hinweis enthalten:

<pre>This package isn't compatible with azure-storage.
If you installed azure-storage, or if you installed azure 1.x/2.x and didn’t uninstall azure-storage,
you must uninstall azure-storage first.</pre>

Sie finden die Dokumentation für Ihre Paketversion in `https://pypi.org/project/<package-name>/<package-version>`.

Informationen zur Problembehebung finden Sie unter [Aktualisieren Ihres Pakets auf die aktuelle Version](#update-your-package-to-the-latest-version) oder [Ersetzen des Pakets durch äquivalente Pakete](#replace-the-package-with-equivalents).

### <a name="the-package-only-supports-windows-or-macos-platforms"></a>Das Paket unterstützt nur Windows- oder macOS-Plattformen

Öffnen Sie `requirements.txt` mit einem Text-Editor, und überprüfen Sie das Paket in `https://pypi.org/project/<package-name>`. Einige Pakete können nur auf Windows- oder macOS-Plattformen ausgeführt werden. Beispielsweise kann pywin32 nur auf Windows ausgeführt werden.

Der Fehler `Module Not Found` tritt möglicherweise nicht auf, wenn Sie Windows oder macOS für die lokale Bereitstellung verwenden. Das Paket kann jedoch nicht in Azure Functions importiert werden, weil Functions zur Laufzeit Linux verwendet. Die wahrscheinliche Ursache hierfür ist die Verwendung von `pip freeze`, um während der Projektinitialisierung von Ihrem Windows- oder macOS-Computer aus die virtuelle Umgebung in die Datei „requirements.txt“ zu exportieren.

Informationen zur Problembehebung finden Sie unter [Ersetzen des Pakets durch äquivalente Pakete](#replace-the-package-with-equivalents) oder [Manuelles Erstellen von „requirements.txt“](#handcraft-requirementstxt).

## <a name="mitigate-modulenotfounderror"></a>Beheben von „ModuleNotFoundError“

Nachfolgend werden Möglichkeiten zum Beheben modulbezogener Probleme beschrieben. Verwenden Sie die [Diagnoseinformationen oben](#diagnose-modulenotfounderror), um zu entscheiden, welche Problembehandlung verwendet werden soll.

### <a name="enable-remote-build"></a>Aktivieren der Remotekompilierung

Stellen Sie sicher, dass die Remotekompilierung aktiviert ist. Die hierfür verwendete Vorgehensweise hängt von Ihrer Bereitstellungsmethode.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/vscode)
Stellen Sie sicher, dass die aktuelle Version der [Azure Functions-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) installiert ist. Überprüfen Sie, ob `.vscode/settings.json` vorhanden ist und die Einstellung `"azureFunctions.scmDoBuildDuringDeployment": true` enthält. Falls nicht, erstellen Sie diese Datei mit aktivierter `azureFunctions.scmDoBuildDuringDeployment`-Einstellung, und stellen Sie das Projekt erneut bereit.

# <a name="azure-functions-core-tools"></a>[Azure Functions Core Tools](#tab/coretools)

Stellen Sie sicher, dass die neueste Version der [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools/releases) installiert ist. Wechseln Sie zu Ihrem lokalen Ordner für das Funktionsprojekt, und verwenden Sie `func azure functionapp publish <app-name>` für die Bereitstellung.

# <a name="manual-publishing"></a>[Manuelle Veröffentlichung](#tab/manual)

Wenn Sie Ihr Paket manuell im `https://<app-name>.scm.azurewebsites.net/api/zipdeploy`-Endpunkt bereitstellen, stellen Sie sicher, dass sowohl **SCM_DO_BUILD_DURING_DEPLOYMENT** als auch **ENABLE_ORYX_BUILD** auf **TRUE** festgelegt sind. Weitere Informationen finden Sie unter [Arbeiten mit Anwendungseinstellungen](functions-how-to-use-azure-function-app-settings.md#settings).

---

### <a name="build-native-dependencies"></a>Erstellen nativer Abhängigkeiten

Stellen Sie sicher, dass die neueste Version von **docker** und [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools/releases) installiert ist. Wechseln Sie zu Ihrem lokalen Ordner für das Funktionsprojekt, und verwenden Sie `func azure functionapp publish <app-name> --build-native-deps` für die Bereitstellung.

### <a name="update-your-package-to-the-latest-version"></a>Aktualisieren Ihres Pakets auf die aktuelle Version

Wechseln Sie zur neuesten Paketversion in `https://pypi.org/project/<package-name>`, und überprüfen Sie den Abschnitt **Classifiers:** . Das Paket sollte als `OS Independent` gekennzeichnet oder mit `POSIX` oder `POSIX :: Linux` in **Betriebssystem** kompatibel sein. Darüber hinaus muss die Programmiersprache `Python :: 3`, `Python :: 3.6`, `Python :: 3.7` oder `Python :: 3.8` enthalten.

Wenn diese Angaben korrekt sind, können Sie das Paket auf die aktuelle Version aktualisieren, indem Sie die Zeile `<package-name>~=<latest-version>` in „requirements.txt“ ändern.

### <a name="handcraft-requirementstxt"></a>Manuelles Erstellen von „requirements.txt“

Einige Entwickler verwenden `pip freeze > requirements.txt`, um die Liste der Python-Pakete für ihre Entwicklungsumgebungen zu generieren. Wenngleich diese Vorgehensweise in den meisten Fällen funktionieren sollte, kann sie in plattformübergreifenden Bereitstellungsszenarien zu Problemen führen – wenn beispielsweise Funktionen lokal unter Windows oder macOS entwickelt werden, aber in einer Funktions-App veröffentlicht werden, die unter Linux ausgeführt wird. In diesem Szenario kann `pip freeze` zu unerwarteten betriebssystemspezifischen Abhängigkeiten oder zu Abhängigkeiten für Ihre lokale Entwicklungsumgebung führen. Diese Abhängigkeiten können die Lauffähigkeit der Python-Funktions-App bei Ausführung unter Linux beeinträchtigen.

Die beste Vorgehensweise besteht darin, die Importanweisung aus jeder PY-Datei im Quellcode Ihres Projekts zu überprüfen und nur die Module in der Datei „requirements.txt“ einzuchecken. Auf diese Weise wird garantiert, dass die Paketauflösung auf unterschiedlichen Betriebssystemen ordnungsgemäß ausgeführt werden kann.

### <a name="replace-the-package-with-equivalents"></a>Ersetzen der Pakete durch äquivalente Pakete

Zunächst sollten wir einen Blick auf die neueste Version des Pakets in `https://pypi.org/project/<package-name>` werfen. Normalerweise verfügt dieses Paket über eine eigene GitHub-Seite. Wechseln Sie zum Abschnitt **Issues** auf GitHub und überprüfen Sie, ob Ihr Problem behoben wurde. Falls ja, aktualisieren Sie das Paket auf neueste Version.

In einigen Fällen wurde das Paket möglicherweise in die [Python-Standardbibliothek](https://docs.python.org/3/library/) (z. B. „pathlib“) integriert. Da wir eine bestimmte Python-Distribution in Azure Functions (Python 3.6, Python 3.7 und Python 3.8) zur Verfügung stellen, muss das Paket ggf. aus „requirements.txt“ entfernt werden.

Wenn jedoch ein Problem vorliegt, das noch nicht behoben wurde, und Sie einen Termin einhalten müssen, sollten Sie etwas recherchieren, um ein ähnliches Paket für Ihr Projekt zu finden. In der Regel stellt die Python-Community eine Vielzahl ähnlicher Bibliotheken zur Verfügung, die Sie nutzen können.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie Ihr modulbezogenes Problem nicht lösen können, melden Sie den Fehler beim Functions-Team:

> [!div class="nextstepaction"]
> [Ungelöstes Problem melden](https://github.com/Azure/azure-functions-python-worker/issues)
