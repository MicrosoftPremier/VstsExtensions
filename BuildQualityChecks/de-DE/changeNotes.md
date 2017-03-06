[Zurück zur Übersicht](./overview.md)

# Build Quality Checks - Change Notes

#### 1.5.1
- Anpassung der Dokumentation, damit diese im VSTS Marketplace korrekt angezeigt wird.

#### 1.5.0
- Unterstützung für Multi-Konfigurations-Builds hinzugefügt.

#### 1.4.2
- Zusätzliche Modul-Filter-Prüfungen hinzugefügt.

#### 1.4.1
- Leichte Veränderung der Berechnung der Code Coverage.

#### 1.4.0
- Die Code-Coverage-Regel unterstützt nun verschiedene Code-Coverage-Arten (Lines, Blocks, Branches).
- Eine Race Condition im Zusammenspiel mit Task-Gruppen wurde behoben.
- Die Modul-Filter der Code-Coverage-Regel sind nun deprecated. Die Filter waren eher verwirrend als hilfreich und werden mit
  der nächsten Major-Version vollständig entfernt. Bitte nutzen Sie run settings (oder entsprechende Einstellungen Ihres Test-
  und Code-Coverage-Tools) zum Filtern der Code Coverage.
- Der Default-Wert für Modul-Filter wurde entfernt.

#### 1.3.0
- Eine Race Condition aufgrund des asynchronen Uploads von Build-Informationen wurde behoben.
- Die Code-Coverage-Regel funktioniert nun korrekt bei Builds mit mehreren Tasks zur Code-Coverage-Berechnung (z.B. mehrere VSTest-Tasks).
- Es wurden weitere Informationen über die Tasks und Module, die von den Policies geprüft werden, zum Task-Log hinzugefügt.

#### 1.2.2
- Ein Fehler in der Code-Coverage-Regel wurde behoben, der zur Ausgabe **Cannot read property 'percentage' of undefined** führte.

#### 1.2.1
- Der Task funktioniert nun auch ohne aktivierten Zugriff auf das OAuth-Token.

#### 1.1.5
- Erste öffentliche Version.