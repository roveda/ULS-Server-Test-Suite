Version 1.9.6

(Stand 2021-02-09)

-- User-Interface
- Das User-Interface ist nicht mehr über die Ports 11975 und 11976 erreichbar,
  sondern über die Ports 80 und 443.
- Es werden keine Framesets mehr verwendet
- Die Anmeldung läuft nicht mehr über "Basic Auth"
- Es werden User-Sessions genutzt.
- Deshalb kann es während des Updates zu einer kurzfristigen nicht Erreichbarkeit
  der Seite kommen. Außerdem muss man sich nach dem Update neu anmelden.


-- Neues Anmldefenster mit Sessions.
- Nach erfolgreicher Anmledung werden fehlerhafte Anmeldeversuchen angezeigt.
- Für Kennwörter können alle Zeichen verwendet werden.


-- IPV6
- Es werden IP-V6 Adressen unterstützt


-- UTT
- Zu Tickets können Wiedervorlagen eingerichtet werden.
- In der Übersicht können Tickets mit Error- / Warn-Level farblich markiert werden. Kann
  über Option konfiguriert werden.


-- Fileformat
- Kennung 'O' hinzugefügt für Overwrite. Das Überschreiben muss beim
  Access-Rechte erlaubt sein.
- Kennung 'W' hinzugefügt für Serverdoku als Wert übergeben:
  "W;<timestamp>;<server>;<name>;<description>;<retention>;<Filecontent>;<filename>;<characterset>"


-- Wildcards
- Weil das Quoting sehr verwirrend war, doch ein Quoting mit '\' - Sorry an Windows
  Bei den ULS-Suchmustern können nun folgende Sequenzen verwendet werden: \, \* \? \{ \} \\


-- Alle Limit/isAlive Formulare
- in der Zeile für den neuen Eintrag ist die Checkbox 'Aktiv' gesetzt.


-- Limits
- Neue Funktion "All", die nur dann erfüllt ist, wenn alle Werte die Bedingung erfüllen.
  Wenn z. B. erst alarmiert werden soll, wenn innerhalb der letezten 30 Minuten alle Werte
  ungleich OK sind, dann Funktion "All", Argument "30m", Vergleich "!=" und Info, Warn oder Fehler "OK".
  "All" ist nur bei Zeichenketten möglich. Das Feld Exclude hat keine Funktion.
- Neue Funktion "Prediction": Das Argument wird als Zeitspanne in Stunden in die Zukunft
  interpretiert und das Ergebnis ist der erwartete Wert bei einem linearen Verlauf unter
  Berücksichtigung der letzten 24 Stunden. Als zweiter Parameter kann angegeben werden,
  wie viele Stunden in der Vergangenheit berücksichtigt werden. Die Zeiträume können auch
  als Tage angegeben werden: "3d". Beispiele für das Argument:
  - 3      : Erwarteter Wert in 3 Stunden, 24 Stunden Berücksichtigung
  - 2,30   : Erwarteter Wert in 2 Stunden, 30 Stunden Berücksichtigung
  - 2d,3d  : Erwarteter Wert in 48 Stunden, 72 Stunden Berücksichtigung
- Links zu Tickets enthalten bei Limits die Limit-ID, um bei der tabellarischen Ausgabe
  die Werte einzufärben.
- Neue Funktion in den Meldungen: @SET_SERVERNAME(<servername>). Es wird der <servername> an
  das Skript zum Generieren des Tickets übergeben, um ggf. nach dem Servernamen zu routen.
  Ergibt Sinn bei z. B. Datensicherungen, die gleich dem Dasi-Client zugeordnet werden sollen.
- einzelne Wertelimits, Autolimits und Gruppen-Autolimits können durch Klicken auf "Erweitern"
  in einem separaten Fenster mit sehr viel größeren Eingabefeldern editiert werden.


-- isAlive
- Bei den "Ausgelösten Benachrichtigungen" können isAlives als "erledigt" gekennzeichnet
  werden. Das hat zur Folge, dass der Status auf "grün" geht und es wird erst erneut
  alarmiert, wenn wieder Werte eingelaufen sind.


-- Kombilimits
- es wird das Ergebnis der Berechnung angezeigt
- Kombilimits für aggregierte Werte


-- Kombidetails
- neue Funktion $ACTUAL_OF(<details>) liefert den aktuellen Wert. $VALUE_OF liefert immer
  den Wert passend zum Zeitstempel des Referenzwertes. Bei Kombilimits besteht zwischen
  $ACTUAL_OF und $VALUE_OF kein Unterschied.


-- Grouped Notifications
- Bei der Meldung zu gruppierten Überwachungen kann auf die einzelnen ausgelösten Limits
  zugegriffen werden über: @[<Text, der für jedes ausgelöste Limit ausgegeben wird>].
  Es können zusätzlich die Variablen $LIMITTYPE, $SERVER, $SECTION, $TESTSTEP, $DETAIL,
  $LIMITLEVEL, $ULSTABLELINK und $ULSCHARTLINK genutzt werden.


-- Verfahren / Server / Section / Teststep Filter
- Es kann konfiguriert werden, dass der Filter nur ab einer vorgegebenen Anzahl
  Einträgen aktiviert wird.


-- Komprimierung
- Daten werden nicht erst komprimiert, wenn Daten des nächsten Tages vorliegen, sondern
  bei stündlicher und täglicher Komprimierung spätestens am übernächsten Tag, bei
  wöchentlicher Komprimierung nach einer Woche und bei monatlicher Komprimierung nach
  einem Monat.
- Wenn die Komprimierung aktiviert wird, werden die dazugehörigen Daten erst gelöscht,
  wenn die Komprimierung gelaufen ist.


-- Überwachungspausen
- Überwachungspausen für Gruppen
  Die Überwachungspausen für Gruppen gelten nur für die Gruppenlimits. So kann erreicht werden,
  dass z. B. Testserver nicht nachts alarmieren, indem für die Testserver eine
  Gruppen-Überwachungspause während der Nachtstunden eingerichtet wird.


-- Passwort Hashfunktion
- Alte Passworthaschfunktion entfern. User, die sich seit dem letztem Update nicht angemeldet
  haben, müssen ihr Kennwort zurücksetzen lassen.
- Zum Haschen des Kennwortes wird nicht mehr der übergebene Username sonder der nach
  Kleinbuchstaben konvertierte Username verwendet. Wenn User sich bisher mit Großbuchstaben
  im Usernamen angemeldet haben, dann muss das Kennwort für diese User zurückgesetzt werden.


-- Optionen
- Wenn als Startseite News gewählt ist, dann werden nun alle News angezeigt. Der zusätzliche
  leere Eintrag zeigt nur neue News an.
- Bei der Startseite, Serverseite, Sektion Seite und den Menüs werden ausgeblendete
  Gruppenreports nicht mehr angezeigt. Es sei denn, sie sind bereits ausgewählt.


-- Accessrechte
- Attribut "allow overwrite" hinzugefügt (Siehe Fileformat).


-- Admin Menüs
- Markierte Verfahren werden oben angezeigt.


-- Auto-Server
- Man kann Verfahren IP-Adressranges zuweisen, aus denen sich Server selbst anlegen können.


-- Grafana
- Über "JSON over HTTP datasource" kann Grafana an ULS angebunden werden.
  Die URL ist: https://<ulsserver>:11976/grafana/ (http://<ulsserver>:11975/grafana/),
  mit "Basic auth", User und Password eines zugehörigen Servicekontos.
- Es können Variablen definiert werden, um die Ausgabe zu ändern (nur bei Tables):
    Name         Query         Regex
  - Servers      /*            /^([^.]*)/
  - Sections     /*.*          /^[^.]*\.([^.]*)/
  - Teststeps    /*.*.*        /^[^.]*\.[^.]*\.([^.]*)/
  - Details      /*.*.*.*      /^[^.]*\.[^.]*\.[^.]*\.([^.]*)/
  Das Feld Query kann geändert werden, um die Auswahl einzuschänken. Z. B. um nur Oracle
  Sections zu erhalten: /*.Oracle*


-- Bugfixe
-- Clientseitige Überwachungspausen: Der Startwert wird nicht mehr verändert.
   Wenn der Startwert nicht erlaubt ist, dann wird Pause abgelehnt.
-- $AVERAGEOVER: berichtigt bei aggregierten Limits
-- $DIFFERENCE: funktioniert nun auch bei "differ" und nicht nur bei "diffto"
-- Änderungprotokolle: Einige Trigger basierten auf OLD.TS != NEW.TS, durch den Update der
   MariaDB Version wurde dieser Trigger auch ausgelöst, wenn die Daten nicht geändert wurden,
   denn NEW.TS ist immer der neue TS, der geschrieben würde. Umgestellt auf: <table>.TS = NEW.TS
-- Komprimierung über Woche mit Einheit [E] gefixt
-- [G]-Auto-Kombilimits/-details: Wenn Details einer anderen Tabelle verwendet werden und diese
   später angelegt werden, dann wurden die resultierenden Limits oder Details nicht angelegt.
-- Rundung bei Kombidetails: Es wird nur noch nach dem Komma gerundet
-- Bei der Anzeige "Gruppierte Überwachungen" wurde bei Kombilimits bei den Überwachungspausen
   die Serverklasse nicht berücksichtigt.
-- Rundung bei Kombilimits: Es wird nur noch nach dem Komma entsprechend der signifikantgen
   Ziffern gerundet. Vor dem Komma wurde gerade bei Summen zu stark gerundet.
-- Leere Tabelle nach Datumsänderung über der Tabelle bei komprimierten Werten.
-- Tabelle "changeprot": Feld "newvalues" vergrößert auf 15000 Zeichen,
   denn es gabe gelegentlich Überläufe bei Limits mit sehr langen Excludes.
-- Neuer User müssen nun das Kennwort ändern, auch wenn die Kennwörter nicht ablaufen.
-- Meldung "Für diesen Server waren Überwachungspausen eingetragen." auch bei Überwachungspausen
   in der Zukunft.
-- Es konnten Userreport, die für eine eigene Gruppe freigegeben waren als
   Start-, Server- oder Section-Seite eingetragen werden. Es gehen nur noch Gruppenreports.


