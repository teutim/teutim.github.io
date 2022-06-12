# STP-Dokumentation

<head>
    <meta name='robots' content='noindex,nofollow,noarchive' />
</head>

## Vorwort

Bla

## Zweck

Diese Dokumentation definiert und erläutert STP, anhand von Definitionen, Erklärungen und Beispielen.

## Konforme Implementierung

Eine konforme STP-Implementierung muss das in dieser Dokumentation beschriebene Verhalten von STP nachbilden können.
Sie muss Quelltext entsprechend des aktuellen Unicode-Standards behandeln.
Es ist ihr erlaubt, das hier beschriebene Verhalten zu ergänzen.
Sie darf nicht hier beschriebenes Verhalten umdefinieren.
Alles weitere ergibt sich aus den Beschreibungen in dieser Dokumentation.

## Ausdrücke

1. Ein Ausdruck ist entweder ein Atom oder etwas, was als Liste dargestellt werden kann, deren Elemente wiederum Ausdrücke sind.
2. Ein Atom ist eine Zeichenkette, die einen Text, eine Zahl, einen Wahrheitswert, einen Namen oder ähnliches hinreichend "verlustfrei" darstellen kann. Ein Atom kann keine Unterausdrücke enthalten. Ein Atom ist daher aus Sicht von STP "unteilbar".
3. Ein Atom ist entweder eine Zahlenliteral, ein Bezeichnerliteral oder ein Zeichenkettenliteral.
4. Ein Zahlenliteral hat entweder das Format sz, bz, sz.z oder sz/z, wobei z eine Kette beliebiger Länge von Ziffern ist, b entweder leer, 0b, 0o, 0d oder 0x ist, s entweder leer oder - (Bindestrichminus) ist. Folgende sind unter anderem Zahlenliterale
    1. 0xAFFE (Hexadezimalzahl, Basis 16, dezimal 45054)
    2. 0d1000 (Duodezimalzahl, Basis 12, dezimal 1728)
    3. 0o1234 (Oktalzahl, Basis 8, dezimal 668)
    4. 0b1111 (Binärzahl, Basis 2, dezimal 15)
    5. -10
    6. 987654321
    7. 355/113
    8. 0.75
5. Ein Zeichenkettenliteral besteht aus beliebigen Zeichen, die nicht doppelte Anführungszeichen sein dürfen, die in doppelte Anführungszeichen gesetzt werden. `"..."`, `"Hallo, Welt!"`, `"( Eine kaputte.Liste"`
6. Ein Bezeichnerliteral besteht aus den Zeichen a-b, A-B, 0-9 und dem Bindestrich, wobei das erste Zeichen keine Zahl sein darf.
7. Die Darstellung einer Liste beginnt mit einer öffnenden Klammer und endet mit einer schließenden Klammer, die einzelnen Elemente der Liste werden innerhalb dieser Klammern durch Leerzeichen getrennt. Damit ein Element als solches erkannt wird, muss es zwingend mindestens ein Zeichen enthalten. Daraus folgt, dass `(abc d )` wie `(abc d)` und `( abc d )` interpretiert wird. Jegliche Whitespace-Zeichen werden vor der Interpretation in Leerzeichen umgewandelt. Dadurch Aufeinanderfolgende in ein einzelnes.
8. Eine leere Liste wird mit `()` dargestellt.
9. Jeder Listenausdruck kann ein Präfix und ein Suffix enthalten. Das Präfix ist der Liste vorangestellt und wird als erstes Element in diese eingefügt, sodass: `a(b c)` = `(a b c)` gilt. `a` muss hierbei entweder ein Atom oder eine einzelne Liste sein, um Mehrdeutigkeit zu vermeiden. In dieser Hinsicht mehrdeutige Ausdrücke sind unzulässig. Es gibt für jeden Datentypen eine Funktion gleichen Namens, die die Operanden als eigene Funktion evaluiert und das Ergebnis in den entsprechenden Typ umwandelt, sofern möglich.
10. Der Doppelpunkt `:` sowie das Komma `,` am Ende von Ausdrücken werden ignoriert. Diese sollen jedoch ausschließlich in Map- bzw. Objektausdrücken verwendet werden, um eine bessere Übersicht der Schlüssel-Wert-Paare zu erlauben.
Damit ist
`Map(vorname: "Max", nachname: "Mustermann", geburtsjahr: 1950)`
gleichbedeutend mit
`(Map vorname "Max" nachname "Mustermann" geburtsjahr 1950)`
Es wird sich vorbehalten, diesen Zeichen in diesem Zusammenhang in Zukunft eine tatsächliche syntaktische Bedeutung/Erfordernis zu verleihen.
In allen anderen Zusammenhängen (id est Ausdrücke, die nicht Bestandteil eines Mapkonstruktors sind) darf der Parser Ausdrücke ablehnen, die mit diesen beiden Zeichen enden.

## Blockausdrücke

1. Blöcke sind Ausdrücke, die mit geschweiften statt runden Klammern beginnen und enden.
2. Die Argumente eines Blocks werden von links nach rechts interpretiert, wobei Seiteneffekte früherer Argumente sich auf die Interpretation späterer Argumente auswirken können.
3. Ein Block gibt sein letztes Argument interpretiert zurück.

## Funktionen

1. In der Regel evaluieren Ausdrücke zu sich selbst. Es gibt allerdings Ausdrücke, die eine spezielle Bedeutung haben und eine spezielle Interpretation erzwingen. Funktionen gehören zu den Ausdrücken mit besonderer Bedeutung.
2. Für eine Liste mit mindestens einem Element gilt, dass wenn das erste Element dieser Liste ein Funktionsname ist, die Liste bei der Interpretation einen Funktionsaufruf der entsprechenden Funktion darstellt, wobei die übringen Elemente der Liste die zu übergebenden Parameter darstellen. Bei der Interpretation wird die Liste entsprechend ausgewertet und durch den Rückgabewert der Funktion ersetzt.
3. Im Anhang befindet sich eine Tabelle von direkt unterstützten Funktionen.
4. Jede Funktion kann mit `(call FUNC LISTE)` aufgerufen werden, wobei FUNC eine Funktion ist und LISTE die zu übergebenden Parameter.

## Variablen

Es ist möglich, an einen "Bezeichner" einen Ausdruck zu binden. Dieser Ausdruck kann ein Wert sein. Der Bezeichner ist dann der "Name" des Wertes und steht symbolisch für diesen. Durch erneutes Binden an den Bezeichner wird der Wert faktisch überschrieben, sodass das ganze in Gesamtheit einer Variable gleicht. Auf diese Weise erzeugte Variablen sind "lokal" und können nur im Code-Block und ihm untergeordneten Code der Binde-Funktion benutzt werden.

## Datentypen

Es gibt folgende atomare primitive Datentypen:

| Kürzel | Code | Deutsch | Wertebereich | Bemerkung |
| ------ | ---- | ------- | ------------ | --------- |
| B | `Boolean` | Wahrheitswert | false, true | |
| Q | `Rational` | Rationale Zahl | -2^63 .. 2^63-1 | Verhältnis zweier Ganzzahlen |
| I | `Identifier` | Bezeichner | n.A. | Dient als Name für Variablen, Parameter usw. |
| S | `String` | Zeichenkette | n.A. | Stellt Text dar |
| N | `Null` | Nichtstyp | null | kein Wert |
| V | `Void` | Leertyp | void | keine Rückgabe |

Es gibt folgende strukturierte primitive Datentypen:

| Kürzel | Code | Deutsch | Wertebereich | Bemerkung |
| ------ | ---- | ------- | ------------ | --------- |
| L | `List`| Liste | Eine Liste beliebig vieler Ausdrücke |
| Er | `Error` | Fehler | Eine Fehlermeldung |
| F | `Function` | Funktion | Eine anonyme Funktion |
| M | `Map` | Zuordnungstabelle | Ein Schlüssel-Wert-Verzeichnis |

## Die Rationale Zahl

Rationale Zahlen (Verhältnisse zweier ganzer Zahlen) sind die Grundlage mathematischer Berechnungen in STP. STP akzeptiert jede Darstellung nach dem Schema a/b und -a/b , wobei a und b ganze Zahlen dargestellt in der Basis 10 und zwischen 0 und 2^31-1 sein müssen. Intern werden diese gekürzt und dafür gesorgt, dass der Nenner nie negativ ist. Dadurch ist das Vorzeichen des Zählers automatisch das Vorzeichen des gesamten Bruchs.

## Beschränkungen und Funktionsüberladungen

Standardmäßig akzeptiert STP jeden Ausdruck als Funktionsargument und kann jeden Ausdruck jeder Art als Resultat zurückgeben. Um den Bereich erlaubter Argumente einzuschränken, ist es möglich Beschränkungen, sogenannte 'Constrain's zu verwenden. Diese stellen eine Bedingung dar, die das übergebene Argument erfüllen muss, damit die entsprechende Funktion ausgewertet werden kann; und sind entsprechend Parameter-Annotationen.

Es gibt allgemeine Beschränkungen und Datentypbeschränkungen. Allgemeine Beschränkungen sind beliebige Bedingungen, die erfüllt werden müssen.
Beispiel:
`(Function ((x (Function (x) (% x 2)))) x)`
Beschränkung für den Parameter x, die ausschließlich grade Zahlen als Argumente erlaubt.

Datentypbeschränkungen sind Beschränkungen des Datentyps der Parameter.
Beispiel:
`(Function ((constrain x (instanceof x "Integer"))) x)`
Kann abgekürzt werden zu:
`(Function (x:Integer) x)`
Beschränkung für den Parameter x, die ausschließlich Argumente des Datentyps Integer (Ganzzahl) und dessen Untertypen erlaubt.

Für den Aufruf anonymer Funktionen mit Beschränkungen gilt, dass wenn diese nicht erfüllt werden, ein Fehler zurückgegeben wird.

Für den Aufruf benannter Funktionen mit allgemeinen Beschränkungen gilt, dass alle gleichnamigen Funktionen auf Erfüllung der Bedingungen getestet werden. Erfüllen die Argumente alle Bedingungen einer benannten Funktion, so wird diese ausgeführt, andernfalls wird ein Fehler zurückgegeben. Wenn die Argumente die Bedingungen mehrerer benannter Funktionen erfüllen, so ist das Verhalten implementationsdefiniert. Entweder wird ein Ambiguitätsfehler zurückgegeben oder eine beliebige dieser Funktionen ausgewertet.

Für den Aufruf benannter Funktionen mit Datentypbeschränkungen gilt, dass bei mehreren gleichnamigen Funktionen diejenige ausgewertet wird, deren Typbeschränkungen die am meisten spezialisierten sind, die durch die Argumente noch erfüllt werden.

## Operatoren

1. Der Indexoperator `a[b]` entspricht `(get a b)`
2. Der Mitgliedsoperator `a.b` entspricht `(get-prop a b)`
3. Der Kurzfunktionsausdruck `{A... -> B...}` entspricht `(Function (A...) (begin B...))`
4. Ein Ausdruck, der mit ... beginnt, wird transformiert zu ...
    - `'x` -> `(quote x)`
    - `?x` -> `(eval x)`
    - ``x` -> `(quasiquote x)`
    - `,x` -> `(unquote x)`
5. `(a . b)` wird transformiert zu `(a b)`
6. Die Zeichen `` ' ? ` `` und `,` innerhalb von Ausdrücken sind unzulässig.

## Grundfunktionen

Legende zur Notation:

- A/B steht für eine Alternative: Entweder A oder B
- T... steht für beliebig viele Argumente des Typs T
- Tn, wobei n eine Zahl ist, steht für das n-te Argument des Typs T
- Ai in der Beschreibung steht für A in der Eingabe, Ao für A in der Ausgabe
- Cn steht für eine Beschränkung, die je nach Implementation ein primitiver 0. Datentyp, eine Liste oder eine Funktion ist
- Lp steht für ein logisches Prädikat, welches B als Ergebnis hat
- St steht für eine Struktur
- Z steht für eine rationale Zahl, die zugleich eine Ganzzahl ist
- N steht für eine rationale Zahl, die zugleich eine Ganzzahl >= 0 ist
- L(T) steht für eine Liste, deren Elemente alle vom Typ T sind
- P(T1 T2) steht für ein Paar, dessen erstes Element den Typ T1 hat und dessen zweites Element den Typ T2 hat
- [A]:B steht dafür, dass B der Default-Wert bei einem optionalen A ist. Wird kein 0. A angegeben, wird B angenommen
- {A B} steht dafür, dass für A die Beschränkung B gilt
- name=A steht dafür, dass name vom Typ A ist

| a   | b   |
| --- | --- |
| a   | b   |

## Beispiele

todo

## Definiertes, implementationsdefiniertes und undefiniertes Verhalten

### Ungewöhnliches Verhalten

1. is-identical sagt aus, ob alle Operanden denselben Speicherbereich teilen und nicht, ob diese in Bezug auf alle Merkmale gleich sind. (Dafür gibt es `=`)
2. `{(def i 1) (def i 2) (+ 1 1)}` hat das Ergebnis `4`. Dies liegt daran, dass `i` auf `1` gesetzt wird, und im nächsten `def` daher `(def 1 2)` steht, sodass die Zahl `1` mit dem Wert `2` überschrieben wird. Der zurückzugebende Ausdruck ist dann nicht `(+ 1 1)`, sondern `(+ 2 2)`, der zu `4` evaluiert.
Daher ist es wichtig, wenn man eine Variable selbst und nicht ihren Wert meint, diese mit einem Apostroph zu quotieren.
`{(def 'i 1) (def 'i 2) (+ 1 1)}` hat das erwüschte Verhalten.

### Definiertes Verhalten

1. Der Interpreter darf davon ausgehen, dass ein Skript ausschließlich (implementations-)definiertes Verhalten aufweist.
2. Die Evaluation von Listenelementen mit Seiteneffekten erfolgt von links nach rechts, sofern nicht im Speziellen anders definiert. Diese Regel findet ebenfalls Anwendung auf jeden Ausdruck, der als Liste mit äquivalenter Funktion dargestellt werden kann.
3. Es werden im Sinne der Lazy-Evaluation nur die Argumente evaluiert, die verwendet werden.
4. Eine Division durch 0 erzeugt einen Fehler.
5. Erfüllt ein Ausdruck nicht die Bedingungen der Grammatik, wird ein Fehler erzeugt.
6. Erhält eine Funktion weniger Argumente als erwartet, so sind diese null.

### Implementationsabhängiges Verhalten

1. Datentypbeschränkungen der Doppelpunkt-Kurzschreibweise sind der oben aufgeführten Langformentsprechung vorzuziehen, da es implementationsabhängig ist, ob die entsprechende Langform als Datentypbeschränkung und nicht als allgemeine Beschränkung erkannt wird, für welche die Datentyp-Spezialisierungsregel nicht angewendet werden kann. Für die Doppelpunkt-Kurzschreibweise wird dagegen das beschriebene Verhalten garantiert.
2. Fehlermeldungen und ihre Darstellungsform sind implementationsdefiniert.
3. Die textuelle Darstellung von primitiven Datentypen ist implementationsdefiniert.
4. Man sollte sich nicht auf Identitäten bei primitiven Datentypen verlassen.

### Undefiniertes Verhalten

1. Das Verhalten des Interpreters bei undefiniertem Verhalten ist undefiniert.
2. Das Verhalten bei zu hoher Rekursionstiefe bzw. zu vielen verschachtelten Elementen ist undefiniert.
3. Es ist undefiniert, ob Ausdrücke ausgewertet werden, die das "Endergebnis" nicht beeinflussen.
4. Das Verhalten bei Umdefinierung der Grundfunktionen ist undefiniert.
5. Das Verhalten der sorted-Funktion, falls F einen Wert ausgibt, der nicht 0, 1, oder -1 ist.

## Stilempfehlungen für Skriptdateien (unvollständig)

- Der Beginn aller Funktionsdefinitionen höchsten Levels wird nicht eingerückt.
- Schließende Klammern folgen niemals auf eine neue Zeile.
- Die Einrückung nimmt bei jeder Stufe um zwei Leerzeichen zu.
- Für robusten Code ist so weit wie möglich die reine Listendarstellung (etwa von Funktionsdefinitionen) zu verwenden, um Parsing-Fehler nicht zu provozieren.

## Bibliotheken

- STP-Skriptdateien, die neue Funktionen bereitstellen und mit einem include eingebunden werden, werden als Bibliotheken bezeichnet.
- Die corelib.stp-Bibliothek stellt gemeinsam mit der Kernimplementation dem Nutzer alle hier spezifizierten Grundfunktionen von STP zur Verfügung.
- Die stdmath.stp-Bibliothek erweitert STP um praktische Mathefunktionen.
- Die stdlib.stp-Bibliothek erweitert STP um praktische Funktionen. Sie enthält corelib.stp und stdmath.stp

## Inspirationsquellen

- [Scheme](http://www.scheme-reports.org/)
- [JavaScript](https://www.ecma-international.org/publications-and-standards/standards/ecma-262/)
- [Java](https://www.oracle.com/java/)
- [Snap!](https://snap.berkeley.edu/)
- [GP](https://gpblocks.org/)
- [Lua](https://www.lua.org/)

(c) Teutim 2022
