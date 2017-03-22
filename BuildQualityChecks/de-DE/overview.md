# Build Quality Checks
Der *Build Quality Checks* Task ermöglicht Ihnen, Qualitätsprüfungen in den Build-Prozess einzufügen.

### Change Notes
Sie finden die Change Notes für diesen Task [hier](https://github.com/almtcger/VstsExtensions/blob/master/BuildQualityChecks/de-DE/changeNotes.md).

### Bekannte Probleme
- Wenn sich Ihr Build-Agent hinter einem Proxy-Server befindet, wird der Task fehlschlagen, selbst wenn der Proxy-Server korrekt
  wie [hier](https://github.com/Microsoft/vsts-agent/blob/master/docs/start/proxyconfig.md) beschrieben im Agenten konfiguriert wurde.
  Im Task-Log ist dann der Eintrag `connect ETIMEDOUT` zu finden. Um das Problem zu beheben, erzeugen Sie zwei Umgebungsvariablen namens
  `HTTP_PROXY` und `HTTPS_PROXY` (entweder für das gesamte System oder für den Benutzer, der den Agent-Service ausführt) und setzen Sie
  deren Wert auf die Proxy-Server-URL inkl. Port (z.B. http://meinProxy:8080).  
  **Hinweis:** Build-Tasks unterstützen derzeit keine Proxy-Authentifizierung.
- Der Ergebnisbereich auf der Build-Zusammenfassungsseite zeigt ein falsches Icon (Mülltonne statt rotes X) für fehlgeschlagenen Regeln bei
  Team Foundation Server 2015 an. Wir werden dieses Problem nicht mehr beheben, da es weder bei Team Foundation Server 2017 noch bei Visual
  Studio Team Services auftritt.

### Support
Falls Sie Hilfe zu dieser Erweiterung benötigen oder Probleme auftreten, kontaktieren Sie uns bitte unter <a href='&#109;&#97;&#105;&#108;&#116;&#111;&#58;&#112;&#115;&#103;&#101;&#114;&#101;&#120;&#116;&#115;&#117;&#112;&#112;&#111;&#114;&#116;&#64;&#109;&#105;&#99;&#114;&#111;&#115;&#111;&#102;&#116;&#46;&#99;&#111;&#109;'>&#112;&#115;&#103;&#101;&#114;&#101;&#120;&#116;&#115;&#117;&#112;&#112;&#111;&#114;&#116;&#64;&#109;&#105;&#99;&#114;&#111;&#115;&#111;&#102;&#116;&#46;&#99;&#111;&#109;</a>.

### Hinzufügen des Tasks zu einer Build-Definition
Der *Build Quality Checks* Task (in der Task-Kategorie *Build*) muss nach den Tasks eingefügt werden, die von ihm überwacht werden
sollen. In einer Visual Studio Build-Definition bietet es sich z.B. an, den Task hinter den Build-, Test- und Symbol-Indexing/Publishing-Tasks
einzufügen. Dadurch wird sichergestellt, dass die Testergebnisse und die Kompilate/Symbole auch dann zur Verfügung stehen, wenn der Build
durch eine der Regeln abgebrochen wird. 

![Task-Einbindung](../assets/AddTask.png "Empfohlene Einbindung des Build Quality Checks Tasks")

### Regeln
Der *Build Quality Checks* Task unterstützt derzeit zwei Regeln (klicken Sie die Links für mehr Informationen):

- **[Warnungen-Regel](https://github.com/almtcger/VstsExtensions/blob/master/BuildQualityChecks/de-DE/WarningsPolicy.md)** - Erlaubt
  es, einen Build auf Basis der Anzahl der Build-Warnungen abzubrechen.
- **[Code-Coverage-Regel](https://github.com/almtcger/VstsExtensions/blob/master/BuildQualityChecks/de-DE/CodeCoveragePolicy.md)** -
  Erlaubt es, einen Build auf Basis der Code Coverage der im Build enthaltenen Tests abzubrechen.

### Task-Parameter

#### Baseline
Wenn Sie als *Abbruchkriterium* einer Regel die Option `Vorheriger Wert` wählen, wird der Regel-Wert (z.B. Anzahl der Warnungen)
standardmäßig mit dem entsprechenden Wert des letzten Builds der aktuellen Build-Definition verglichen. Falls die Build-Definition
ein Git-Repository in Visual Studio Team Services oder Team Foundation Server als Quelle nutzt, wird immer der letzte Build zum Vergleich
herangezogen, der denselben Branch wie der aktuelle Build betrifft. Dieses Verhalten kann durch die folgenden Parameter angepasst werden:

![Baseline-Parameter](../assets/Baseline.png "Festlegung von Baseline-Build-Definition und -Branch")

- **Build-Definition:** Wählen Sie die Build-Definition, über die nach einem Basis-Build gesucht werden soll. Wenn Sie keinen Wert
  angeben, wird automatisch der letzte Build der aktuellen Build-Definition zum Vergleich von Regel-Werten herangezogen. Sollte die
  Drop-down-Liste leer sein, klicken Sie bitte auf das Refresh-Icon, um die Liste der verfügbaren Build-Definitionen neu zu laden.
  Lesen Sie [TFVC Topic Branches](https://github.com/almtcger/VstsExtensions/blob/master/BuildQualityChecks/de-DE/PullRequests.md#tfvc-topic-branches)
  für Beispiele, wann die Wahl einer anderen Build-Definition sinnvoll sein kann.

  **Hinweis:** Um die Regel-Werte immer gegen einen Build der aktuellen Build-Definition zu vergleichen, der einen anderen als den
  aktuellen Branch betrifft, müssen Sie hier die aktuelle Build-Definition sowie die passenden Werte für *Repository* und *Branch (Git)*
  auswählen.

- **Repository:** Wählen Sie das Repository aus, in dem nach möglichen Basis-Branches gesucht werden soll. Die Liste wird gefüllt, nachdem
  Sie die *Build-Definition* gewählt haben. Sie enthält immer nur das Repository, das mit der gewählten Build-Definition verknüpft ist.
  Sollte die Drop-down-Liste leer sein, nachdem Sie die *Build-Definition* ausgewählt haben, klicken Sie bitte auf das Refresh-Icon, um die
  Repository-Information neu zu laden.

  **Hinweis:** Wenn Sie die *Build-Definition* verändern, nachdem Sie das Repository ausgewählt haben, wird manchmal eine GUID im Feld
  *Repository* angezeigt. Es handelt sich hierbei um eine Refresh-Einschränkung der Build-UI. Bitte wählen Sie in diesem Fall das Repository
  erneut aus, um den Wert zu korrigieren.

- **Branch (Git):** Wählen Sie den Branch aus, der für die Suche nach einem Basis-Build verwendet wird. Wenn Sie keinne Wert angeben, wird
  automatisch der letzte Build gesucht, der denselben Branch wie der aktuelle Build betrifft. Sollte die Drop-down-Liste leer sein, nachdem
  Sie das *Repository* ausgewählt haben, klicken Sie bitte das Refresh-Icon, um die verfügbaren Branches neu zu laden. Branches werden mit
  ihrem Git-Ref-Namen angezeigt, z.B. ref/heads/master oder refs/heads/meinTopicBranch. Lesen Sie
  [Pull Request Policy Builds](https://github.com/almtcger/VstsExtensions/blob/master/BuildQualityChecks/de-DE/PullRequests.md#pull-request-policy-builds)
  für Beispiele, wann die Wahl eines anderen Branches sinnvoll sein kann.

  **Hinweis:** Wenn Sie die *Build-Definition* und das *Repository* verändern, nachdem Sie einen Branch ausgewählt haben, müssen Sie den Branch
  erneut auswählen. Anderenfalls könnte ein falscher Wert in der Task-Konfiguration gespeichert werden. Dies ist eine Refresh-Einschränkung der
  Build-UI.

  **Hinweis:** Wann immer Sie einen anderen Branch wählen, stellen Sie sicher, dass die [Retention Policy](https://www.visualstudio.com/de-de/docs/build/concepts/policies/retention)
  so konfiguriert ist, dass mindestens ein erfolgreicher Build für diesen Branch aufbewahrt wird!

### Typische Anwendungsbeispiele

- [Pull Requests und TFVC Topic Branches](https://github.com/almtcger/VstsExtensions/blob/master/BuildQualityChecks/de-DE/PullRequests.md)

### Regel-Ergebnisse
Der *Build Quality Checks* Task erzeugt seine eigene Zusammenfassung auf der Build-Ergebnisseite. Diese Zusammenfassung listet
alle Erfolgsmeldungen, Warnungen und Fehler aller aktiven Regeln auf. Wenn Sie einen Multi-Konfigurations-Build ausführen, wird
für jeden Build-Job ein eigener Unterbereich angezeigt, so dass Sie genau sehen können, welche Konfiguration evtl. Qualitätsprobleme
aufweist.

![Policy Result](../assets/PolicyResult.png "Zusammenfassung der Ergebnisse des Build Quality Checks Tasks")

**Hinweis:** Bitte beachten Sie den Abschnitt [Limitierungen und Spezialfälle](https://github.com/almtcger/VstsExtensions/blob/master/BuildQualityChecks/de-DE/CodeCoveragePolicy.md)
der *Code-Coverage-Regel* bzgl. möglicher Probleme bei der Nutzung von Code Coverage in Verbindung mit Multi-Konfigurations-Builds.

[Checklist board icon](https://www.vexels.com/vectors/png-svg/129767/checklist-board-icon) | Icon designed by Vexels.com
