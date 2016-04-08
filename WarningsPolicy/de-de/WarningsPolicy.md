#Build Warnings Policy
Viele Software-Projekte, vor allem ältere, die über eine längere Zeit gewachsen sind, häufen hunderte oder tausende Build-
Warnungen an. Diese Warnungen durch Refactoring oder aufräumen des Codes loszuwerden, ist oft schwierig, da neue Warnungen
in den bestehenden untergehen. In diesem Fall ist es auch nicht hilfreich, Warnungen grundsätzlich wie Fehler zu behandeln
(*treat warnings as errors*), denn dies zwingt Teams entweder dazu, die Warnings sofort zu eliminieren oder über einen langen
Zeitraum mit fehlschlagenden Builds zu leben. 

Die *Build Warnings Policy* hilft, den Überblick über Warnungen zu behalten und sie über die Zeit zu reduzieren. Dies wird
erreicht, indem der Build gebrochen wird, wenn die Zahl der Warnungen einen bestimmten Wert übersteigt oder sich von einem
Build zum nächsten erhöht.

###Hinzufügen der Policy zu einer Build-Definition
Die *Build Warnings Policy* muss nach den Tasks eingefügt werden, die von ihr überwacht werden sollen. In einer Visual Studio
Build-Definition bietet es sich z.B. an, die Policy hinter den Build-, Test- und Symbol-Indexing/Publishing-Tasks einzufügen.
Dadurch wird sichergestellt, dass die Testergebnisse und die Kompilate/Symbole auch dann zur Verfügung stehen, wenn der Build
durch die Policy abgebrochen wird. 

![Policy-Einbindung](../assets/AddPolicy.png "Empfohlene Einbindung der Build Warnings Policy")

###Parameter der Build Warnings Policy
- **Abbruchkriterium:** Setzen Sie diese Option auf `Fixed Threshold`, um den Build abzubrechen, wenn eine bestimmte Zahl an
Warnungen überschritten wird. Dies ist dann hilfreich, wenn Sie eine geringe Zahl an Warnungen zulassen aber gleichzeitig
sicherstellen möchten, dass die Warnungen nicht Überhand nehmen. Um die Zahl der Warnungen über einen längeren Zeitraum
kontinuierlich zu reduzieren, wählen Sie die Option `Previous Build`. Dadurch wird der Build abgebrochen, wenn die Zahl der
Warnungen seit dem letzten Build gestiegen ist.

- **Schwellwert:** Geben sie die maximale Anzahl an Warnungen an, die nicht überschritten werden darf. Dieser Parameter ist
nur sichtbar, wenn als *Abbruchkriterium* die Option `Fixed Threshold` gewählt wurde.

- **Nur erfolgreiche Builds:** Aktivieren Sie diese Option, um den aktuellen Build immer mit dem letzten erfolgreichen Build
zu vergleichen. Wenn Sie diese Option deaktivieren, vergleicht die Policy den aktuellen Build mit dem vorherigen Build,
unabhängig von dessen Ergebnis. Die Option sollte immer gesetzt sein, da ein Vergleich mit fehlgeschlagenen Builds zu einer
stetig steigenden Zahl von Warnungen führen kann (Bsp.: Build 1 wird mit 10 Warnungen erfolgreich beendet, Build 2 schlägt mit
15 Warnungen fehlt, Build 3 wird mit 15 Warnungen erfolgreich beendet). Dieser Parameter ist nur sichtbar, wenn als
*Abbruchkriterium* die Option `Previous Build` gewählt wurde.

- **Erzwinge weniger Warnungen:** Aktivieren Sie diese Option, wenn der aktuelle Build immer weniger Warnungen als der vorherige
Build haben soll.

####Erweitert
- **Task Filter:** Da das Build-System verschiedenste Tasks ausführen und jeder dieser Tasks Warnungen erzeugen kann, muss die
*Build Warnings Policy* wissen, welche Tasks berücksichtigt und welche ignoriert werden sollen. *Task Filter* enthält eine Liste
von regulären Ausdrücken (einer pro Zeile). Die Policy berücksichtigt nur Tasks, auf die einer der Filter passt. Die Filter werden
auf den Namen der Timeline eines Tasks angewendet; dieser wird in der Build-Definition unterhalb des Task-Namens angezeigt. Der
Standardwert `/(build|ant|maven)/i` berücksichtig bereits die meisten der Standard-Build-Tasks in TFS/VSTS.
Klicken Sie [hier](http://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions), um mehr über reguläre Ausdrücke
zu erfahren.