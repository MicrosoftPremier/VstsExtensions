# Build Quality Checks - Change Notes

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