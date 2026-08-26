## 1. Einführung (3P)
### Klausur-SS25
3x Klausur2-SS24, Klausur1-SS24
![[Pasted image 20260708185812.png]]
Eine Sammlung unabhängiger Computer, die den Benutzern als ein einziger Computer erscheinen.
durch Netzwerk verbunden und gemeinsam eine Aufgabe lösen.
## 2. Architekturen (3P)
- Peer-to-Peer (P2P): Jeder Rechner ist gleichzeitig Informationsanbieter und –konsument
- Client-Server: 
	- Client -> Anfrage -> Server (Informationsanbieter)-> Antwort -> Client (Konsument)
Service-Oriented Architecture, Cluster, Cloud Computing, High Performance Computing,
Edge-Computing, Batch vs. Stream processingd
### Klausur2-SS23
![[Pasted image 20260721142125.png]]
### Klausur1-SS23
![[Pasted image 20260721151716.png]]
![[Pasted image 20260721151732.png]]

## 3. Grundlagen (4P)
Wettlaufsituation (engl. race condition): wenn nebenläufige Threads eines Prozesses auf gemeinsame Variablen schreiben, so ist das Ergebnis nicht deterministisch
### Klausur1-SS26
zwei Threads gleichzeitig auf eine gemeinsame Variable schreibend zugreifen, dass eine Schreiboperation von einem der zwei Threads verloren geht. 
Vermeidung: Synchronisierung, kritische Abschnitte Sperren
![[Pasted image 20260819153606.png]]
### Klausur-SS25
![[Pasted image 20260708190647.png]]
## 4. Sockets (16P)
- socket: door between application process and end-end-transport protocol Kommunikationsendpunkte
- 

| TCP Segment                                             | UDP Datagram                            |
| ------------------------------------------------------- | --------------------------------------- |
| Verbindung zwischen zwei Endpunkten                     | Verbindungslos                          |
| Erhält Sequenz der Nachrichten                          | Nachrichtensequenz nicht garantiert     |
| Verlorene Nachrichten werden automatisch neu übertragen | Keine Fluss- und Verstopfungskontrolle  |
| Flusskontrolle, Verstopfungskontrolle                   | Beispiele: Video- und Audioströme, tftp |
| Beispiele: https, ssh, sftp, X11                        |                                         |

IP: Adressierung
Port-Nummer: Identifikation eines bestimmten Diensts 
Beispiele reservierter Ports: 80 HTTP 22 SSH

#### Load-Balancer
Nutzer effizient und gleichmäßig auf mehrere Server zu verteilen
Einsatz mehrerer Server, aber alle Clients bekommen durch einen Load-Balancer einen Server zugeteilt, an den sie ihre Anfragen schicken
Nachteile: 
- Funktioniert so einfach nur, wenn jeder Server jede Anfrage beantworten kann ( replizierte Zustände).
- single point of failure
- Replikation und Konsistenz Verwaltung & Validierung geteilter Zustände 
#### asynchrone Sockets
- Asynchrone Programmierung erfordert Umdenken gegenüber blockierenden Threads → anspruchsvoll 
- Ereignisgetriebene Logik (ähnlich einem Zustandsautomat) muss nicht-blockieren realisiert werden → Sperren vermeiden


### Klausur-SS25
![[Pasted image 20260708191822.png]]
![[Pasted image 20260708193615.png]]
![[Pasted image 20260708194304.png]]
C) 
- Ein Thread in einer 64-Bit JVM belegt ca. 1 MB (auch wenn er inaktiv ist) 
- Erzeugen und Beenden von Threads kostet auch Zeit
- Viele Threads belasten auch den Scheduler
- Lösung asynchrone Sockets, Load-Balancer
### Klausur2-SS24
![[Pasted image 20260710192756.png]]
![[Pasted image 20260710193901.png]]
### Klausur2-SS23
![[Pasted image 20260721143900.png]]
![[Pasted image 20260721143930.png]]
b) Asynchrone TCP-Sockets sind sinnvoll, **wenn ein Programm gleichzeitig andere Aufgaben ausführen soll, während es auf Netzwerkdaten wartet**. (bei stark I/O-Operationen mit viel Warten)
Mit asynchronen Sockets kann das Programm währenddessen weiterarbeiten bzw. andere Verbindungen bearbeiten.
### Klausur1-SS23
![[Pasted image 20260721151816.png]]

## 5. RPC (33P)
![[Pasted image 20260822130345.png]]

- **Client :** ruft Funktionen auf dem Server auf
- **Server:** fernaufrufbaren Funktionen anbietet und ausführt
- **Schnittstelle:** Eine sprach- und plattformunabhängige *Beschreibung der aufrufbaren Funktionen*
- **Stub (Client-Seite):** Er setzt den Aufruf in einen Nachrichtenaustausch um, verpackt (Marshalling) die Aufrufparameter und entpackt (Unmarshalling) den Rückgabewert.
- **Skeleton (Server-Seite):**  Es entpackt die Aufrufparameter, gibt sie an den Server weiter, und verpackt nach der Ausführung den Rückgabewert in eine Nachricht.
- **Namensdienst (optional):** Der Server macht dort seinen Dienst bekannt.

Problem: Umsetzung von **Call-by-Reference** 
- Client und Server haben keinen gemeinsamen Speicher 
- Versand von Zeigern in Anfragen / Antworten geht nicht!
Lösung 1: Übermittlung einer Kopie des referenzierten Objekts
Lösung 2: Realisierung mittels Remote-Referenz
Übermittlung eines systemweit eindeutigen Zeigers anstatt der Speicheradresse



| **Ansatz**        | **Semantik**                                    | **Mechanismus**                                                                                                                                              | **Eigenschaften & Probleme**                                                                                                                                                                                                                               |
| ----------------- | ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **At-Least Once** | Fernaufruf wird mindestens ein Mal durchgeführt | Client wartet eine bestimmte Zeit auf Antwort (Timeout).Bei Ausbleiben der Antwort wird der Aufruf wiederholt.                                               | Gut geeignet für Leseoperationen.Problematisch bei Schreibzugriffen, da mehrfache Ausführung zu Seiteneffekten führt.                                                                                                                                      |
| **At-Most Once**  | Fernaufruf wird höchstens ein Mal durchgeführt  | Client ergänzt jeden Aufruf um eine global eindeutige ID.Server speichert diese ID mit Antwort und filtert so Duplikate heraus, um nicht erneut auszuführen. | Server muss alte Antworten und IDs speichern (Speicheraufwand).Diese Daten müssen vorsorglich persistent (auf Festplatte) gespeichert werden, um Duplikate auch nach einem Absturz zu erkennen. Wie lange muss der Server alte Antworten und IDs speichern |
| **Exactly Once**  | Fernaufruf wird exakt ein Mal ausgeführt.       | Kombiniert die Aufruf-Wiederholung (At-Least Once) mit der Duplikatfilterung (At-Most Once).                                                                 | Client (offene Aufrufe) und Server (bearbeitete Anfragen) müssen ihre Zustände persistent speichern, um nach Abstürzen weiterarbeiten zu können.Sehr aufwändig zu implementieren und in der Praxis kaum verwendet.                                         |

![[Pasted image 20260822141132.png]]

### Klausur1-SS26
![[Pasted image 20260819154051.png]]
b) Da Client und Server keinen gemeinsamen Speicher besitzen, muss die Parameterübergabe über den Austausch von Nachrichten erfolgen. Der allgemeine Fachbegriff für das Umwandeln und Verpacken der zu sendenden Daten in dieses Nachrichtenformat lautet **Marshalling** (und **Unmarshalling** für die Rekonstruktion beim Empfänger)
- **Primitive Datentypen:** Die Werte werden direkt in die Nachricht verpackt und über das Netzwerk an den Server geschickt.
	- Eingabeparameter Call-by-Value: client -> server 
		- Beispiel: ueberweiseGeld(in zielKonto, in betrag)
	- Ausgabeparameter Call-by-Result: server ->  client
		- Beispiel: holeAktuellenKontostand(out kontostand)
	- Ein-/Ausgabeparameter Call-by-Value-Result: client <-> server
		- Beispiel:  korrigiereFehlerhaftenBetrag(inout betrag)
	
- **Referenzen (Zeiger):** Entweder wird eine vollständige *Kopie des referenzierten* **(flachgeklopft)** Objekts inklusive aller Unterobjekte übertragen, oder es wird ein *systemweit eindeutiger Zeiger* **(Remote-Referenz)** verwendet.
### Klausur2-SS24
![[Pasted image 20260710194432.png]]
b) 
1. Client-Stub sendet einen Fernaufruf und wartet bestimmte Zeit auf eine Antwort vom Server. Falls in dieser Zeit keine Antwort vom Server eingeht, wird der Aufruf wiederholt.
2. Dies ist kritisch bei schreibenden Operationen mit Seiteneffekten, da der Server einen Aufruf möglicherweise mehrfach ausführt.
3. At-Least Once nur für lesenden Operationen verwenden (seiteneffektfreie Methoden) und für Methoden mit Seiteneffekte At-Most Once Ansatz verwenden. 
### Klausur1-SS24
![[Pasted image 20260711004637.png]]
![[Pasted image 20260711004727.png]]
### Klausur2-SS23
![[Pasted image 20260721144026.png]]
![[Pasted image 20260721144139.png]]

## 6. DSM (10P)

Bei einem Page-Fault muss die fehlende Seite gefunden werden
Feste Zuordnung von Adressbereichen zu Knoten
Anfordern per Multicast


- **True-Sharing:** Zwei oder mehr Objekte liegen auf einer Seite (Caching-Effekt)
- **False-Sharing:** Verschiedene Knoten greifen auf unterschied. Objekte innerhalb einer Speicherseite zu (Speicherseite hin- und her transportiert)


- **Objekt-basierter DSM** : Zugriffserkennung pro Objekt
	- Vermeidet False-Sharing



| Distributed Shared Memory (Speicherbasierte Kommunikation)                                                      | Message Passing (Nachrichtenbasierte Kommunikation)                                    |
| --------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| Vereinfachte implizite Kommunikation                                                                            | Serialisierung und Deserialisierung von Objekten                                       |
| Serialisierung von Objekten entfällt → keine Hüllenbildung für Referenzen notwendig (Marshalling/Unmarshalling) | Gekapselt in Middleware, z.B. gRPC, Web services                                       |
| Konsistenz wird für alle Objekte im DSM garantiert                                                              | Mit umfangreichen sprachunabhängigen Laufzeitumgebungen                                |
| Inhärente Replikation ist gute Basis für Fehlertoleranz                                                         | Aber Konsistenz & Fehlertoleranz von Daten/Objekt ist die Aufgabe des Programmierers   |
| I.d.R. etwas langsamer als Message Passing, aber einfacher zu programmieren                                     | Bei korrekter und optimaler Programmierung ist die maximale Nebenläufigkeit erzielbar. |

### Klausur1-SS26
![[Pasted image 20260819204938.png]]
### Klausur-SS25
![[Pasted image 20260708194833.png]]
![[Pasted image 20260708195042.png]]
d)  DSM: Speicher transparent zu aggregieren.
	Message Passing: 

### Klausur1-SS24
![[Pasted image 20260711004846.png]]
a) Speicherzugriffserkennung erfolgt hier pro Speicherseite
Page-Hit -> Zugriff im lokalen Speicher
Page-Fault -> die fehlende Seite anfordern per Multicast
## 7. Logische Zeit (68P)
- **Berkeley Verfahren** (aktiv): 
	- Server erfragt Zeit bei allen Clients
	- berechnet Durchschnitt der Abweichungen 
	- Individuelle Korrekturwerte berechnen und versenden
- **Cristian Verfahren (Passiver zentraler Zeitserver):**
	- Periodische Anfragen der Klienten an den Zeitserver
	- Antwort des Zeitservers, eventuell mit Mittelwertbildung
	- Halbieren der Antwortzeit ergibt ungefähre Zeit im Server
	- $T = T_s + t/2$
	- ![[Pasted image 20260823124825.png]]
- **Network Time Protocol (NTP):**
	- Synchronisierung der Server untereinander
	- 
	- ![[Pasted image 20260823124903.png]]
### Klausur1-SS26 (14P)
![[Pasted image 20260819153826.png]]
![[Pasted image 20260819210301.png]]
### Klausur-SS25 (13P)
![[Pasted image 20260708191237.png]]
![[Pasted image 20260708200927.png]]
![[Pasted image 20260708200940.png]]
![[Pasted image 20260708200954.png]]
![[Pasted image 20260708201027.png]]
![[Pasted image 20260708201051.png]]
### Klausur2-SS24 (10P)
![[Pasted image 20260710200754.png]]
![[Pasted image 20260710200925.png]]
### Klausur1-SS24 (10P)
![[Pasted image 20260711003652.png]]
### Klausur2-SS23 (11P)
![[Pasted image 20260721142415.png]]
![[Pasted image 20260721144440.png]]
![[Pasted image 20260721144523.png]]
### Klausur1-SS23 (10P)
![[Pasted image 20260721152333.png]]
![[Pasted image 20260721152436.png]]

## 8. Koordination (24P)
### Klausur1-SS26
![[Pasted image 20260819205839.png]]
### Klausur-SS25
![[Pasted image 20260708200138.png]]
a) **Bully-Algorithmus**
- Wenn ein Prozess P feststellt, dass der jetzige Koordinator nicht mehr reagiert, startet er den Auswahlprozess.
- P schickt Auswahl-Nachricht an alle Prozesse mit höherer ID 
	- Bekommt P keine Antwort, ist er der neue Koordinator
	- Bekommt P eine Antwort, ist seine Aufgabe erledigt.
- Antwortende Prozesse übernehmen die Aufgabe und halten wieder eine Wahl ab (rekursiv).
- Der „Stärkste“ (engl. bully) bleibt übrig, und schickt am Ende an alle eine Koordinator-Nachricht
![[Pasted image 20260708200306.png]]
![[Pasted image 20260824172845.png]]
## 9. Replikation und Konsistenz (62P)

### Klausur1-SS26 (7P)
![[Pasted image 20260819205531.png]]
### Klausur-SS25 (13P)
![[Pasted image 20260708195635.png]]

a) **Konsistenz:** Wann sieht ein anderer Server die Änderung?
**Kohärenz:** Wie sieht ein anderer Server die Änderung?
![[Pasted image 20260708195739.png]]
![[Pasted image 20260708195757.png]]
c) **Strikte Konsistenz:** Alle Leseoperationen liefern den global „zuletzt“ geschriebenen Wert - unabhängig davon, welcher Prozess geschrieben hat.
![[Pasted image 20260708195814.png]]

d) ![[Pasted image 20260708195927.png]]
e)
### Klausur2-SS24 (8P)
![[Pasted image 20260710195713.png]]
![[Pasted image 20260710195755.png]]
### Klausur1-SS24 (10P)
![[Pasted image 20260711002138.png]]
![[Pasted image 20260711002554.png]]
![[Pasted image 20260711002640.png]]

### Klausur2-SS23 (12P)
![[Pasted image 20260721143548.png]]

![[Pasted image 20260721145043.png]]
a) 
- Es müssen nicht immer alle Replikate erreicht werden
- Lese-Schreib-Zugriffe überlappen in mindestens einem Replikat 
- Beim Lesen hat dadurch mindestens ein Replikat die neueste Version
- Zwei Schreib-Zugriffe überlappen in mindestens einem Replikat 
- Wenigstens ein -Replikat erhält Kenntnis über beide Schreib-Operationen
![[Pasted image 20260721145155.png]]
b) 

| **Write-Invalidate**                                                                                                                       | **Write-Update**                                                                          |
| ------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------- |
| Verteilung von Aktualisierungshinweisen (Führt zur Löschung betroffener Daten. Beim nächsten Lesezugriff müssen Daten neu „geholt“ werden) | Verteilung der aktualisierten Werte. Lokaler Datenspeicher erfährt die neuen Werte direkt |
| Mehrfache Invalidierungen können unterbleiben                                                                                              | Daten sind immer aktuell                                                                  |
| Mehrere entfernte Aktualisierungen werden nur einmal geholt                                                                                | schnelle Lesezugriffe, ohne Netzwerkkommunikation                                         |
| wenig Kommunikationsbandbreite                                                                                                             | effezienter bei häufigem Lesen                                                            |


![[Pasted image 20260721145209.png]]

### Klausur1-SS23 (12P)
![[Pasted image 20260721152042.png]]
![[Pasted image 20260721153023.png]]
![[Pasted image 20260721153059.png]]
![[Pasted image 20260721153212.png]]
## 10. Fehlertoleranz ( 25P)
### Klausur1-SS26
![[Pasted image 20260819205627.png]]
### Klausur-SS25
![[Pasted image 20260708190301.png]]
b) 

| **Crash Failure**             | **Byzantine Failure**                      |
| ----------------------------- | ------------------------------------------ |
| Rechner- bzw. Programmabsturz | System kann sich beliebig falsch verhalten |
|                               |                                            |

### Klausur2-SS24
![[Pasted image 20260710192633.png]]
![[Pasted image 20260710195635.png]]
- **C**onsistency: alle Server sehen beim Lesen immer den zuletzt geschriebenen Wert
- **A**vailability: Alle Anfragen an das System werden in endlicher Zeit beantwortet
- **P**artitionstoleranz: Das System arbeitet auch bei Netzwerkpartitionierung weiter.
b) Das Theorem sagt, dass es in einem verteilten System unmöglich ist, gleichzeitig Konsistenz, Verfügbarkeit und Partitionstoleranz zu garantieren → es sind immer nur maximal zwei dieser Eigenschaften erzielbar.

### Klausur1-SS24
![[Pasted image 20260711001207.png]]
a) Lokaler Checkpoint von S1 enthält Nachrichtempfang, jedoch ist das Sende- ereignis nicht im lokalen Checkpoint von S2 enthalten.
Bei einer Rücksetzung im Fehlerfall würde die Nachricht erneut gesendet und somit zwei Mal empfangen
### Klausur2-SS23
![[Pasted image 20260721143352.png]]
![[Pasted image 20260721145532.png]]
![[Pasted image 20260721145548.png]]
a)
- **Verfügbarkeit** (engl. availability): Wahrscheinlichkeit für das korrekte Arbeiten des Systems zu einem gegebenem Zeitpunkt. 
- **Zuverlässigkeit** (engl. reliability): Zeitintervall solange das System sich korrekt verhält
### Klausur1-SS23
![[Pasted image 20260721152153.png]]
![[Pasted image 20260721152936.png]]

## 11. Eventual Consistency & Scaling Out: P2P-Systeme (70P)
### Klausur1-SS26
![[Pasted image 20260819210442.png]]
### Klausur-SS25
![[Pasted image 20260708201348.png]]
![[Pasted image 20260708201406.png]]
![[Pasted image 20260708201420.png]]
### Klausur2-SS24
![[Pasted image 20260710202034.png]]
### Klausur1-SS24
![[Pasted image 20260711004011.png]]

### Klausur2-SS23
![[Pasted image 20260721141434.png]]
![[Pasted image 20260721150529.png]]
![[Pasted image 20260721150623.png]]
### Klausur1-SS23
![[Pasted image 20260721154002.png]]
![[Pasted image 20260721154057.png]]

## 12. Eventual Consistency & Scaling Out: Key-Value Storage (31P)
### Klausur-SS25
![[Pasted image 20260708201721.png]]
![[Pasted image 20260708201758.png]]
![[Pasted image 20260708201820.png]]
### Klausur2-SS24
![[Pasted image 20260710232401.png]]
### Klausur1-SS24
![[Pasted image 20260711000825.png]]
### Klausur1-SS23
![[Pasted image 20260721154321.png]]
## 13. Eventual Consistency & Scaling Out: Google File System (GFS) (12P)
### Klausur2-SS24
![[Pasted image 20260710200246.png]]
![[Pasted image 20260710200353.png]]
![[Pasted image 20260710200326.png]]
### Klausur1-SS24
![[Pasted image 20260711001020.png]]
## 14. Strong Consistency & Scaling Out: Transaktionen (34P)
### Klausur1-SS26
![[Pasted image 20260819211543.png]]
### Klausur1-SS24
![[Pasted image 20260711001331.png]]
![[Pasted image 20260711001748.png]]
![[Pasted image 20260711001902.png]]
### Klausur2-SS23
![[Pasted image 20260721150251.png]]
![[Pasted image 20260721150346.png]]
### Klausur1-SS23
![[Pasted image 20260721153330.png]]
![[Pasted image 20260721153406.png]]
## 15. Strong Consistency & Scaling Out: Konsensus (58P)
### Klausur1-SS26
![[Pasted image 20260819211224.png]]
### Klausur-SS25
![[Pasted image 20260708203129.png]]
![[Pasted image 20260708203258.png]]
### Klausur2-SS24
![[Pasted image 20260710233845.png]]
![[Pasted image 20260710233921.png]]
### Klausur1-SS24
![[Pasted image 20260711003334.png]]
### Klausur2-SS23
![[Pasted image 20260721145612.png]]
![[Pasted image 20260721154559.png]]

## 16. Sicherheit (46P)
### Klausur1-SS26
![[Pasted image 20260819210801.png]]
### Klausur-SS25
![[Pasted image 20260708202210.png]]
![[Pasted image 20260708202221.png]]
### Klausur2-SS24
![[Pasted image 20260710233250.png]]
![[Pasted image 20260710233306.png]]
![[Pasted image 20260710233320.png]]
### Klausur1-SS24
![[Pasted image 20260711005159.png]]
![[Pasted image 20260711005212.png]]
### Klausur2-SS23
![[Pasted image 20260721150818.png]]
![[Pasted image 20260721150857.png]]
![[Pasted image 20260721150954.png]]
![[Pasted image 20260721151423.png]]
![[Pasted image 20260721151453.png]]
### Klausur1-SS23
![[Pasted image 20260721153754.png]]
![[Pasted image 20260721153823.png]]
![[Pasted image 20260721153850.png]]

## 17. NoSQL Storage (0P)

## 18. Map Reduce (0P)

## 19. In-Memory Processing (0P)




![[Pasted image 20260721150039.png]]
![[Pasted image 20260721151947.png]]



