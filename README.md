# WinIBW-Suchtipps

## Suche im Katalogisierungssystem (CBS) mit Hilfe der WinIBW

### Grundlagen der Suche in der WinIBW

#### Kombination von Suchergebnissen

Bei umfangreichen Selektionen kann es sinnvoll sein, mehrere Suchanfragen zu kombinieren, anstatt eine einzelne sehr komplexe Suchanfrage zu stellen.

Grundsätzlich wird bei jeder Suchanfrage ein Set gebildet, welches über set<SETNUMMER> bzw. s<SETNUMMER> zu verfügung steht. 
Diese Sets können mit Hilfe von Boolschen Operatoren (und, oder, nicht) kombiniert werden. 

##### Schema

    f s<SETNUMMER> [UND ODER NICHT] s<SETNUMMER> [[UND ODER NICHT] s<SETNUMMER>[...]]
##### Beispiel

    f s1 NICHT s2
    
##### Erläuterung

Finde alle Titel des Sets 1, die nicht im Set 2 vorkommen.

---
 
#### Begriffe, die bei der Suche in Anführungszeichen gesetzt werden müssen

Es kann vorkommen, dass Sie nach Titeln suchen, die teilweise gleiche Wörter enthalten, wie sie vom CBS als Index genutzt werden. Dies führt zu dem Ergebnis, dass nichts gefunden wird, da das CBS ein Wort ihrer Suchanfrage als Index und nicht als Suchwort interpretiert.

##### Beispiel

Gesucht wird der Titel "DOK". 

Ihre Suche 

    f tit DOK

führt zu keinem Ergebnis, da 'DOK' ein Index im CBS ist. 

Um den Titel zu finden, müssen Sie das Suchwort in Anführungszeichen setzen: 

    f tit "DOK"

##### Hinweis

Hier finden Sie eine Liste aller Stichwörter (als Word-Datei), die bei der Suche in Anführungszeichen zu setzen sind. 
 
#### Suchwörter mit Schrägstrich

##### Beispiel 

    f sg 188/e

führt zu keinem Ergebnis, da der Schrägstrich intern als Steuerzeichen interpretiert wird. 

Richtig ist daher 

    f sg 188\/e

oder auch 

    f sg 188 e

---

#### Runde Klammern

Runde Klammern () erfüllen in der Suchanfrage verschiedene Rollen. Im Zusammenhang mit einem Index können sie helfen, die Suchanfrage zu verschlanken: 

##### Beispiel 1 

    f dcz 500 nicht 510 oder 520 nicht 510 oder 530 nicht 510 oder 540 nicht 510

Besser kann geschrieben werden 

    f dcz (500 oder 520 oder 530 oder 540) nicht 510

Beide Suchanfragen führen zur gleichen Ergebnismenge. Dabei ist die zweite Suchanfrage sehr viel kompakter. 

##### Beispiel 2 

Finde alle Titel mit der DDC-Notation 500 oder 600, die aber nicht die DDC-Notation 610 besitzen. 

Falsch: 

    f dcz 500 oder 600 nicht 610

Die Anfrage findet aber auch die Titel, welche die DDC-Notation 500 und 610 gleichzeitig haben. 

Richtig: 

    f dcz (500 oder 600) nicht 610

Wie in der Mathematik werden die Klammern zuerst "berechnet". D.h. es werden zuerst alle Titel mit der DDC-Notation 500 oder 600 gesucht. Die resultierende Treffermenge wird dann und auf diejenigen Titel eingeschränkt, welche nicht die DDC-Noation 610 besitzen. 

Man könnte die gleiche Treffermege auch folgendermaßen ermitteln: 

    f dcz 500 nicht 610
    f dcz 600 nicht 610
    f s1 oder s2

---

#### Eckige Klammern

Eckige Klammern erlauben bei der Suchanfrage einen Zeichenbereich anzugeben. Dies kann eine Suchanfrage sehr verschlanken. 

##### Beispiel: 

    f bcz  500 oder 510 oder 520 oder 530 oder 540 oder 550 oder 560 oder 570 oder 580 oder 590 

Besser kann geschrieben werden 

    f bcz 5[0-9]0

Dies kann besonders sinnvoll sein, wenn sehr große Bereiche abgefragt werden sollen. Dann kann anstatt des Aufzählens der Zeichen in den eckigen Klammern auch ein Bis-Strich eingefügt werden ([a-z] oder [0-9]). 
Buchstaben, Zahlen und Satzzeichen können auch in eckigen Klammern als Zeichenbereich zusammengefasst werden: [a-z0-9,.?!@] 

---

#### Besondere Zeichen: Trunkierung und Platzhalter

Oft sind Suchworte bei der Suche nur teilweise bekannt oder es werden Titel gesucht, die ein Suchwort in verschiedenen Formen enthalten. 

Um zu verhindern, dass nach allen möglichen Formen der Suchörter gesucht werden muss, wird am Ende des Suchwort-Stamm das Zeichen '?' oder '*' als Trunkierung angefügt. 

##### Beispiel 

    f tit histori?

Diese Suche findet alle Titel mit dem Stichwort-Stamm 'hsitori'. Also zum ##### Beispiel 'historisch', 'historische', 'historic', 'historical', 'historia' oder 'historique'. 

Wird die Trunkierung nicht rechts am Wort sondern im Wort angewendet, so dient es als Platzhalter für beliebig viele Zeichen. 

##### Beispiel 

    f tit histori?e

Diese Suche findet alle Titel mit den Suchchwörtern 'historische', 'historique' etc. 

Anstelle der Trunkierungszeichen '?' oder '*' kann auch ein Ausrufezeichen als Platzhalter eingesetzt werden. Das Ausrufezeichen dient dann als Platzhalter für ein beliebiges Zeichen. 

##### Beispiel

    f tit histor!

Diese Suche findet alle Titel mit den Stichwörtern 'history', 'histora' etc. nIcht aber Titel mit Stichwörtern, die nach dem Stichwort-Satmm mehr als ein Zeichen haben (z.B. 'historia'). 

### Spezielle Suchanfragen
 
#### Wie kann man ermitteln, ob ein Feld in einem Lokalsatz nicht vorkommt?

Hintergrund der Fragestellung ist eine Fachbibliothek, die alle Titel ermitteln möchte, bei den sie Bestände hat, aber z.B. das Signaturenfeld (7100) nicht besetzt ist. 

##### Schema

Set 1

    f sg <SIGEL DER FACHBIBLIOTHEK>

Set 2

    f sig [0123456789abcdefghijklmnopqrstuvwxyz]?

Set 3 

    f s1 NOT s2

##### Erläuterung

Set 1 bildet alle Titel ab, bei denen die Fachbibliothek Bestände hat. Set 2 selektiert alle Titel, bei denen die Fachbibliothek oder andere Bibliotheken der gleichen ILN Bestände hat, in denen in 7100 Signaturen vorhanden sind, die mit Zahlen oder Buchstaben beginnen. Set 3 kombiniert Set 1 und Set 2. Es werden nur die Titel abgebildet, bei denen die Fachbibliothek Bestände hat, bei denen aber kein Bestand 7100 besetzt hat.

---

#### Wie können unabhängig vom LogIn Bestände einer beliebigen Bibliothek gefunden werden?

Normalerweise können nur die Titel mit Beständen der eigenen Bibliothek über die Suche mit dem Sigel/ISIL gefunden werden. Der Index 'bie' erlaubt aber auch diese Suche für andere Bibliotheken.

##### Schema

    f bie <IDN DES BIBLIOTHEKSSATZES>

##### Beispiel

    f bie 009001506

##### Erläuterung

Die Suche im Beispiel findet alle Titel, an denen die Bibliothek mit der IDN 009001506 Bestand hat. 

### GravKorr - Schnelleinstieg
 
Dieser Abschnitt soll als Schnelleinstieg für den Umgang mit Gravierenden Korrekturen (GravKorr) dienen. Für eine ausführliche Erläuterung lesen Sie bitte die Einführung Gravierende Korrekturen.

Zumeist werden in bestimmten Intervallen (z.B. monatlich) GravKorr-Listen erstellt. Diese werden dann entweder in einer Datei abgelegt oder ausgedruckt, um damit zu arbeiten. Dies soll eine kurze Einführung in das Vorgehen geben.

Was brauchen Sie?

1. Ihre ILN
2. Den Zeitraum, für den Sie die GravKorr-Liste erstellen wollen

Zu 1.:

Ihre ILN finden Sie, indem Sie den Bibliothekssatz (Tw-Satz) ihrer Bibliothek aufrufen (suchen Sie diesen mit Ihrem ISIL oder Sigel). In der Kategorie 051 steht Ihre ILN.

##### Beispiel für die Bibliothek der HU-Berlin

    f sg 11

Die ILN der Bibliothek der HU-Berlin ist dann: 0019.

Zu 2.:

Den Zeitraum für die GravKorr-Listen erstellen Sie durch das Ersetzen von Datumsteilen durch Ausrufezeichen "!". Ein Datum hat das Format "JJ-MM-TT".

##### Beispiel für einen Zeitraum von einem Monat (Juli 2012)

    12-07-!!

##### Beispiel für einen Zeitraum von einem Jahr (2012)

    12-!!-!!

Jetzt haben Sie alles zusammen. Fügen Sie ILN und Zeitraum in diese Abfrage ein: 

    f sta <IHR ZEITRAUM> g und mk <IHRE ILN>

##### Beispiel für die Bibliothek der HU-Berlin für den Juli 2012

    f sta 12-07-!! g und mk 0019

Die entstandene Treffermenge können Sie dann entweder speichern: 

dow s<SETNUMMER IHRER GRAVKORR-LISTE> grav

...oder Sie drucken Sie aus: 

    p s<SETNUMMER IHRER GRAVKORR-LISTE> grav

### Ermittlung von Zahlen bezogen auf den Gesamtbestand

#### Abfrage nach Erscheinungsjahren

Für die Abfrage nach Erscheinungsjahren sind die Indizes

- EJB Anfangsjahr
- EJE Endjahr

eingerichtet. Jeder Indexeintrag besteht aus 4 Zeichen (Ziffern). 

##### Beispiele

Zeitschriften vor 1800 

    f EJB 1[4567]!!

laufende Zeitschriften 

    f EJB [12]!!! not EJE [12]!!!

Zeitschriften, die nach 1993 begonnen haben 

    f EJB 199[456789] or 2!!!

Zeitschriften, die 1955 oder früher begonnen haben und 1955 noch erschienen sind 

    f (EJB 1[45678]!! or 19[01234]! or 195[012345]!) not (EJE 1[45678]!! or 19[01234]! or 195[01234])

Zeitschriften. die in den Jahren 1988 –2000 durchgängig verzeichnet sind 
    f ((EJB 1[45678]!! or 19[012345678]!) not EJB 1989) not EJE 1!!!

##### Erläuterung

Die Abfragen fallen im Einzelfall etwas komplizierter aus, da bisher nicht mit größer / kleiner – Anfragen gearbeitet werden kann.

Es wird unterstellt, dass Zeitschriften erst seit dem 15. Jh. erscheinen.

Die Operatoren ! und […] bedeuten wie allgemein in der Retrieval-Syntax:

- wird der Operator ! als Platzhalter verwendet, so muss anstelle dieses Platzhalters im Wort 1 Zeichen stehen.
- in den eckigen Klammern werden alle Zeichen aufgelistet, die an dieser Stelle vorkommen dürfen.

*Achtung:* Derzeit können wir die Benutzung dieser Indexe nur bei kleineren Datenmengen empfehlen, da es bei größeren Datenmengen noch zu fehlerhaften Ergebnissen kommen kann!!! (Beispiel: Die o. a. Suchanfrage nach laufenden Zeitschriften)

---

#### Finde alle fortlaufenden Titel

##### Beispiel 

    f ejb [12]!!! not eje [12]!!!

##### Erläuterung 

Finde alle Titel mit dem Anfangsjahr später als 999, die kein Endjahr später als 999 haben. 

---

#### Alle Titel zu den Fächern X und Y

##### Schema 

    f nsa (<DDC-NOTATION> oder <DDC-NOTATION> [oder <DDC-NOTATION> [...]])

##### Beispiel 

    f nsa (300 oder 320)

##### Erläuterung 

Finde alle Titel, die die DDC-Notation 300 (Politikwissenschaften) oder 320 (Sozialwissenschaften, Soziologie, Anthropologie) besitzen. 

---

### Ermittlung von Zahlen für einzelne Einrichtungen
 
 
Erfahrungsgemäß sind am Anfang eines neuen Jahres oder am Ende eines Projektes Zahlen von den in der ZDB neu erstellten oder korrigierten Titeln /  Beständen (Exemplardaten) gewünscht. Nachfolgend werden anhand von Beispielen grundlegende Suchanfragen zur Ermittlung dieser Zahlen erläutert. Die aufgeführten Beispiele beziehen sich insbesondere auf das Jahr 2014 und können durch andere Datumsangaben natürlich entsprechend angepasst werden.

#### Gesamtzahl Titel

##### Schema 

    f sg <BIBLIOTHEKSSIGEL>

##### Beispiel 

    f sg 364

##### Erläuterung 

Es werden alle Titel ermittelt, die mit Exemplardatensätze der Martin-Opitz-Bibliothek (Sigel 364) verknüpft sind. 

---

#### Finde alle fortlaufenden Titel einer Einrichtung

##### Schema 

    f lfd <BIBLIOTHEKSSIGEL>

##### Beispiel 

    f lfd 929

##### Erläuterung 

Es werden alle Titel ermittelt, die mit Exemplardatensätze der Rheinischen Landesbibliothek (Sigel 929) verknüpft sind und mit forlaufenden Bestand gekennzeichnet sind.

---

#### Finde alle abgeschlossenen Titel einer Einrichtung

Diese Selektion ist eine Kombination der beiden vorherigen Suchanfragen. 

##### Schema 

    f sg <BIBLIOTHEKSSIGEL> NOT lfd <BIBLIOTHEKSSIGEL>

##### Beispiel 

    f sg 929 NOT lfd 929

##### Erläuterung 

Es werden alle Titel ermittelt, die mit Exemplardatensätze der Rheinischen Landesbibliothek (Sigel 929) verknüpft sind und die mit abgeschlossenen Bestand gekennzeichnet sind.

---

#### Elektronische Titel

##### Schema 

    f <BIBLIOGRAPHISCHE GATTUNG, FELD 0500, POS. 1> und sg <BIBLIOTHEKSSIGEL>

##### Beispiel 

    f bbg o* und sg 364

##### Erläuterung 

Es werden alle elektronischen Titel ermittelt, die mit Exemplardatensätze der Martin-Opitz-Bibliothek verknüpft sind. 

##### Schema 

    f <BIBLIOGRAPHISCHE GATTUNG, FELD 0500, POS. 1> und mk <ILN>

##### Beispiel 

    f bbg o* und mk 0182

##### Erläuterung 

Es werden alle elektronischen Titel ermittelt, die mit Exemplardatensätze der ILN 0182 Martin-Opitz-Bibliothek verknüpft sind. 

---

#### Neu angelegte Exemplardatensätze

##### Schema 

    f slk <ZEITRAUM>

##### Beispiel 

    f slk [0123]!-!!-14

##### Erläuterung 

Es werden alle Titel ermittelt, die im Jahr 2014 neu mit Exemplardatensätzen der eigenen ELN (Bearbeiterkennzeichen) verknüpft wurden.

Obwohl die Suchanfrage auf Exemplardatensätze abzielt, wird immer nur die Anzahl der verknüpfte Titel zurückgeliefert; die Anzahl der wirklich vorhandenen Exemplardatensätze wird nicht gezählt. 

---

#### Korrekturen von Exemplardatensätzen

##### Schema 

    f aee <ZEITRAUM>

##### Beispiel 

    f aee [0123]!-!!-14?

##### Erläuterung 

Es werden alle Titel ermittelt, bei denen im Jahr 2014 an mindestens einem Exemplardatensatz der eigenen ELN (Bearbeiterkennzeichen) eine Korrektur erfolgte. 

Obwohl die Suchanfrage auf Exemplardatensätze abzielt, wird immer nur die Anzahl der verknüpfte Titel zurückgeliefert; die Anzahl der wirklich korrigierten Exemplardatensätze wird nicht gezählt.

Bitte beachten Sie ferner, dass das Ergebnis nur eine begrenzte Aussage über die Zahl der korrigierten Exemplardatensätze zulässt, da jeder neu eingegebene Exemplardatensatz zeitgleich mit der Neuerfassung auch ein Korrekturdatum erhält und zusätzlich maschinelle Datenänderungen globaler Art das Ergebnis verfälschen können.

---

#### Löschungen von Exemplardatensätzen

##### Erläuterung

Exemplarlöschungen können nachträglich in der Datenbank nicht mehr ermittelt werden.

#### Neu angelegte Titeldatensätze

##### Schema 

    rec t; f ser <ZEITRAUM> und ser <ELN>

##### Beispiel 

    rec t; f ser [0123]!-!!-14 und ser 4040

##### Erläuterung 

Es werden alle Titel ermittelt, die im Jahr 2014 durch die ELN 4040 (Bearbeiterkennzeichen der Martin-Opitz-Bibliothek) neu angelegt wurden.

---

#### Neu angelegte Körperschaftsdatensätze

##### Schema 

    f ser <ELN> and ser <ZEITRAUM> and bbg t? 

##### Beispiel 

    f ser 4040 and ser [0123]!-!!-14 and bbg t? 

##### Erläuterung 

Es werden alle Körperschaftsdatensätze ermittelt, die im Jahr 2014 durch die ELN 4040 (Bearbeiterkennzeichen der Martin-Opitz-Bibliothek) neu angelegt wurden. 


