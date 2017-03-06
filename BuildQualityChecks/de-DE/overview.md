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
Der *Build Quality Checks* Task muss nach den Tasks eingefügt werden, die von ihm überwacht werden sollen. In einer Visual Studio
Build-Definition bietet es sich z.B. an, den Task hinter den Build-, Test- und Symbol-Indexing/Publishing-Tasks einzufügen.
Dadurch wird sichergestellt, dass die Testergebnisse und die Kompilate/Symbole auch dann zur Verfügung stehen, wenn der Build
durch eine der Regeln abgebrochen wird. 

![Task-Einbindung](../assets/AddTask.png "Empfohlene Einbindung des Build Quality Checks Tasks")

### Regeln
Der *Build Quality Checks* Task unterstützt derzeit zwei Regeln (klicken Sie die Links für mehr Informationen):

- **[Warnungen-Regel](https://github.com/almtcger/VstsExtensions/blob/master/BuildQualityChecks/de-DE/WarningsPolicy.md)** - Erlaubt
  es, einen Build auf Basis der Anzahl der Build-Warnungen abzubrechen.
- **[Code-Coverage-Regel](https://github.com/almtcger/VstsExtensions/blob/master/BuildQualityChecks/de-DE/CodeCoveragePolicy.md)** -
  Erlaubt es, einen Build auf Basis der Code Coverage der im Build enthaltenen Tests abzubrechen.

### Typische Anwendungsbeispiele
*In Kürze verfügbar*

### Regel-Ergebnisse
Der *Build Quality Checks* Task erzeugt seine eigene Zusammenfassung auf der Build-Ergebnisseite. Diese Zusammenfassung listet
alle Erfolgsmeldungen, Warnungen und Fehler aller aktiven Regeln auf. Wenn Sie einen Multi-Konfigurations-Build ausführen, wird
für jeden Build-Job ein eigener Unterbereich angezeigt, so dass Sie genau sehen können, welche Konfiguration evtl. Qualitätsprobleme
aufweist.

![Policy Result](../assets/PolicyResult.png "Zusammenfassung der Ergebnisse des Build Quality Checks Tasks")

**Hinweis:** Bitte beachten Sie den Abschnitt [Limitierungen und Spezialfälle](https://github.com/almtcger/VstsExtensions/blob/master/BuildQualityChecks/de-DE/CodeCoveragePolicy.md)
der *Code-Coverage-Regel* bzgl. möglicher Probleme bei der Nutzung von Code Coverage in Verbindung mit Multi-Konfigurations-Builds.

[Checklist board icon](https://www.vexels.com/vectors/png-svg/129767/checklist-board-icon) | Icon designed by Vexels.com
