# WasserAlarm
Das Modul dient dazu einen unnatürlich hohen Wasserverbrauch festzustellen. Es reagiert auf eine Zählervariable und schaltet unter bestimmten Bedingungen einen Alarm.
Es gibt zwei Alarmvariablen.  
Einen Rohrbruch-Alarm, welcher schaltet wenn große Mengen auf einmal fließen.
Einen Leckage-Alarm, welcher in 7 Stufen hochtickt wenn über längeren Zeitraum eine kleine Menge fließt (z.B. tropfender Wasserhahn).
Das Intervall für beide Kontrollen kann über die Konfiguration eingestellt werden.

### Inhaltsverzeichnis

1. [Funktionsumfang](#1-funktionsumfang)
2. [Voraussetzungen](#2-voraussetzungen)
3. [Software-Installation](#3-software-installation)
4. [Einrichten der Instanzen in IP-Symcon](#4-einrichten-der-instanzen-in-ip-symcon)
5. [Statusvariablen und Profile](#5-statusvariablen-und-profile)
6. [WebFront](#6-webfront)
7. [PHP-Befehlsreferenz](#7-php-befehlsreferenz)

### 1. Funktionsumfang

* Auswahl der Wasserzählervariable
* Rohrburch Timer in Minuten einstellbar
* Leckage Timer in Minuten einstellbar
* 7 Stufen Anzeige für Leckage
* Alarmanzeige bei Rohrbruch

### 2. Voraussetzungen

- IP-Symcon ab Version 4.2

### 3. Software-Installation

* Über den Module Store das Modul Wasser-Alarm installieren.
* Alternativ über das Module Control folgende URL hinzufügen:
`https://github.com/symcon/WasserAlarm`

### 4. Einrichten der Instanzen in IP-Symcon

- Unter "Instanz hinzufügen" kann das 'WasserAlarm'-Modul mithilfe des Schnellfilters gefunden werden.
    - Weitere Informationen zum Hinzufügen von Instanzen in der [Dokumentation der Instanzen](https://www.symcon.de/service/dokumentation/konzepte/instanzen/#Instanz_hinzufügen)

__Konfigurationsseite__:

Name                | Beschreibung
------------------- | ---------------------------------
Zählervariable      | Variable, welche den Zählerwert wiedergibt.
Leckage Intervall   | __Standard: 1min__ Zeitintervall in dem kontrolliert wird, ob zuviel Wasser durchgeflossen ist.
Rohrbruch Intervall | __Standard: 15min__ Zeitintervall in dem kontrolliert wird, ob zuviel Wasser durchgeflossen ist.

### 5. Statusvariablen und Profile

Die Statusvariablen/Kategorien werden automatisch angelegt. Das Löschen einzelner kann zu Fehlfunktionen führen.

##### Statusvariablen

Name                | Typ       | Beschreibung
------------------- | --------- | ----------------
Leckage Grenzwert   | Float     | Differenzwert, welcher im eingestellten "Leckage Intervall" überprüft wird und bei Überschreitung wird die "Leckage" um 1 erhöht. Wenn die Differenz unterhalb oder gleich der Grenze ist, wird das Alarmlevel resetet. 
Leckage             | Integer   | 7 Stufenanzeige für den Stand den Alarmlevels.
Rohrbruch Grenzwert | Float     | Grenzwert, welcher geprüft wird und gegebenfalls der Auslöser für einen Alarm ist. 
Rohrbruch           | Boolean   | Alarm ob der Durchfluss zu hoch ist. 

##### Profile:

Bezeichnung        | Beschreibung
------------------ | -----------------
WAA.LeakLevel      | Profil für Leckage - 7 Alarmstufen mit verschiedenen Symbolen und Farbanzeigen
WAA.ThresholdValue | Profil für Leckage/Rohrbruch Grenzwert

### 6. WebFront

Über das WebFront können die Grenzwerte eingestellt werden.  
Es wird zusätzlich angezeigt, ob ein Alarm vorliegt oder nicht.

### 7. PHP-Befehlsreferenz

`boolean WAA_CheckAlert(integer $InstanzID, string $BorderValue, string $OldValue);`
Kontrolliert innerhalb des WasserAlarms mit der InstanzID $InstanzID ob ein Grenzwert überschritten wurde und setzt die Alarmvariablen  
Die Funktion liefert keinerlei Rückgabewert.  
`WAA_CheckAlert(12345, "LeakThreashold", "LeakBuffer");`
