[Zurück zur Übersicht](./overview.md)

# Warnungen-Regel
Viele Software-Projekte, vor allem ältere, die über eine längere Zeit gewachsen sind, häufen hunderte oder tausende Build-
Warnungen an. Diese Warnungen durch Refactoring oder aufräumen des Codes loszuwerden, ist oft schwierig, da neue Warnungen
in den bestehenden untergehen. In diesem Fall ist es auch nicht hilfreich, Warnungen grundsätzlich wie Fehler zu behandeln
(*treat warnings as errors*), denn dies zwingt Teams entweder dazu, die Warnungen sofort zu eliminieren oder über einen langen
Zeitraum mit fehlschlagenden Builds zu leben. 

Die *Warnungen-Regel* hilft, den Überblick über Warnungen zu behalten und sie über die Zeit zu reduzieren. Dies wird
erreicht, indem der Build gebrochen wird, wenn die Zahl der Warnungen einen bestimmten Wert übersteigt oder sich von einem
Build zum nächsten erhöht.

### Limitierungen und Spezialfälle
- **Warnungen müssen Build Issues sein**  
  Die *Warnungen-Regel* zählt nur Warnungen, die als Build Issues geloggt wurden. Wenn ein Task Warnungen in seine Ausgabe
  schreibt, diese aber dem Build-System nicht als Issues bekanntmacht, erkennt die Regel diese nicht.

### Parameter der Warnungen-Regel

![Warnungen-Regel](../assets/WarningsPolicy.png "Parameter der Warnungen-Regel")

- **Aktiv:** Über diese Option lässt sich die Regel ein- und ausschalten. Wenn die Regel ausgeschaltet ist, sind die weiteren
  Parameter nicht sichtbar.

- **Abbruchkriterium:** Setzen Sie diese Option auf `Fester Schwellwert`, um den Build abzubrechen, wenn eine bestimmte Zahl an
  Warnungen überschritten wird. Dies ist dann hilfreich, wenn Sie eine geringe Zahl an Warnungen zulassen aber gleichzeitig
  sicherstellen möchten, dass die Warnungen nicht Überhand nehmen oder wenn Sie eine "No Warnings Policy" durchsetzen möchten
  (*Schwellwert* = 0). Um die Zahl der Warnungen über einen längeren Zeitraum kontinuierlich zu reduzieren, wählen Sie die Option
  `Vorheriger Wert`. Dadurch wird der Build abgebrochen, wenn die Zahl der Warnungen seit dem vorherigen Build gestiegen ist.

- **Schwellwert:** Geben sie die maximale Anzahl an Warnungen an, die nicht überschritten werden darf. Dieser Parameter ist
  nur sichtbar, wenn als *Abbruchkriterium* die Option `Fester Schwellwert` gewählt wurde.

- **Weniger Warnungen erzwingen:** Aktivieren Sie diese Option, wenn der aktuelle Build immer weniger Warnungen als der vorherige
  Build haben soll. Diese Option ist nur sichtbar, wenn als *Abbruchkriterium* die Option `Vorheriger Wert` gewählt wurde.

- **Task-Filter:** Da das Build-System verschiedenste Tasks ausführen und jeder dieser Tasks Warnungen erzeugen kann, muss die
  *Warnungen-Regel* wissen, welche Tasks berücksichtigt und welche ignoriert werden sollen. *Task-Filter* enthält eine Liste
  von regulären Ausdrücken (einer pro Zeile). Die Regel berücksichtigt nur Tasks, auf die einer der Filter passt. Die Filter werden
  auf den Namen der Timeline eines Tasks angewendet; dieser wird in der Build-Definition unterhalb des Task-Namens angezeigt. Der
  Standardwert `/^(((android|xcode|gradlew)\\s+)?build|ant|maven|cmake|gulp)/i` berücksichtig bereits die meisten der Standard-Build-Tasks
  in Team Foundation Server/Visual Studio Team Services.

  **Hinweis:** Reguläre Ausdrücke müssen in der JavaScript RegExp-Syntax angegeben werden. Klicken Sie [hier][JSRegExp], um mehr über
  reguläre Ausdrücke zu erfahren.
  
  [JSRegExp]: http://www.regular-expressions.info/javascript.html