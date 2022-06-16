# Changelog ULS-Server

You can find all changes made to the ULS-server in the README file on the stage server:
```
$ less ~uls/update/von_1.9.6/README.txt
```

REMEMBER: The current version of the ULS-server has a higher release!

Sorry, this changelog is in German.

Version 1.9.7

Voraussetzung ist mindesten MariaDB 10.4

Einheiten
- Neue Einheit {DTR} (Date Time Range) wie {T}, aber als ein Wert im Format:
  yyyy-mm-dd HH:MM:SS[.ssssss[{+|-}HH:MM]] - yyyy-mm-dd HH:MM:SS[.ssssss[{+|-}HH:MM]]
  Start Datum und Zeit                     - Ende Datum und Zeit
- Die mit '{E}' beginnenden Einheiten werden demnächst ersatzlos gestrichen, es sei
  denn, es gibt noch jemanden, der die Einheiten nutzt.
  Als Ersatz kann {T} oder {DTR} mit einem entsprechenden Detailnamen genutzt werden.


Details löschen
- Es werden alle Details gelöscht, aber die Aufbewahrungszeit wird nicht verändert.


Reports
- Neue Variablen in den Namen oder Beschreibungen:
  $IPADDRESSES: beide IP-Adressen, wenn angegeben
- Bei parametrisierten Reports werden die Variablen $DOMAIN, $SERVER, $CLASS, $IPADDRESS,
  $IPADDRESSES, $LOCATION, $CONTACT und $MAINTENANCEWINDOW in den Namen und Beschreibungen aufgelöst,
  wenn nur ein Server ausgewählt wird. Damit kann ein einfacher Report angelegt werden,
  um eine schnelle Übersicht eines Servers zu erhalten.


Gruppenreports
- Gruppenreports können für alle eigenen Verfahren freigegeben werden.
- Wenn Gruppenreports als User ausgeführt werden, gelten die Rechte des Users.
- Bei Mailreports aus Gruppenreports gelten die Rechte der Gruppe.


Mailreports
- Es können nun alle Mailreports mit "zip" gepackt werden.
- Durch setzen von 'nicht leer senden' wird der Report nicht gesendet, wenn im gewählten
  Zeitraum keine Werte vorhanden sind.


Überwachungen
- Neue Funktion in Meldungstexten: @SET_LEVEL(<level>). Man kann in Abhängikeit eines Details
  den Level der Meldung einstellen. Für <level> sind die Werte "PREINFO", "INFO", "WARN" oder "ERROR"
  erlaubt. Beispiel: Wenn der Werte des Details Level ERROR oder
  WARN ist, dann soll dies als Level gelten, ansonsten soll der Level "INFO" sein:
  @SET_LEVEL('@{if(find_in_set("$VALUE_OF(Level)", "ERROR,WARN"), "$VALUE_OF(Level)", "INFO")}')
- Ausgelöste Benachrichtigungen: Neue Checkbox "bestätigt".
  - "bestätigt": Das Ticket wird auf in Bearbeitung gesetzt, der Status geht auf "grün", es
               wird erneut alarmiert, wenn der Wert eine höher Schwelle überschreitet.
               Wenn der Wert unter die aktuelle Schwelle sinkt und dann wieder überschreitet
               wird auch alarmiert. Wenn der Wert auf der aktuellen Schwelle bleibt, wird
               nach der Wiederalarmierungszeit nicht erneut alarmiert.
  - "erledigt":  Der Status wird auf "grün" gesetzt und es kann unabhängig von der gesetzten
               Wiederalarmierungszeit beim Überschreiten eines Level alarmiert werden.
- Bei komprimierten Kombilimits wird das Ergebnis der Berechnung angezeigt.


Gruppierte Überwachungen
- Die Berechtigung auf die "gruppierten Überwachungen" wird über die Verfahrenszugehörigkeit
  geregelt. D. h. wenn es eine Verfahrensschnittmenge gibt, dann kann die gruppierte Überwachung
  einer anderen Gruppe angezeigt werden, aber nur mit den eigenen Verfahren
  (Schnittmenge der eigenen Verfahren mit den Verfahren der Gruppe).
  Neuer Button "alle Gruppen" auf oberster Ebene der "Gruppierten Überwachung".


Überwachungspausen
- Bei allen Formularen mit Überwachungspausen werden abgelaufene Überwachungspausen hellgrau
  und Überwachungspausen, die erst in der Zukunft aktiv werden, hellgrün hinterlegt.
- Das Ende ist bei neuen Überwachungspausen auf den nächsten Tag 08:00 Uhr vorbelegt,
  damit die Pausen nicht um Mitternacht auslaufen und es dann zu ungewollten
  Rufbereitschaftsalarmierungen kommt.


Meldungspläne
- Meldungspläne für Gruppenlimits. Über Meldungspläne können die Meldungen nach
  Level der Meldung, Klasse des Servers, Wochentag und Uhrzeit an unterschiedliche
  Meldungsziele geroutet werden.
  Bei gruppierten Limits können UTT Ziele nicht angesprochen werden.


Übergreifende Einträge
- Werde Daten mit dem Servernamen "DOMAIN:<hostname>" an ULS gesendet, werden die Daten
  unter dem Servernamen @<domainname> eingetragen.
  Wenn Daten mit dem Servernamen "DOMAIN:<hostname>:" gesendet werden, dann heisst
  der Servername im ULS @<serverklasse> und hat die selbe Serverklasse wie der <hostname>.
  Beim Servernamen "DOMAIN:<hostnams>:<name>" ergibt sich der Servername @<name> und
  gehört keiner Serverklasse an.
  Wenn also die übergreifenden Einträge über Meldungspläne gesteuert werden sollen, dann
  sollte der Servername "DOMAIN:<hostname>:" verwendet werden, denn nur so ergibt sich
  die entsprechende Serverklasse.


Reset Passwort
- Nach erfolgloser Anmeldung kann man sich per E-Mail einen Kennwort Rücksetzlink
  zusenden lassen. Der Link ist eine Stunde gültig und es kann frühestens nach
  einer Stunde ein neuer Link angefordert werden.


## Admin Menü

Server
- Die drei IP-Adressen "ip", "transferip" und "ipv6" zu zwei IP-Adressen "ip1" und "ip2"
  zusammengefasst. Bei "ip1" und "ip2" können IP-V4 oder IP-V6 IP-Adressen eingetragen werden.
  Wenn alle drei IP-Adressen belegt sind, wird beim Update die "transferip" als
  Werte-IP-Adresse eingetragen. "ip1" und "ip2" werden aus den eingetragenen IP-Adressen
  "ip", "ipv6", "transferip" generiert.

Verfahren - Autoserver bearbeiten
- Wenn Server aus dem IP-Bereich Werte senden und der Servername dem serverregexp
  entspricht, dann wird der Server dem Verfahren aus Domainname zugeordnet, wenn es das
  Verfahren gibt. Ansonsten wird der Server dem Verfahren "Default Verfahren" zugeordnet.
  Bei Domainname kann auf Suchpositionen Serverregexps verwiesen werden.
  Beispiel: Sendender Servername: "server.mydom.global"
            Serverregexp:         ".*\.([^.])*\..*$
            Domainname:           "local\1"
            Zielverfahren:        "localmydom", wenn vorhanden, sonst Defaultverfahren

Gruppierte Überwachungen
- Während einer Überwachungspause wird der Status der Limits auf 'unknown' gesetzt,
  so dass der Status der gruppierten Überwachung direkt nach dem Ende der Pause bis zur
  ersten Limitprüfung auf 'unknown' steht. Bisher wurde der letzte Stand vor der Pause
  angezeigt, was bei Alarmierungen auch zu Fehlalarmen führen konnte.

Verfahren / User / Gruppen
- Bei den Auswahlfeldern kann der Anfang (erste Buchstaben) angeklickt werden, so dass
  dann nach dem nächsten Buchstaben gruppiert wird.


## Bugfix
- Bei Gruppenreports wurden nicht alle Felder gesichert.
- Wildcard "<text>\, <text>" wurde zu "<text>\,<text>" konvertiert
- Bei Limits mit Aggregatfunktion 'all' wurde gelegentlich das Testdatum nicht upgedatet,
  so dass es beim direkten Ändern zu 'jeder Wert' zu einer Alarmierungsflut kam.
- Es konnte vorkommen, dass übergreifende Kombilimits oder Kombidetails nicht
  angelegt wurden, wenn das Detail mit Pfadangabe später gesendet wurde.
- Gruppenüberwachungspausen werden nun bei Limits aus Vorlagengruppen beachtet.
- Mailreports als CSV klappen nun auch mit Gruppierung
- Bei Limits / isAlives wurde bei der Anzeige der eingetragenen Gruppen-Überwachungspausen
  das Verfahren nicht beachtet.

  
