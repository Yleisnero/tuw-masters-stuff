# Fragen Programmiersprachen Prüfung

### Behandelte Themen in der Vorlesung
- Syntax & Semantik von Programmiersprachen
- Implementierung von Programmiersprachen
- Datentypen
- Kontrollstrukturen & Exceptions
- Nebenläufigkeit
- Objekt-orientierte & funktionale Programmierung

### Activation Record
Der Activation Record dient als lokales Environment (= stack frame).
Er speichert alle lokalen Variablen und Verwaltungsdaten (z.B. return pointer).

### Dynamic/Static Link
#### Static Link
Zeigt die statische Umgebung einer Funktion an. Zeigt auf den Activation Record der Funktion die die aktuelle Funktion im Programmcode umgibt. Notwendig bei geschachtelten Funktionen.
#### Dynamic Link
Bei Rückkehr aus einer Methode muss auch an die richtige Stelle im Stack gesprungen werden. Der dynamic link enthält den Activation Record zu dem gesprungen werden soll (Anfang des Activation Records der Methode welche die aktuell aufgerufen hatte).

Der dynamic link wird notwendig, falls für eine Routine mehr als ein Activation Record existieren kann (Rekursion). Andernfalls ist ein Return Pointer ausreichend.

### Arten der Parameterübergabe
- **Call-by-Reference**: Die Adresse einer Speicherzelle wird übergeben. Aufgerufene Methode kann in die übergebene Adresse schreiben.
- **Call-by-Copy**: Parameter ist lokale Variable der aufgerufenen Funktion 
    - Call-by-Value: Kopiert R-Value in eine Variable der aufgerufenen Funktion
    - Call-by-Result: Kopiert Ergebnis zurück in die Variable der aufrufenden Funktion
    - Call-by-Value-Result: Kombination aus Call-by-Value & Call-by-Result
- **Call-by-Name**: Jedes Vorkommen des formalen Parameters wird durch den aktuellen Parameter ersetzt.

### Wann ist Call-by-Reference nicht gleich Call-by-Value-Return?
- Wenn zwei formale Parameter Aliases sind
- Wenn ein formaler Parameter und eine Variable, die für beide (Aufrufer und Aufrufer) Aliases sind

## Was berechnet fp(d) und wozu, was ist d?
- d ist der Abstand zwischen dem aktuellen und dem adressierten static scope
- fp(d) berechnet den Frame-Pointer (für Methodenaufruf)

## Welche Sprachen unterstützen geschachtelte Funktionen?
Algol, Pascal, Ada

## Welche Sprachen unterstützen keine geschachtelte Funktionen?
Java, C++, C#, VB, Eiffel

## Warum braucht man in Java keinen static link?
Java unterstützt keine geschachtelten Funktionen. Objekte und Klassen liegen auf dem Heap.

## Wie kann man in Java nested functions simulieren?
- Verwendung von Lambda Ausdrücken (Lambda Ausdrücke erlauben den Zugriff auf Variablen im Scope der umgebenden Funktion)
- Verwendung einer anonymen Klassen, welche genau eine Funktion enthaltet

## Strong and Static Type Systems
- strong type system ensures type consistency
- static type system associates each expression statically with a type
- A statically-typed language is a language (such as Java, C, or C++) where variable types are known at compile time
- A language is dynamically typed if the type is associated with run-time values

Each static type system is also strong, but not every strong type system is static

## Exception-Handling
Der Compiler weißt jeder möglichen Exception einen eindeutigen Namen zu (IP-Adresse). Damit eine Exception behandelt werden kann, ist ein geeigneter Exception-Handler notwendig. Dieser wird von innen (aktueller Block) nach außen gesucht (über dynamic link). Hierbei ist der global eindeutige Name notwendig.

## Welche Möglichkeiten gibt es, Exception-Handling zu implementieren?
- Der Activation Record wird um ein Feld erweitert, welches auf eine statische Exception Handling Tabelle verweist. Wenn in dieser Tabelle kein passender Handler gefunden wird, wird der Activation Record der Methode vom Stack gelöscht und in der aufrufenden Methode weiter gesucht.
Bei jedem Aufruf muss bei diesem Verfahren jedoch der zusätzliche Zeiger geschrieben werden.
- In den Activation Record wird keinerlei zusätzliche Information geschrieben.
Es wird eine globale Exceptions-Liste erstellt. Binärer Suche auf die IP-Adresse um Handler zu finden. Es entstehen nur Kosten wenn eine Exception geworfen wird.

## Was sind checked exceptions?
In Java werden sogenannte checked exceptions verwendet. Der Compiler überprüft, dass keine Exceptions weitergereicht werden, die nicht im Kopf einer Methode deklariert wurden.

## Im neuen C++ ist die Exception Liste in der Signatur deprecated, warum?
Wenn eine exception geworfen wird, die nicht in der Exception Liste steht, wird die exception unexpected() aufgerufen. Diese terminiert das Programm. Das ist meist jedoch nicht wünschenswert.

## Welche Arten von Synchronisationsmechanismen gibt es?
- Semaphoren
- Monitor
- Rendezvous in Ada (vor der Einführung von Monitoren)
- Protected Type (= Monitor) in Ada

## Synchronization mit Hilfe von Semaphoren
Low-level Synchronization-Mechanismus der auf Counters basiert. Es gibt zwei Operationen:
- P(s): if s > 0 then s = s - 1 else suspend current process
- V(s): if process is suspended on s then wake up process else s = s + 1
Vor einer kritischen Section wird P(s) aufgerufen beim Verlassen V(s)

## Synchronization mit Hilfe von Monitoren
Ein Monitor repräsentiert einen kritischen Abschnitt. Ein Monitor ist ein Modul (ein abstrakter Datentyp, eine Klasse), in dem die von Prozessen gemeinsam genutzten Daten und ihre Zugriffsprozeduren (oder Methoden) zu einer Einheit zusammengeführt sind. 
Bei Verwendung eines Monitors muss sich der Programmierer nicht mehr explizit (wie bei Semaphore) um die Synchronisierung kümmern.

## Rendezvous in Ada
Zwischen accept & end findet das Rendezvous statt. Dort läuft der Programmcode synchron ab. Accept statements benutzen vorher definierte messages. Um mehrere messages zu verknüpfen gibt es select when.
