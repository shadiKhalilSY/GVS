# GVS – Klausurlösungen nach Folien

> **Bearbeitbare Markdown-Version mit größerem Abstand und Vektorgrafiken**  

> Reihenfolge: **Woche → Originalfrage → Antwort**.  

> Bei mehrfach vorkommenden Fragen wird die Lösung nur einmal aufgeführt; die Klausurtermine stehen jeweils dabei.

# ==K01: Einführung  (3 P)

## 1. Begriff: Verteiltes System

**Vorgekommen in:** 16.09.2025 A1a; 17.09.2024 A1a; 23.07.2024 A1a


### Originalfrage aus der Klausur

```text
a) Definieren Sie den Begriff „Verteiltes System“. (1 P.)
```


#### Antwort


Autonome Rechnern, die über ein Netzwerk verbunden sind und gemeinsam eine Aufgabe lösen.

---

# ==K02: Architekturen  (3 P)

## 1. Peer-to-Peer - Multiple Choice

**Vorgekommen in:** 19.07.2023 A1a

![[Pasted image 20260903112326.png]]

#### Antwort


- **Richtig:** Jeder Rechner ist gleichzeitig Anbieter und Konsument.


## 2. Edge-Computing - Multiple Choice

**Vorgekommen in:** 19.07.2023 A1b; 21.09.2023 A1b


![[Pasted image 20260903112513.png]]
<br>
#### Antwort


- **Richtig:**
-  Speicher und Rechenleistung auf **IoT-Geräten** werden genutzt.
-  Die Berechnungen finden **nahe der Datenquelle** statt.

# ==K03: Grundlagen  (4 P)

## 1. Lost Update

**Vorgekommen in:** 29.07.2026 A1a; 16.09.2025 A1c

![[Pasted image 20260903112731.png]]

#### Antwort


- **Lost Update:** Zwei Threads ändern dieselbe Variable; ein Update wird überschrieben.
- **Vermeidung:** kritischen Abschnitt sperren, z. B. `synchronized`.

---

# ==K04: Sockets  (16 P)

## 1. Verlorene Nachrichten bei TCP

**Vorgekommen in:** 16.09.2025 A2a


![[Screenshot 2026-09-03 at 11.28.35.png]]
#### Antwort


TCP erkennt den Verlust und sendet die fehlenden Daten erneut.

<br>

## 2. IP-Adresse und Port beim TCP-Verbindungsaufbau

**Vorgekommen in:** 16.09.2025 A2b; 17.09.2024 A1c

![[Screenshot 2026-09-03 at 11.29.11.png]]


#### Antwort


- **IP-Adresse:** Zielrechner.
- **Port:** Dienst/Prozess auf diesem Rechner.

<br>

## 3. Warum skaliert "ein Thread pro Client" schlecht?

**Vorgekommen in:** 16.09.2025 A2c


### Originalfrage aus der Klausur

![[Pasted image 20260917202609.png]]

<br>

#### Antwort


Viele Threads brauchen viel Speicher und Scheduling-Aufwand. **Besser:** asynchrone Sockets oder Thread-Pool.

<br>

## 4. TCP und UDP

**Vorgekommen in:** 17.09.2024 A2a; 21.09.2023 A2a; 19.07.2023 A1c

![[Screenshot 2026-09-03 at 11.29.47.png|855]]

#### Antwort


| TCP                                                        | UDP                                                |
| ---------------------------------------------------------- | -------------------------------------------------- |
| verbindungsorientiert, zuverlässig, Reihenfolge garantiert | verbindungslos, keine Zustell-/Reihenfolgegarantie |
| z. B. HTTPS/SSH                                            | z. B. Streaming                                    |


## 5. Asynchrone TCP-Sockets

**Vorgekommen in:** 21.09.2023 A2b

![[Screenshot 2026-09-03 at 11.31.07.png]]

#### Antwort


Sinnvoll bei sehr vielen gleichzeitigen Verbindungen: Ein Thread kann mehrere Sockets bedienen.


---

# ==K05: RPC: Remote Procedure Call  (33 P)

## 1. Komponenten eines RPC

**Vorgekommen in:** 29.07.2026 A2a; 23.07.2024 A7a; 21.09.2023 A2c

![[Screenshot 2026-09-03 at 11.32.02.png|1087]]

#### Antwort

![[Pasted image 20260903113308.png|1093]]


## 2. Parameterübergabe bei RPC

**Vorgekommen in:** 29.07.2026 A2b; 23.07.2024 A7b; 21.09.2023 A2d

![[Screenshot 2026-09-03 at 11.34.07.png]]


### Antwort




- **Primitive Datentypen – Call-by-Value:**
    - Der Wert wird als Kopie an den Server übertragen.
    - Änderungen an der Kopie verändern die Variable beim Client nicht.
    
- **Referenzen:**
    - Direktes **Call-by-Reference** über Speicheradressen ist nicht möglich, da Client und Server getrennte Speicher besitzen.
    - Das referenzierte Objekt wird serialisiert und als Kopie übertragen.
    - Dabei werden auch alle darüber erreichbaren Objekte übertragen (**transitive Hüllenbildung**).
    - Werden Änderungen zurückübertragen und beim Client übernommen, nennt man dies **Call-by-Value-Result**.
    - Alternativ verwendet man **Remote-Referenzen**; der Zugriff erfolgt über weitere RPCs.








## 3. RPC: At-Least Once

**Vorgekommen in:** 17.09.2024 A2b

![[Screenshot 2026-09-03 at 11.34.54.png|962]]


### Antwort


- Client sendet bei Timeout erneut.
- Aufruf kann **mehrfach** ausgeführt werden.
- Lösung: eindeutige Request-ID; Duplikate erkennen.

OR:

**1. Vorgehen des Clients:**

- Nach einem Timeout sendet der Client die Anfrage erneut, bis er eine Antwort erhält.

**2. Problem auf dem Server:**

- Eine Anfrage kann mehrfach ausgeführt werden, beispielsweise wenn nur die Antwort verloren geht.
- Bei nicht-idempotenten Operationen kann dies unerwünschte Auswirkungen haben, z. B. eine doppelte Abbuchung.

**3. Umgang mit dem Problem:**

- Anfragen durch eindeutige IDs erkennen und Ergebnisse speichern.
- Bei Duplikaten das gespeicherte Ergebnis zurücksenden, ohne die Operation erneut auszuführen.
- Alternativ idempotente Operationen verwenden.
---


# ==K06: DSM: Distributed Shared Memory (10 P)

## 1. DSM vs. Message Passing

**Vorgekommen in:** 16.09.2025 A2d

![[Screenshot 2026-09-03 at 11.36.21.png|1006]]

### Antwort


- **DSM:** einfacher wie gemeinsamer Speicher.
- **Message Passing:** mehr Kontrolle und oft bessere Performance.



## 2. Speicherzugriffserkennung beim seitenbasierten DSM

**Vorgekommen in:** 29.07.2026 A2c1; 23.07.2024 A7c1

![[Pasted image 20260903113654.png|1032]]

### Antwort


Über **Page Faults**: Fehlt die Seite lokal, wird sie über das Netzwerk geholt.

<br>

## 3. False Sharing

**Vorgekommen in:** 29.07.2026 A2c2; 16.09.2025 A2e; 23.07.2024 A7c2

![[Screenshot 2026-09-03 at 11.37.08.png|1071]]

### Antwort


- Mehrere unabhängige Daten liegen auf derselben Seite; dadurch wird die ganze Seite unnötig übertragen.
- **Lösung:** Daten auf verschiedene Seiten legen / kleinere Granularität.


# ==K07: Logische Zeit  (68 P)

## 1. Uhrensynchronisierung nach Cristian

**Vorgekommen in:** 29.07.2026 A1b; 16.09.2025 A1d


![[Screenshot 2026-09-03 at 11.37.45.png|833]]

### Antwort

Client fragt Zeitserver, misst die **RTT** und setzt seine Uhr ungefähr auf **Serverzeit + RTT/2**.

![[Pasted image 20260903113835.png|982]]


## 2. Network Time Protocol - Multiple Choice

**Vorgekommen in:** 21.09.2023 A1c


### Originalfrage aus der Klausur




![[Pasted image 20260828121014.png|514]]


![[Screenshot 2026-09-18 at 12.18.20.png]]
<br>

### Antwort
- tauscht wechselseitig Zeitstempel aus.

<br>

## 3. Allgemeine Methode für Lamport- und Vektorzeit

**Vorgekommen in:** 16.09.2025 A5; 17.09.2024 A5; 23.07.2024 A5; 21.09.2023 A3; 19.07.2023 A2

Lamport-Zeit:
![[Pasted image 20260828121118.png|945]]

Vektor-Zeit:
![[Pasted image 20260828121148.png|945]]
### Lösungsmethode

Wie berechnet man Lamport- und Vektorzeiten?


### Vorgehensweise / Lösungsschritte


1. Lamport: lokal `+1`, Empfang `max()+1`.
2. Vektor: eigene Komponente `+1`, beim Empfang komponentenweise `max`.

<br>

### Antwort


- **Lamport:** lokal/Senden `+1`; Empfangen `max(lokal, empfangen)+1`.
- **Vektor:** eigene Komponente `+1`; beim Empfang komponentenweise `max`.
- Nur Vektorzeit zeigt Nebenläufigkeit direkt.

<br>

## 4. Lamport- und Vektorzeit :NK - 2025

**Vorgekommen in:** 16.09.2025 A5


### Originalfrage aus der Klausur
![[Pasted image 20260918115302.png|802]]

#### Antwort

- C: `1`
- C → B: Senden `2`, Empfangen bei B `3`
- B → C: Senden `4`, Empfangen bei C `5`
- C → A: Senden `6`, Empfangen bei A `7`
- B: `5`


![[Screenshot 2026-09-18 at 11.53.15.png|767]]
#### Antwort
C;
Senden C → B;
Empfangen bei B;
Senden B → C.

![[Pasted image 20260918115335.png|769]]
#### Antwort

A: `TL = 7`, B: `TL = 5`.

**Keine sichere Aussage über eine Abhängigkeit möglich**, da aus `TL(B) < TL(A)` nicht automatisch `B → A` folgt.

![[Pasted image 20260918115355.png|784]]
#### Antwort

Vektorreihenfolge `(A,B,C)`:

- C: `(0,0,1)`
- C → B senden: `(0,0,2)`
- B empfängt: `(0,1,2)`
- B → C senden: `(0,2,2)`
- C empfängt: `(0,2,3)`
- C → A senden: `(0,2,4)`
- A empfängt: `(1,2,4)`
- B: `(0,3,2)`

![[Pasted image 20260918115415.png|752]]
#### Antwort

A = `(1,2,4)`, B = `(0,3,2)`.

Die Vektoren sind nicht vergleichbar → **A und B sind nebenläufig**.


<br>

## 5. Lamport- und Vektorzeit :NK - 2024

**Vorgekommen in:** 17.09.2024 A5


###  Originalfrage aus der Klausur

![[Screenshot 2026-09-18 at 11.55.26.png|914]]
#### Antwort

- B: `1`
- B → A: Senden `2`, Empfangen bei A `3`
- A → C: Senden `4`, Empfangen bei C `5`
- C → B: Senden `6`, Empfangen bei B `7`
- A: `5`


![[Screenshot 2026-09-18 at 11.55.58.png|927]]
#### Antwort

B; Senden B → A; Empfangen bei A; Senden A → C.


![[Screenshot 2026-09-18 at 11.56.07.png|960]]
#### Antwort

Letztes B-Ereignis: `TL = 7`, letztes A-Ereignis: `TL = 5`.

**Keine sichere Abhängigkeit ableitbar.**


![[Screenshot 2026-09-18 at 11.56.18.png|971]]
#### Antwort

Vektorreihenfolge `(A,B,C)`:

- B: `(0,1,0)`
- B → A senden: `(0,2,0)`
- A empfängt: `(1,2,0)`
- A → C senden: `(2,2,0)`
- C empfängt: `(2,2,1)`
- C → B senden: `(2,2,2)`
- B empfängt: `(2,3,2)`
- A: `(3,2,0)`



![[Screenshot 2026-09-18 at 11.56.30.png|986]]
#### Antwort

B = `(2,3,2)`, A = `(3,2,0)`.

Nicht vergleichbar → **nebenläufig**.


<br>

## 6. Lamport- und Vektorzeit :HK - 2024

**Vorgekommen in:** 23.07.2024 A5


### Originalfrage aus der Klausur


![[Screenshot 2026-09-18 at 11.57.03.png|984]]

#### Antwort
- A: `1`
- A → C: Senden `2`, Empfangen bei C `3`
- C → A: Senden `4`, Empfangen bei A `5`
- A → B: Senden `6`, Empfangen bei B `7`
- C: `5`

![[Screenshot 2026-09-18 at 11.57.27.png|1000]]
#### Antwort

A; Senden A → C; Empfangen bei C; Senden C → A.


![[Pasted image 20260918115757.png|1006]]
#### Antwort

B: `TL = 7`, C: `TL = 5`.

**Keine sichere Aussage über eine Abhängigkeit möglich.**

![[Pasted image 20260918115815.png|1016]]
#### Antwort

Vektorreihenfolge `(A,B,C)`:

- A: `(1,0,0)`
- A → C senden: `(2,0,0)`
- C empfängt: `(2,0,1)`
- C → A senden: `(2,0,2)`
- A empfängt: `(3,0,2)`
- A → B senden: `(4,0,2)`
- B empfängt: `(4,1,2)`
- C: `(2,0,3)`


![[Pasted image 20260918115826.png|1018]]
#### Antwort

B = `(4,1,2)`, C = `(2,0,3)`.

Nicht vergleichbar → **B und C sind nebenläufig**.


<br>

## 7. Lamport- und Vektorzeit :NK - 2023

**Vorgekommen in:** 21.09.2023 A3


### Originalfrage aus der Klausur


![[Screenshot 2026-09-18 at 11.59.03.png]]
#### Antwort

- A: `1`
- B → C: Senden `1`, Empfangen `2`
- C → A: Senden `3`, Empfangen `4`
- C: `4`
- C → B: Senden `5`, Empfangen `6`
- A: `5`


![[Screenshot 2026-09-18 at 11.59.22.png]]
#### Antwort

Erstes A; Senden B → C; Empfangen bei C; Senden C → A; Empfangen bei A.


![[Screenshot 2026-09-18 at 11.59.33.png]]
#### Antwort

A: `TL = 5`, B: `TL = 6`.

Aus `5 < 6` kann **keine kausale Abhängigkeit** gefolgert werden.



![[Screenshot 2026-09-18 at 11.59.49.png]]
#### Antwort

Vektorreihenfolge `(A,B,C)`:

- A: `(1,0,0)`
- B → C senden: `(0,1,0)`
- C empfängt: `(0,1,1)`
- C → A senden: `(0,1,2)`
- A empfängt: `(2,1,2)`
- C: `(0,1,3)`
- C → B senden: `(0,1,4)`
- B empfängt: `(0,2,4)`
- A: `(3,1,2)`



![[Screenshot 2026-09-18 at 12.00.03.png]]
#### Antwort

A = `(3,1,2)`, B = `(0,2,4)`.

Nicht vergleichbar → **Die Ereignisse sind nebenläufig**.


<br>

## 8. Lamport- und Vektorzeit :HK - 2023

**Vorgekommen in:** 19.07.2023 A2


### Originalfrage aus der Klausur


![[Screenshot 2026-09-18 at 12.00.53.png]]
#### Antwort

- A: `1`
- A → B: Senden `2`, Empfangen `3`
- B → C: Senden `4`, Empfangen `5`
- C → A: Senden `6`, Empfangen `7`
- B: `5`


![[Screenshot 2026-09-18 at 12.01.05.png|790]]
#### Antwort

A; Senden A → B; Empfangen bei B; Senden B → C.



![[Pasted image 20260918120126.png]]
#### Antwort

A: `TL = 7`, B: `TL = 5`.

**Keine kausale Abhängigkeit aus den Lamportzeiten ableitbar.**



![[Screenshot 2026-09-18 at 12.01.47.png]]
#### Antwort

Vektorreihenfolge `(A,B,C)`:

- A: `(1,0,0)`
- A → B senden: `(2,0,0)`
- B empfängt: `(2,1,0)`
- B → C senden: `(2,2,0)`
- C empfängt: `(2,2,1)`
- C → A senden: `(2,2,2)`
- A empfängt: `(3,2,2)`
- B: `(2,3,0)`



![[Screenshot 2026-09-18 at 12.02.02.png]]
#### Antwort

A = `(3,2,2)`, B = `(2,3,0)`.

Nicht vergleichbar → **Die Ereignisse sind nebenläufig**.

<br>

## 9. Logische Zeit :HK - 2026

**Vorgekommen in:** 29.07.2026 A5


### Originalfrage aus der Klausur

![[Pasted image 20260828121458.png|937]]


### Vorgehensweise / Lösungsschritte

1. Ein Sprung einer Lamportuhr, der nicht durch lokale +1-Fortsetzung erklaerbar ist, benötigt einen Empfang von einer entsprechend großen Senderzeit.
2. So wenig Nachrichten wie möglich wählen.
3. Danach Vektoren entlang der gegebenen Pfeile berechnen.

<br>

### Antwort


- **Minimale Nachrichten:** 3.
- Eine gültige Wahl: `A2→B3`, `B4→C5`, `C6→A7`.
- Vektorzeiten anschließend mit der Standardregel berechnen.

![Skizze|822](logical_2026_vector.svg)

---


# ==K08: Koordination  (24 P)

## 1. Bully-Algorithmus

**Vorgekommen in:** 16.09.2025 A4a


![[Screenshot 2026-09-19 at 08.22.10.png|1014]]

### Antwort:

- Erkennt ein Prozess den Ausfall des Koordinators, sendet er **ELECTION** an alle Prozesse mit höherer ID.
- Antwortet niemand innerhalb des Timeouts, wird er selbst Koordinator und informiert alle anderen.
- Antwortet ein höherer Prozess mit **OK**, übernimmt dieser die Wahl; der ursprüngliche Prozess wartet auf das Ergebnis.
- Der aktive Prozess mit der **höchsten ID** gewinnt und sendet eine **COORDINATOR-Nachricht** an die anderen Prozesse.

<br>

## 2. Lamport-Algorithmus für gegenseitigen Ausschluss - Grundidee

**Vorgekommen in:** 29.07.2026 A4; 16.09.2025 A4b


### Lösungsmethode

Welche Schritte benutzt das Lamport-Verfahren?


### Vorgehensweise / Lösungsschritte


1. REQUEST an alle.
2. Queue nach `(Zeit,PID)`.
3. Eintritt bei allen REPLYs + eigenem Request vorne.
4. Danach RELEASE.



<br>

### Antwort


REQUEST an alle → Queue nach **(Zeit, PID)** sortieren → Eintritt erst bei allen REPLYs und eigenem Request vorne → danach RELEASE.

<br>

## 3. Lamport-Mutex : HK 2026

**Vorgekommen in:** 29.07.2026 A4


### Originalfrage aus der Klausur
![[Pasted image 20260828122751.png|744]]


### Vorgehensweise / Lösungsschritte

1. Beide Requests überall eintragen und nach Zeitstempel sortieren.
2. (3,3) steht vor (5,1), daher darf P3 zuerst hinein.
3. Nach RELEASE von P3 wird (3,3) überall entfernt; danach ist P1 an der Reihe.

<br>

### Antwort


`(3,3)` steht vor `(5,1)` → **P3 zuerst**, danach RELEASE → **P1**.


![[Pasted image 20260919104330.png|1369]]
HINWEIS: ACK == REPLY

<br>

## 4. Lamport-Mutex : NK 2025

**Vorgekommen in:** 16.09.2025 A4b


### Originalfrage aus der Klausur
![[Pasted image 20260919082616.png|854]]


### Vorgehensweise / Lösungsschritte

1. Requests verteilen und sortieren.
2. (3,1) steht vor (5,3), daher tritt P1 zuerst ein.
3. Danach RELEASE von P1, dann darf P3 eintreten.

<br>

### Antwort


`(3,1)` steht vor `(5,3)` → **P1 zuerst**, danach RELEASE → **P3**.
![[Screenshot 2026-09-19 at 10.55.38.png]]



---


# ==K09: Replikation und Konsistenz  (62 P)

## 1. Konsistenz und Kohärenz

**Vorgekommen in:** 29.07.2026 A3c; 16.09.2025 A3a; 19.07.2023 A3c


![[Screenshot 2026-09-19 at 08.28.32.png|812]]
### Antwort

A und B beide haben eine Replikation von Variable: x = 5  und dann: A ändert x = 8.
nun die Frage ist :

- **Konsistenz:** Wann wird die Änderung für B sichtbar?
- **Kohärenz:** Wie wird die Änderung für B verteilt?

<br>

## 2. Schwache Konsistenz (sync): Vorteil/Nachteil

**Vorgekommen in:** 16.09.2025 A3b; 21.09.2023 A1e; 19.07.2023 A1e
![[Pasted image 20260828181934.png|832]]



![[Screenshot 2026-09-19 at 08.29.33.png|832]]
<br>

### Antwort


- **Vorteil:** hohe Nebenläufigkeit / weniger Kommunikation.
- **Nachteil:** ohne korrektes `sync()` können alte Werte gelesen werden.

<br>

## 3. Strikte Konsistenz

**Vorgekommen in:** 16.09.2025 A3c

![[Pasted image 20260828181726.png|853]]


![[Screenshot 2026-09-19 at 08.30.06.png|865]]


### Antwort


Jeder Read liefert den **global zuletzt geschriebenen Wert**.



## 4. Sequentielle Konsistenz

**Vorgekommen in:** 16.09.2025 A3d

![[Pasted image 20260828181839.png|898]]


![[Screenshot 2026-09-19 at 08.30.26.png|911]]

### Antwort


Alle Operationen müssen sich als **eine gemeinsame Reihenfolge** darstellen lassen; die Reihenfolge jedes Prozesses bleibt erhalten.

<br>

## 5. Write-Update vs. Write-Invalidate

**Vorgekommen in:** 17.09.2024 A3c; 23.07.2024 A3a; 21.09.2023 A4b; 19.07.2023 A3d



![[Screenshot 2026-09-19 at 08.30.55.png|1282]]

### Antwort


- **Write-Update:** neuen Wert senden → gut bei vielen Reads.
- **Write-Invalidate:** Kopien ungültig machen → gut bei vielen Writes.



***Vertiefung***:  

Nach einer Änderung von x = 5 -> x = 8:

- **Write-Update:** Bei einer Änderung wird der neue Wert an alle anderen Kopien übertragen. 
  Vorteilhaft bei häufigen Lesezugriffen anderer Rechner zwischen den Schreibzugriffen.
	  --> x ist 8 in alle Kopien
	
- **Write-Invalidate:** Bei einer Änderung werden die anderen Kopien ungültig gemacht.
  Der aktuelle Wert wird erst beim nächsten Lesen nachgeladen. Vorteilhaft bei vielen Schreibzugriffen und wenigen Lesezugriffen anderer Rechner. 
	  --> x = 5 ist nicht mehr gültig in alle Kopien.
<br>

## 6. Aktualisierende Operationen

**Vorgekommen in:** 23.07.2024 A3b


![[Screenshot 2026-09-19 at 08.31.20.png|1113]]
<br>
### Antwort


- **Vorteil:** weniger Datenverkehr als kompletter Datenblock.
- **Bedingung:** Operation muss deterministisch sein und überall dieselben Daten haben.

***OR:***
- **Vorteil:** Die Operation mit ihren Parametern ist oft kleiner als die geänderten Daten und spart dadurch Bandbreite.
- **Voraussetzung:** Alle benötigten Daten müssen lokal verfügbar sein. Die Operation muss auf den Replikaten deterministisch ausgeführt werden und zum gleichen Ergebnis führen.

## 7. Vorteil quorenbasierter Replikation

**Vorgekommen in:** 21.09.2023 A4a


![[Screenshot 2026-09-19 at 08.32.01.png|1112]]
<br>
### Antwort

Nicht alle Replikate müssen erreichbar sein; überlappende Quoren ermöglichen trotzdem aktuelle Reads/Writes.



## 8. Schwache Konsistenz - Sync-Variante 23.07.2024

**Vorgekommen in:** 23.07.2024 A3c


![[Screenshot 2026-09-19 at 08.32.24.png|988]]
### Vorgehensweise / Lösungsschritte


1. Writer veröffentlicht mit `sync()`.
2. Reader führt vor dem benötigten Read `sync()` aus.

<br>

### Antwort
![[Screenshot 2026-09-19 at 21.02.13.png|1504]]

Minimal **5 syncs**: P2 nach `w(a)=2`; P3 vor `r(a)=2`; P1 nach `w(a)=4`; P2 vor `r(a)=4`; P3 vor `r(a)=4`.

<br>

## 9. Schwache Konsistenz - Aufgabe 29.07.2026

**Vorgekommen in:** 29.07.2026 A3d

<br>![[Screenshot 2026-09-19 at 08.35.15.png|874]]

### Antwort


Die Aufgabe ist in der abgedruckten Form **widersprüchlich**: Es gibt keinen Write `a=4`, obwohl später `a=4` gelesen werden soll.

> **Hinweis:** Falls im Diagramm statt w(b)=4 eigentlich w(a)=4 gemeint war, wäre die Aufgabe analog zur Variante vom 23.07.2024 lösbar. Diese Korrektur wird hier aber nicht stillschweigend angenommen.



<br>

## 10. Strikte/sequentielle Konsistenz - Muster A

**Vorgekommen in:** 16.09.2025 A3e; 17.09.2024 A3d


### Originalfrage aus der Klausur

![[Pasted image 20260828182436.png|948]]


### Vorgehensweise / Lösungsschritte


1. Strikt: letzten realen Write prüfen.
2. Sequentiell: mögliche gemeinsame Reihenfolge prüfen.

<br>

### Antwort

- **Strikt:** nein, weil Reads nicht den letzten realen Write liefern.
- **Sequentiell:** nein, weil P3 und P4 widersprüchliche Write-Reihenfolgen verlangen.


![[Screenshot 2026-09-19 at 21.17.30.png|1001]]




## 11. Strikte/sequentielle Konsistenz - Muster B

**Vorgekommen in:** 21.09.2023 A4c


### Originalfrage aus der Klausur

```text
W1(x)=1, W1(x)=2, W2(x)=3; P3 liest 1,3,2 und P4 liest 3,1,2.
```

![[Screenshot 2026-09-19 at 08.36.06.png|827]]

### Vorgehensweise / Lösungsschritte


1. Strikt: letzten realen Write prüfen.
2. Sequentiell: Write-Reihenfolgen aus den Reads ableiten.

<br>![[Screenshot 2026-09-19 at 21.35.08.png|989]]

### Antwort


- **Strikt:** nein.
- **Sequentiell:** nein; P3 verlangt `W1 < W2`, P4 dagegen `W2 < W1`.

---

# ==K10: Fehlertoleranz (25 P)

## 1. Crash-Failure und Byzantine-Failure

**Vorgekommen in:** 16.09.2025 A1b

![[Screenshot 2026-09-19 at 08.41.04.png|1025]]
<br>
### Antwort


- **Crash:** Prozess fällt aus und antwortet nicht mehr.
- **Byzantine:** Prozess kann beliebig/falsch handeln.

***OR***:

- **Crash-Failure:** Ein Prozess arbeitet korrekt, bis er ausfällt. Danach führt er keine weiteren Aktionen aus und antwortet nicht mehr.
- **Byzantine-Failure:** Ein Prozess verhält sich beliebig fehlerhaft und kann falsche oder widersprüchliche Nachrichten senden.

## 2. Availability und Reliability

**Vorgekommen in:** 21.09.2023 A5c


![[Screenshot 2026-09-19 at 08.41.25.png|1005]]
<br>

### Antwort


- **Availability (Verfügbarkeit):** Wahrscheinlichkeit, dass ein System zu einem bestimmten Zeitpunkt betriebsbereit ist. *(Zeitpunkt)*
- **Reliability (Zuverlässigkeit):** Wahrscheinlichkeit, dass ein System während eines bestimmten Zeitraums ohne Ausfall korrekt funktioniert. *(Zeitraum)*

<br>

## 3. CAP und CAP-Theorem

**Vorgekommen in:** 29.07.2026 A3a/b; 17.09.2024 A3a/b; 21.09.2023 A5a/b; 19.07.2023 A3a/b

<br>![[Screenshot 2026-09-19 at 08.41.54.png|902]]

### Antwort


a)

| Abkz. | Ausgeschrieben      | Eigenschaft                       |
| ----- | ------------------- | --------------------------------- |
| C     | Consistency         | letzter Wert wird gelesen.        |
| A     | Availability        | jede Anfrage wird beantwortet.    |
| P     | Partition Tolerance | funktioniert trotz Netzpartition. |

b) - Bei Partition kann man **C und A nicht gleichzeitig vollständig garantieren**.

<br>

## 4. Orphan Message beim Checkpointing

**Vorgekommen in:** 17.09.2024 A1b; 23.07.2024 A1d

<br>![[Pasted image 20260920105359.png|992]]

### Antwort

- **Orphan-Message:** Eine Nachricht, deren Empfang im wiederhergestellten Zustand enthalten ist, deren Senden jedoch nicht.

- **Recovery:** Der Empfänger muss auf einen Checkpoint vor dem Empfang zurückgesetzt werden, sodass der Empfang und seine Auswirkungen rückgängig gemacht werden. Dadurch kann ein Domino-Effekt entstehen.
<br>


## 5. Aktive Fehlererkennung - Multiple Choice

**Vorgekommen in:** 21.09.2023 A1f


```text
Welche Aussagen sind richtig?
```
<br>![[Pasted image 20260920105112.png|883]]

### Antwort

 **Richtig:**
- wird der Ausfall mittels eines Timeouts erkannt  ***Timeout***
- werden periodisch Heartbeat-Nachrichten verschickt. ***Heartbeat***



## 6. Koordiniertes Checkpointing - Multiple Choice

**Vorgekommen in:** 21.09.2023 A1d



```text
Welche Aussage ist richtig?
```

![[Pasted image 20260920110248.png|758]]
<br>

### Antwort

**Richtig:**
- wird das gesamte System angehalten

<br>

## 7. Unabhängiges Checkpointing - Multiple Choice

**Vorgekommen in:** 19.07.2023 A1f

```text
Welche Aussagen sind richtig?
```

![[Pasted image 20260920110339.png|949]]
<br>

### Antwort

**Richtig:**
- muss im Fehlerfall zunächst ein konsistenter Zustand berechnet werden
- kann das System auf den Initialzustand zurückfallen

---



# ==K11: P2P-Systeme und Chord (70 P)

## 1. Distributed Hash Table - Multiple Choice

**Vorgekommen in:** 21.09.2023 A1a

<br>![[Screenshot 2026-09-19 at 08.43.52.png]]

### Antwort


**Richtig:** :
	unterteilt den Hash-Wertebereich in nicht überlappende Teilbereiche

<br>

## 2. Chord - allgemeine Lösungsmethode

**Vorgekommen in:** 29.07.2026 A6; 16.09.2025 A6; 17.09.2024 A6; 23.07.2024 A6; 21.09.2023 A7; 19.07.2023 A6


### Lösungsmethode

Wie löst man Zuständigkeit, naive Suche, Fingertabelle und skalierbare Suche?


### Vorgehensweise / Lösungsschritte


1. Zuständigen Successor bestimmen.
2. Finger: `succ(n+2^i)`.
3. Immer zum größten passenden Finger springen.

<br>

#### Antwort


- Zuständig: erster Knoten im Uhrzeigersinn mit `ID ≥ Key`.
- Finger `i`: `succ(n + 2^i)`.
- Suche mit Finger Table: **O(log N)**.

<br>

## 3. Chord : HK - 2026

**Vorgekommen in:** 29.07.2026 A6

![[Screenshot 2026-09-19 at 08.48.37.png|783]]
#### Antwort

**N4**



![[Screenshot 2026-09-19 at 08.49.36.png|853]]
#### Antwort

![[Pasted image 20260920112018.png|857]]

Gesuchter Key: `11` → Zielknoten **N12**.

Routing: `N46 → N4 → N12`



| i   | 2^i | N46 |     | N4  |     | N12 |
| --- | --- | --- | --- | --- | --- | --- |
| 0   | 1   | 51  |     | 12  |     | 16  |
| 1   | 2   | 51  |     | 12  |     | 16  |
| 2   | 4   | 51  |     | 12  |     | 16  |
| 3   | 8   | 58  |     | 12  |     | 20  |
| 4   | 16  | 4   |     | 20  |     | 28  |
| 5   | 32  | 16  |     | 42  |     | 46  |



## 4. Chord : NK - 2025

**Vorgekommen in:** 16.09.2025 A6


![[Screenshot 2026-09-19 at 08.50.37.png|781]]
#### Antwort

**N6**



![[Screenshot 2026-09-19 at 08.50.52.png|783]]
#### Antwort

`N48 → N51 → N56 → N62 → N6 → N14`



![[Screenshot 2026-09-19 at 08.51.08.png|788]]
#### Antwort

Zielknoten: **N32**

Routing: `N48 → N22 → N31 → N32`

Fingertabelle N48:

| i   | 2^  | N48 |     | N22 |     | N31 |     |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 0   | 1   | N51 |     | N31 |     | N32 |     |
| 1   | 2   | N51 |     | N31 |     | N48 |     |
| 2   | 4   | N56 |     | N31 |     | N48 |     |
| 3   | 8   | N56 |     | N31 |     | N48 |     |
| 4   | 16  | N6  |     | N48 |     | N48 |     |
| 5   | 32  | N22 |     | N56 |     | N6  |     |


## 5. Chord : NK - 2024

**Vorgekommen in:** 17.09.2024 A6


![[Screenshot 2026-09-19 at 08.51.50.png|704]]
#### Antwort

**N58**



![[Screenshot 2026-09-19 at 08.55.10.png|728]]
#### Antwort

`N51 → N58 → N4 → N12 → N16`



![[Screenshot 2026-09-19 at 08.55.29.png|736]]
#### Antwort

Zielknoten: **N27**

Routing: `N46 → N16 → N20 → N27`


| i   | 2^i | N46 |     | N16 |     | N20 |
| --- | --- | --- | --- | --- | --- | --- |
| 0   | 1   | N51 |     | N20 |     | N27 |
| 1   | 2   | N51 |     | N20 |     | N27 |
| 2   | 4   | N51 |     | N20 |     | N27 |
| 3   | 8   | N58 |     | N27 |     | N28 |
| 4   | 16  | N4  |     | N32 |     | N42 |
| 5   | 32  | N16 |     | N51 |     | N58 |


## 6. Chord : HK - 2024

**Vorgekommen in:** 23.07.2024 A6

![[Screenshot 2026-09-19 at 08.56.31.png|699]]

#### Antwort

**N4**

```text
b) Welche Knoten werden angefragt, wenn die Suche mit dem naiven
Routing-Algorithmus realisiert ist und der Knoten N4 nach dem
Objekt mit der ID 28 sucht. (2 P.)
```

#### Antwort

`N4 → N12 → N16 → N20 → N27 → N28`

```text
c) Erläutern Sie, wie die Suche nach dem Objekt mit der ID 47 vom
Knoten N4 aus mit dem skalierbaren Routing-Algorithmus abläuft.
Geben Sie die Fingertabellen aller besuchten Knoten an
(den Zielknoten ausgeschlossen). (8 P.)
```

#### Antwort

Zielknoten: **N51**

Routing: `N4 → N42 → N46 → N51`

Fingertabelle N4:

| i | NodeID |
|---|---|
| 0 | N12 |
| 1 | N12 |
| 2 | N12 |
| 3 | N12 |
| 4 | N20 |
| 5 | N42 |

Fingertabelle N42:

| i | NodeID |
|---|---|
| 0 | N46 |
| 1 | N46 |
| 2 | N46 |
| 3 | N51 |
| 4 | N58 |
| 5 | N12 |

Fingertabelle N46:

| i | NodeID |
|---|---|
| 0 | N51 |
| 1 | N51 |
| 2 | N51 |
| 3 | N58 |
| 4 | N4 |
| 5 | N16 |


<br>

## 7. Chord : NK - 2023

**Vorgekommen in:** 21.09.2023 A7

![[Screenshot 2026-09-19 at 08.58.04.png|631]]
#### Antwort

**N4**



![[Screenshot 2026-09-19 at 08.58.28.png|729]]
#### Antwort

`N4 → N10 → N20 → N21 → N29`

Zuständig: **N29**



![[Screenshot 2026-09-19 at 08.58.58.png|742]]
#### Antwort

| i | NodeID |
|---|---|
| 0 | N32 |
| 1 | N32 |
| 2 | N37 |
| 3 | N37 |
| 4 | N48 |
| 5 | N4 |



![[Screenshot 2026-09-19 at 08.59.19.png|739]]
#### Antwort

Zielknoten: **N56**

Routing: `N4 → N37 → N48 → N56`

Fingertabelle N4:

| i | NodeID |
|---|---|
| 0 | N10 |
| 1 | N10 |
| 2 | N10 |
| 3 | N20 |
| 4 | N20 |
| 5 | N37 |

Fingertabelle N37:

| i | NodeID |
|---|---|
| 0 | N40 |
| 1 | N40 |
| 2 | N48 |
| 3 | N48 |
| 4 | N56 |
| 5 | N10 |

Fingertabelle N48:

| i | NodeID |
|---|---|
| 0 | N56 |
| 1 | N56 |
| 2 | N56 |
| 3 | N56 |
| 4 | N4 |
| 5 | N20 |


<br>

## 8.Chord : HK - 2023

**Vorgekommen in:** 19.07.2023 A6



![[Screenshot 2026-09-19 at 09.00.35.png|731]]
#### Antwort

**N2**


![[Screenshot 2026-09-19 at 09.01.53.png|750]]

#### Antwort

`N2 → N6 → N12 → N20 → N27 → N28 → N32`

Zuständig: **N32**




![[Screenshot 2026-09-19 at 09.01.45.png|768]]
#### Antwort

| i | NodeID |
|---|---|
| 0 | N27 |
| 1 | N27 |
| 2 | N27 |
| 3 | N28 |
| 4 | N40 |
| 5 | N58 |




![[Screenshot 2026-09-19 at 09.02.23.png|809]]
#### Antwort

Zielknoten: **N58**

Routing: `N2 → N40 → N51 → N58`

Fingertabelle N2:

| i | NodeID |
|---|---|
| 0 | N6 |
| 1 | N6 |
| 2 | N6 |
| 3 | N12 |
| 4 | N20 |
| 5 | N40 |

Fingertabelle N40:

| i | NodeID |
|---|---|
| 0 | N51 |
| 1 | N51 |
| 2 | N51 |
| 3 | N51 |
| 4 | N58 |
| 5 | N12 |

Fingertabelle N51:

| i | NodeID |
|---|---|
| 0 | N58 |
| 1 | N58 |
| 2 | N58 |
| 3 | N2 |
| 4 | N6 |
| 5 | N20 |


<br><br>

---



# ==K12: Amazon Dynamo / Key-Value Storage (31 P)

## 1. Virtual Nodes

![[Pasted image 20260830132157.png|767]]

**Vorgekommen in:** 16.09.2025 A7a; 19.07.2023 A7a


### Originalfrage aus der Klausur

![[Pasted image 20260920145100.png|782]]
<br>

### Antwort


**Ziel:**

- Bessere Lastverteilung.
- Berücksichtigung der unterschiedlichen Leistungsfähigkeit der Rechner.

**Funktionsweise:**

- Jeder physische Rechner übernimmt mehrere virtuelle Knoten an verschiedenen Positionen im Hash-Ring.
- Dadurch verwaltet er mehrere Teilbereiche.
- Leistungsfähigere Rechner erhalten mehr virtuelle Knoten.

<br>

## 2. Dynamo-Replikation im 3-Bit-Ring

**Vorgekommen in:** 17.09.2024 A7a; 19.07.2023 A7b

![[Pasted image 20260920145138.png|787]]

### Vorgehensweise / Lösungsschritte


Im Uhrzeigersinn Replikate wählen; virtuelle Nodes desselben physischen Servers überspringen.

<br>

### Antwort


Replikate: **Node 0, 1 und 3**. Node 2 wird übersprungen, weil er zum selben physischen Server wie Node 0 gehört.

<br>

## 3. Vektorzeitstempel in Dynamo

**Vorgekommen in:** 16.09.2025 A7b; 17.09.2024 A7b; 23.07.2024 A1b; 19.07.2023 A7c


### Originalfrage aus der Klausur

![[Pasted image 20260920145213.png|830]]
<br>

### Antwort

- Zweck: Versionen vergleichen und Konflikte erkennen.
- Unterschied: Dynamo begrenzt/verkürzt Vektoren.
- Nachteil: selten kann ein Konflikt dadurch übersehen werden.


***OR:

- **Einsatzzweck:**  Dynamo erkennt damit, welche Version neuer ist und ob es Konflikte gibt.
- **Unterschied:**  Dynamo begrenzt die Größe des Vektors und löscht die ältesten Einträge.
- **Nachteil:**  Dabei gehen Informationen verloren. Deshalb ist nicht immer klar, welche Version aus welcher entstanden ist.


## 4. Dynamo-Quorum: N, W, R

**Vorgekommen in:** 16.09.2025 A7c; 17.09.2024 A7c


![[Pasted image 20260920145242.png|646]]
<br>

### Antwort


- `N` = Replikate.
- `W` = nötige Write-Antworten.
- `R` = gelesene Replikate.

- `W + R > N` → Read- und Write-Quorum überlappen sich.



***OR:
- **N:** Anzahl aller Replikate eines Datenobjekts.
- **W:** Anzahl der Server, die einen Schreibvorgang bestätigen müssen.
- **R:** Anzahl der Server, die bei einem Lesevorgang antworten müssen.

- **Warum?** Damit sich Lese- und Schreibgruppe auf mindestens einem Server überschneiden.
          So kann beim Lesen die aktualisierte Version gefunden werden.

---



# ==K13: Google File System (12 P)

## 1. Aufgaben des GFS-Masters und Entlastung

**Vorgekommen in:** 17.09.2024 A4a

![[Pasted image 20260831193100.png|514]]



![[Pasted image 20260920202528.png|742]]

### Antwort


- Master: Metadaten/Chunk-Zuordnung, Replikate überwachen.
- Entlastung: große Chunks + Client-Caching; Daten gehen direkt zu Chunkservern.

***OR:

- **Aufgabe 1:** Der Master verwaltet Metadaten, zum Beispiel Dateinamen und Speicherorte der Chunks.
- **Aufgabe 2:** Er verwaltet die Replikation und sorgt für neue Kopien bei Ausfällen.
    
- **Entlastung:** Clients übertragen Daten direkt zu und von den Chunkservern. Die Dateidaten laufen nicht über den Master.
<br>

## 2. Schreiben in eine GFS-Datei

**Vorgekommen in:** 17.09.2024 A4b


![[Pasted image 20260920202608.png|911]]


### Antwort


1. **Anfrage:** Der Client fragt den Master, wo die Replikate liegen.
2. **Antwort:** Der Master nennt die Adressen und die Primary.
3. **Daten senden:** Der Client sendet die Daten über eine Pipeline an alle Replikate. Alle bestätigen den Empfang.
4. **Schreibauftrag:** Der Client gibt der Primary den Auftrag, die Daten zu schreiben.
5. **Schreiben:** Die Primary bestimmt die Reihenfolge und schreibt die Daten. Die Secondaries schreiben in derselben Reihenfolge.
6. **Bestätigung:** Die Secondaries bestätigen der Primary, dass sie die Daten geschrieben haben.
7. **Ergebnis:** Die Primary meldet dem Client Erfolg oder einen Fehler.



![[Pasted image 20260920211304.png]]

<br>

## 3. GFS Record Append

**Vorgekommen in:** 17.09.2024 A4c; 23.07.2024 A1c


![[Pasted image 20260920202632.png|856]]
### Antwort


- Atomisches Append, aber **mindestens einmal**.
- Duplikate/Padding möglich.
- Anwendung erkennt Duplikate z. B. über eindeutige IDs.


***OR
- **Semantik:** Ein Record wird atomar mindestens einmal angehängt. GFS bestimmt die Position.
- **Inkonsistenz:** Durch Wiederholungen können Replikate unterschiedlich viele Kopien desselben Records enthalten.
- **Lösung:** Jeder Record bekommt eine eindeutige ID. Die Anwendung erkennt doppelte IDs und ignoriert die Duplikate



---

# ==K14: Transaktionen (34 P)


## 1. Deadlock - Multiple Choice

**Vorgekommen in:** 19.07.2023 A1d


![[Pasted image 20260921150557.png|943]]

### Antwort

**Richtig:** 
 - Die beteiligten Threads bleiben blockiert.

<br>

## 2. ACID

**Vorgekommen in:** 21.09.2023 A6a; 19.07.2023 A4a


![[Pasted image 20260921150712.png|874]]
<br>

### Antwort
| Abkz.   | Ausgeschrieben               | Eigenschaft                                                                    |
| ------- | ---------------------------- | ------------------------------------------------------------------------------ |
| ***A*** | Atomicity (Atomarität)       | Alle Änderungen oder keine.                                                    |
| ***C*** | Consistency (Konsistenz)     | Die Daten bleiben gültig und halten alle Regeln ein.                           |
| ***I*** | Isolation                    | Gleichzeitige Transaktionen wirken wie nacheinander ausgeführte Transaktionen. |
| ***D*** | Durability (Dauerhaftigkeit) | Nach dem Commit bleiben Änderungen auch bei einem Ausfall gespeichert.         |




## 3. Zwei-Phasen-Sperrprotokoll: Nachteile

**Vorgekommen in:** 29.07.2026 A9a; 23.07.2024 A2a


![[Pasted image 20260921150747.png|881]]
<br>

### Antwort


- **Deadlocks**.
- **Cascading Aborts**.



- Deadlocks sind möglich: Transaktionen warten gegenseitig auf ihre Sperren.
- Lange Sperrzeiten führen zu Wartezeiten und weniger Parallelität.

<br>

## 4. Vermeidung der 2PL-Probleme

**Vorgekommen in:** 23.07.2024 A2a


![[Pasted image 20260921150825.png|879]]

<br>

### Antwort

- Deadlocks: Transaktionen warten gegenseitig auf ihre Sperren. 
  ***Lösung:*** Konservatives 2PL. Alle benötigten Sperren werden
  vor Beginn zusammen erworben – oder keine.

- Kaskadierende Abbrüche: Der Abbruch einer Transaktion
  führt zum Abbruch anderer, die ihre Änderungen gelesen haben.
  ***Lösung:*** Striktes 2PL. Schreibsperren bleiben bis zum Commit oder Abort erhalten.
<br>

## 5. Wann optimistisch statt pessimistisch?

**Vorgekommen in:** 29.07.2026 A9b; 23.07.2024 A2b


![[Pasted image 20260921150847.png]]

<br>

### Antwort


Wenn **Konflikte selten** sind und Transaktionen zurückgesetzt werden können.

***OR:***

- Optimistische Synchronisierung ist besser, wenn Konflikte selten sind, zum Beispiel bei vielen Lesezugriffen und wenigen Änderungen.

- Es entsteht weniger Aufwand durch Sperren und Wartezeiten. Da Konflikte selten sind, müssen nur wenige Transaktionen abgebrochen und wiederholt werden.

<br>

## 6. Vorwärtsvalidierung - T4, Variante 2026/2024

**Vorgekommen in:** 29.07.2026 A9c; 23.07.2024 A2c


### Originalfrage aus der Klausur

![[Pasted image 20260901125147.png]]

### Vorgehensweise / Lösungsschritte


`active_set` bestimmen → für jedes aktive Tj: `r(Tj) ∩ w(T4)` prüfen.

<br>

### Antwort


- `active_set={T1,T3}`, `w(T4)={y}`.
- T1: `{x,y}∩{y}={y}` → **Konflikt**.
- T3: `{x}∩{y}=∅` → kein Konflikt.
- Ergebnis: **T4 konflikt mit T1**.

<br>![[Screenshot 2026-09-21 at 21.42.46.png]]

## 7. Vorwärtsvalidierung - T4, 21.09.2023

**Vorgekommen in:** 21.09.2023 A6b

![[Pasted image 20260921151051.png|848]]

### Vorgehensweise / Lösungsschritte


`active_set` bestimmen → `r(Tj) ∩ w(T4)` prüfen.

<br>

### Antwort


`w(T4)=∅` → alle Schnitte leer → **kein Konflikt**.

<br>

## 8. Rückwärtsvalidierung - T1, 19.07.2023

**Vorgekommen in:** 19.07.2023 A4b


![[Pasted image 20260921151126.png|894]]

### Antwort


`finished_set={T2}`; `r(T1)={x1,x2}`, `w(T2)={x1}` → Schnitt `{x1}` → **Konflikt, T1 abbrechen**.

Die Prüfung erfolgt am Ende von T1.

- finished_set = {T2, T4}
- T2 und T4 sind während der Laufzeit von T1 fertig geworden.
- T3 ist noch aktiv und wird nicht geprüft.

- r_set(T1) = {x1, x2}
- w_set(T2) = {x1}
- w_set(T4) = ∅

Konflikt mit T2:
r_set(T1) ∩ w_set(T2) = {x1} ≠ ∅.
T2 hat x1 geändert, das T1 gelesen hat.

Kein Konflikt mit T4:
r_set(T1) ∩ w_set(T4) = ∅.
T4 liest nur und ändert keine Daten.

Die Validierung von T1 schlägt fehl.
T1 muss abgebrochen werden.

![[Pasted image 20260921220501.png]]

---

# ==K15: Konsensus (58 P)

## 1. FLP-Theorem

**Vorgekommen in:** 17.09.2024 A9a; 23.07.2024 A4c; 21.09.2023 A5d

![[Pasted image 20260921151207.png|901]]

### Antwort


- FLP-Theorem:
  In einem asynchronen System mit möglichen Prozessausfällen gibt es keinen total korrekten deterministischen Einigungsalgorithmus.

- Auswirkung auf Paxos:
  Paxos ist partiell korrekt: Wenn der Algorithmus terminiert, ist das Ergebnis korrekt. Die Terminierung ist aber nicht garantiert.


## 2. Welches Problem löst Paxos?

**Vorgekommen in:** 19.07.2023 A8a


![[Pasted image 20260921151251.png|822]]

### Antwort


Paxos erreicht **Konsensus über einen Wert**, solange ein Quorum erreichbar ist.

<br>

## 3. Paxos-Rollen

**Vorgekommen in:** 19.07.2023 A8b


![[Pasted image 20260921151310.png|850]]
<br>

### Antwort

| Rolle       | Aufgabe                    |
| ----------- | -------------------------- |
| Proposer    | Wert vorschlagen           |
| Coordinator | Wahl steuern               |
| Acceptor    | Wahl abstimmen + speichern |
| Learner     | gewählten Wert erhalten    |

<br>

## 4. Ballot Numbers

**Vorgekommen in:** 19.07.2023 A8c


![[Pasted image 20260921152115.png|789]]

### Antwort
- Ballot Numbers unterscheiden die einzelnen Abstimmungsrunden. Sie sind eindeutig und aufsteigend.

- Bei konkurrierenden Koordinatoren hat die höhere Ballot Number Vorrang. Nach einem Promise für diese Runde werden Vorschläge
  mit kleineren Nummern nicht mehr akzeptiert.


## 5. Erfolgreicher erster Paxos-Durchlauf

**Vorgekommen in:** 29.07.2026 A8a; 19.07.2023 A8d


![[Pasted image 20260921152141.png|783]]

### Antwort


`propose → prepare → promise → accept → ack → learn`. Ein Quorum reicht.


![[Pasted image 20260902123352.png|1012]]

<br>![[Screenshot 2026-09-22 at 06.53.05.png|1011]]



1. P → C: propose(v)
   Der Proposer schlägt den Wert v vor.

2. C → A: prepare(b)
   Der Coordinator startet die Runde b.

3. A → C: promise(b,0)
   Die Acceptors versprechen, keine kleineren Runden
   mehr anzunehmen. Es gibt keinen früher akzeptierten Wert.

4. C → A: accept(b,v)
   Nach einem Quorum von Promises fordert der Coordinator
   die Acceptors auf, v anzunehmen.

5. A → C: ack(b)
   Die Acceptors bestätigen die Annahme.

6. C → L: decided(v)
   Nach einem Quorum von ACKs informiert der Coordinator
   den Learner über den beschlossenen Wert v.


## 6. Paxos mit Ausfall von Acceptor 2

**Vorgekommen in:** 16.09.2025 A9a

![[Pasted image 20260921152229.png|982]]


### Antwort

Antwort: Ja, eine Einigung auf v=42 ist möglich.

Begründung: Acceptor 1 und 3 bilden ein Quorum: 2 von 3 Acceptors. Ihre Bestätigungen reichen aus.


![[Pasted image 20260922103553.png]]

<br>

## 7. Paxos nach Koordinatorausfall

**Vorgekommen in:** 17.09.2024 A9b


### Originalfrage aus der Klausur

![[Pasted image 20260902120039.png|859]]


### Vorgehensweise / Lösungsschritte


Höhere Ballot Number wählen → Quorum fragen → höchsten bereits akzeptierten Wert übernehmen.

<br>

### Antwort


Neuer Coordinator nimmt höhere Ballot Number und muss den bereits akzeptierten Wert **42** weiterverwenden → Learner erhalten **42**.

![[Pasted image 20260922105209.png]]


## 8. Byzantinische Generäle: 4 Teilnehmer, Anführer ist Verrater

**Vorgekommen in:** 29.07.2026 A8b; 16.09.2025 A9b


![[Pasted image 20260921152512.png|753]]

### Vorgehensweise / Lösungsschritte


Nachrichten austauschen → jeder korrekte Knoten bildet Mehrheit.

<br>

### Antwort


Ja. Bei **4 Knoten und 1 Verräter** ist Konsens möglich (`n ≥ 3m+1`). Die korrekten Knoten entscheiden per Mehrheit.





<br>

## 9. Byzantinische Generäle: nur 3 Teilnehmer

**Vorgekommen in:** 23.07.2024 A4a

![[Pasted image 20260921152608.png|689]]
### Vorgehensweise / Lösungsschritte


Bedingung prüfen: `n ≥ 3m+1`.

<br>

### Antwort


**Nein.** Für einen Verräter braucht man mindestens **4 Knoten**.

<br>

## 10. Byzantinische Generäle: 4 Teilnehmer, C ist Verrater

**Vorgekommen in:** 23.07.2024 A4b


![[Pasted image 20260921152640.png|716]]
### Vorgehensweise / Lösungsschritte


Alle tauschen Werte aus → korrekte Knoten bilden Mehrheit.

<br>

### Antwort


**Ja.** Mit 4 Knoten kann 1 byzantinischer Fehler maskiert werden; die korrekten Knoten entscheiden **1**.

<br><br>

---



# ==K16: Sicherheit (46 P)


## 1. Symmetrische und asymmetrische Verschlüsselung

**Vorgekommen in:** 29.07.2026 A7a; 23.07.2024 A8a; 21.09.2023 A8b; 19.07.2023 A5b

![[Pasted image 20260921152744.png|668]]
<br>

### Antwort



- **Symmetrisch:** gleicher geheimer Schlüssel; schnell.
- **Asymmetrisch:** Public/Private Key; langsamer, kein gemeinsames Geheimnis vorher nötig.

<br>

## 2. Gegenseitige Authentisierung + Sitzungsschlüssel

**Vorgekommen in:** 29.07.2026 A7b; 16.09.2025 A8b; 17.09.2024 A8c; 23.07.2024 A8b


![[Pasted image 20260921152814.png|702]]

### Vorgehensweise / Lösungsschritte


Challenge `RA` → Antwort mit `RA, RB, KAB` → Bestätigung von `RB` mit `KAB`.

<br>

### Antwort


`A→B: E(KB+, A, RA)`

`B→A: E(KA+, RA, RB, KAB)`

`A→B: E(KAB, RB)`

Danach Kommunikation mit `KAB`.

![Skizze](auth_asym.svg)



<br>

## 3. Warum funktioniert ARP-Spoofing?

**Vorgekommen in:** 19.07.2023 A5a



![[Pasted image 20260921153012.png|903]]

<br>

### Antwort


ARP hat **keine Authentifizierung**. Ein Angreifer kann daher falsche IP-MAC-Zuordnungen senden.

<br>

## 4. Man-in-the-Middle per ARP-Spoofing

**Vorgekommen in:** 29.07.2026 A7c


![[Pasted image 20260921153045.png|759]]

### Vorgehensweise / Lösungsschritte


ARP-Cache vergiften → Verkehr umleiten → Identität mit Zertifikat prüfen.

<br>

### Antwort


Angreifer fälscht ARP-Antworten → Verkehr von A und B läuft über ihn. **Gegenmaßnahme:** Identität kryptografisch prüfen, z. B. Zertifikat.

<br>

## 5. Digitale Signatur / veränderte E-Mail erkennen

**Vorgekommen in:** 16.09.2025 A8a; 17.09.2024 A8a; 21.09.2023 A8c; 19.07.2023 A5c


![[Pasted image 20260921153117.png|902]]

### Vorgehensweise / Lösungsschritte


Hash bilden → mit Private Key signieren → mit Public Key prüfen.

<br>

### Antwort


Sender: `Hash(Nachricht)` mit **Private Key signieren**. Empfänger prüft Signatur mit **Public Key** und vergleicht den Hash.

<br>

## 6. Zertifikat: Gültigkeit und Verifikation

**Vorgekommen in:** 21.09.2023 A8a


![[Pasted image 20260921153222.png|726]]

<br>

### Antwort


Zertifikat = Public Key + Identität, von einer **CA digital signiert**. Prüfung mit CA-Public-Key/Zertifikatskette.

<br>

## 7. Warum verhindert ein gültiges Zertifikat theoretisch MITM?

**Vorgekommen in:** 17.09.2024 A8b; 21.09.2023 A8d


![[Pasted image 20260921153302.png|929]]
<br>

### Antwort


Der Angreifer kann kein gültiges Zertifikat für seinen falschen Public Key vorlegen.

<br>

## 8. Shared Key wird veröffentlicht

**Vorgekommen in:** 21.09.2023 A8e


### Originalfrage aus der Klausur

![[Pasted image 20260921153525.png|846]]
<br>

### Antwort


**Vertraulichkeit und Authentisierung sind verloren**: Jeder mit dem Schlüssel kann entschlüsseln und sich ausgeben.
