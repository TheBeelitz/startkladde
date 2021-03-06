# Build unter Windows

Für den Build wird die Installation des QT Creator mit einem aktuellen 64-Bit MinGW-Compiler empfohlen. 
Auf der [Open-Source-Seite](https://www.qt.io/download-open-source) von Qt
wird ein Online-Installer angeboten, in dem man komfortabel auswählen kann, 
welche Komponenten installiert werden sollen.

## Qt Creator

Beim ersten Öffnen des Projekts im Qt Creator wird eine Standard-Build-Konfiguration erstellt, die an sich gut funktioniert.Lediglich der 
Wert _Parallele Jobs_ in der Make-Konfiguration sollte auf 1 gestellt werden, da sonst unvollständig compiliert wird.
Außerdem kann man im Release-Profil den Haken bei _Qt Quick Compiler verwenden_ in der qmake-Konfiguration entfernen.

## MariaDB

Es werden mindestens die MariaDB-Client-Libaries benötigt.
Ist ohnehin schon ein MariaDB-Server installiert, sind diese im Programmverzeichnis mitgeliefert.
Andernfalls kann man auch den MariaDB C/C++ Connector für 64-Bit-Systeme installieren.
Anschließend muss in der `startkladde.pro` der Pfad `WIN32_MARIADB_INSTALLDIR` so gesetzt werden,
dass er auf das Installationsverzeichnis von MariaDB bzw. dem Connector zeigt. Die Ordner `lib` und `include`
müssen Unterverzeichnisse des betreffenden Ordners sein. Ein typischer Pfad ist `C:\Program Files\MariaDB 10.4`.

## Qt-MySQL-Plugin

Leider muss das MySQL-Treiberplugin der Qt-Bibliothek manuell übersetzt werden. Dazu benötigt man den Quellcode von Qt, der aber bequem über den Online-Installer heruntergeladen werden kann. Im Startmenü findet man einen Link zu einer für die Qt-Entwicklung vorkonfigurierten Eingabeaufforderung. In dieser springt man in das Verzeichnis `${QT_SRCDIR}\qtbase\src\plugins\sqldrivers\mysql`. Aus irgendeinem Grund muss man zunächst in der Datei `mysql.pro` die Zeile `QMAKE_USE += mysql` auskommentieren. Dann braucht man noch die Pfade zur MariaDB-Library, dazu ist es am einfachsten, den kompletten Block, der die MariaDB-Pfade setzt aus der `startkladde.pro` in die `mysql.pro` zu kopieren. Anschließend `qmake` aufrufen und dann `mingw32-make` zum compilieren. Anschließend noch `mingw32-make install`. Zu guter letzt muss man meist noch die Datei `libmariadb.dll` aus dem Ordner der MariaDB-Library in das Verzeichnis `${QT_DIR}/bin` kopieren.


## Ruby

Der Buildprozess benötigt das Konsolenprogramm `erb`. 
Indem Ruby installiert wird, sollte dieses auf der Konsole verfügbar werden.

## Warum ist das alles so kompliziert?

Windows ist kein Betriebssystem sondern ein Krampf. Deshalb.
