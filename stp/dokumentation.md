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

<table border="1">
    <thead>
        <tr>
            <th>NAME</th>
            <th>EINGABETYPEN</th>
            <th>AUSGABETYP</th>
            <th>BESCHREIBUNG</th>
            <th>VERSION</th>
            <td>DEFINITION</td>
        </tr>
    </thead>
    <tbody class="control-functions">
        <tr class="grouplabel"><th colspan="5">Contextfunktionen</td></tr>
        <tr>
            <td>get-primitives</td>
            <td>V</td>
            <td>L</td>
            <td>Liste der Bezeichner aller primitiven Funktionen</td>
            <td>umbenannt</td>
        </tr><tr>
            <td>get-context</td>
            <td>V</td>
            <td>M</td>
            <td>Aktueller Kontext</td>
            <td>neu</td>
        </tr><tr>
            <td>set-context</td>
            <td>M</td>
            <td>V / E</td>
            <td>&Uuml;berschreibt den aktuellen Kontext</td>
            <td>neu</td>
        </tr><tr>
            <td>new-subcontext</td>
            <td>E</td>
            <td>E</td>
            <td>Evaluiert Ei in einem neuen Unterkontext</td>
            <td>neu</td>
        </tr><tr>
            <td>put-prop</td>
            <td>M E1 E2</td>
            <td>M</td>
            <td>F&uuml;gt in M den Wert E2 f&uuml;r den Schl&uuml;ssel E1 ein</td>
            <td>neu</td>
        </tr><tr>
            <td>get-prop</td>
            <td>M E</td>
            <td>E</td>
            <td>Wert des Schl&uuml;ssels Ei in M</td>
            <td>neu</td>
        </tr><tr>
            <td>remove-prop</td>
            <td>M E</td>
            <td>M</td>
            <td>Entfernt das E in M/td>
            <td>neu</td>
        </tr><tr>
            <td>context-of</td>
            <td>E</td>
            <td>M</td>
            <td>Kontext der den Schl&uuml;ssel E definiert</td>
            <td>neu</td>
        </tr><tr>
            <td>coalesce</td>
            <td>E1 E2</td>
            <td>E</td>
            <td>Gibt E1 zur&uuml;ck, sofern dieses nicht null ist, sonst E2</td>
            <td>neu (Elvis-Operator)</td>
        </tr>
    </tbody>
    <tbody class="control-functions">
        <tr class="grouplabel"><th colspan="5">Sprachfunktionen</td></tr>
        <tr>
            <td>get-version</td>
            <td>V</td>
            <td>S</td>
            <td>STP-Version</td>
            <td>neu</td>
        </tr><tr>
            <td>date-and-time</td>
            <td>V</td>
            <td>L</td>
            <td>Aktuelle Zeit im Format<br /><code>(Date-And-Time Y M D h m s)</code></td>
            <td>neu</td>
        </tr><tr>
            <td>current-time-millis</td>
            <td>V</td>
            <td>N</td>
            <td>Aktuelle Zeit in Millisekunden</td>
            <td>neu</td>
        </tr>
        <tr class="semanticbreak"><th colspan="5">.</td></tr>
        <tr>
            <td>tokenize</td>
            <td>S</td>
            <td>L</td>
            <td>Liste von Tokens</td>
            <td>neu</td>
        </tr><tr>
            <td>is-valid-expression</td>
            <td>S</td>
            <td>B</td>
            <td>Ist S ein valider Ausdruck?</td>
            <td>neu</td>
        </tr><tr>
            <td>parse</td>
            <td>S</td>
            <td>E</td>
            <td>Parst S</td>
            <td>neu</td>
        </tr>
        <tr class="semanticbreak"><th colspan="5">.</td></tr>
        <tr>
            <td>list</td>
            <td>E...</td>
            <td>L</td>
            <td>Liste aller evaluierten E...</td>
        </tr><tr>
            <td>copy-deep</td>
            <td>E</td>
            <td>E</td>
            <td>Gibt eine Tiefenkopie des evaluierten Ei zur&uuml;ck</td>
            <td>neu</td>
        </tr><tr>
            <td>quote</td>
            <td>E</td>
            <td>E</td>
            <td>Ei in Rohform</td>
        </tr><tr>
            <td>eval</td>
            <td>E</td>
            <td>E</td>
            <td>Evaluiertes Ei</td>
        </tr><tr>
            <td>quasiquote</td>
            <td>E</td>
            <td>E</td>
            <td>Wie quote, nur dass innere Ausdr&uuml;cke mit unquote evaluiert werden</td>
        </tr><tr>
            <td>unquote</td>
            <td>E</td>
            <td>E</td>
            <td>Evaluiertes Ei</td>
        </tr><tr>
            <td>ignore</td>
            <td>E...</td>
            <td>V</td>
            <td>Dient als Kommentar</td>
        </tr><tr>
            <td>is-atom</td>
            <td>E</td>
            <td>B</td>
            <td>Ist E ein Atom?</td>
        </tr><tr>
            <td>type-of</td>
            <td>E</td>
            <td>S</td>
            <td>Name des Datentyps von E</td>
        </tr><tr>
            <td>instanceof</td>
            <td>E S</td>
            <td>B</td>
            <td>Hat E den Datentyp S ?</td>
            <td>neu</td>
        </tr><tr>
            <td>return</td>
            <td>E</td>
            <td>E</td>
            <td>Zur Zeit wie id. Bitte nur dort verwenden, wo eine Funktion abgebrochen werden soll.</td>
        </tr><tr>
            <td>id</td>
            <td>E</td>
            <td>E</td>
            <td>Identit&auml;tsfunktion</td>
            <td></td>
            <td>corelib</td>
        </tr><tr>
            <td>exit</td>
            <td>V</td>
            <td>V</td>
            <td>Beendet das Skript</td>
            <td>neu</td>
        </tr>
    </tbody>
<tbody class="control-functions">
    <tr class="grouplabel"><th colspan="5">Typkonstruktoren</td></tr>
    <tr>
        <td>Error</td>
        <td></td>
        <td>Er</td>
        <td></td>
    </tr><tr>
        <td>Function</td>
        <td></td>
        <td>F</td>
        <td></td>
        <td>umbenannt</td>
    </tr><tr>
        <td>lambda</td>
        <td></td>
        <td>F</td>
        <td>Wie Function</td>
        <td>neu</td>
        <td>corelib</td>
    </tr><tr>
        <td>Map</td>
        <td></td>
        <td>M</td>
        <td></td>
        <td>neu</td>
    </tr><tr>
        <td>Object</td>
        <td></td>
        <td>M</td>
        <td>Wie Map</td>
        <td>neu</td>
        <td>corelib</td>
    </tr><tr>
        <td>Pair</td>
        <td></td>
        <td>P</td>
        <td></td>
        <td>neu</td>
    </tr><tr>
        <td>cons</td>
        <td></td>
        <td>P</td>
        <td>Wie Pair</td>
        <td>neu</td>
        <td>corelib</td>
    </tr><tr>
        <td>Array</td>
        <td></td>
        <td>A</td>
        <td></td>
        <td>neu</td>
    </tr><tr>
        <td>List</td>
        <td></td>
        <td>L</td>
        <td></td>
    </tr><tr>
        <td>Rational</td>
        <td></td>
        <td>Q</td>
        <td></td>
        <td>neu</td>
    </tr><tr>
        <td>as-list</td>
        <td>St</td>
        <td>L</td>
        <td>Wandelt St in ihre Listendarstellung um</td>
    </tr><tr>
        <td>String</td>
        <td></td>
        <td>S</td>
        <td></td>
    </tr>
</tbody>
<tbody class="control-functions">
    <tr class="grouplabel"><th colspan="5">Kontrollstrukturen, Verzweigungen, Schleifen</td></tr>
    <tr>
        <td>cond</td>
        <td>L(P(B E))</td>
        <td>E</td>
        <td>Pr&uuml;ft, ob ein B der Paare wahr ist und gibt das entsprechende E aus. Ist keine wahr, wird false ausgegeben</td>
    </tr><tr>
        <td>if</td>
        <td>B E1 E2</td>
        <td>E</td>
        <td>Wenn B wahr ist, wird E1 ausgegeben, sonst E2</td>
    </tr><tr>
        <td>while</td>
        <td>B E</td>
        <td>E</td>
        <td>E wird evaluiert, solange B wahr ist. Das letzte Ergebnis wird ausgegeben</td>
    </tr><tr>
        <td>for</td>
        <td>E1 B E2 E3</td>
        <td>E</td>
        <td>Evaluiert E1. Evaluiert danach solange E3 und dann E2, wie B wahr ist</td>
    </tr><tr>
        <td>begin</td>
        <td>E...</td>
        <td>E</td>
        <td>Evaluiert nacheinander alle E...i und gibt letztes Ergebnis aus</td>
        <td>umbenannt</td>
    </tr><tr>
        <td>pipe</td>
        <td>E [F1...Fn]</td>
        <td>E</td>
        <td>&Uuml;bergibt E als erstes Argument an F1 und deren Ergebnis als erstes Argument an F2 bis Fn.<br />Das Ergebnis von Fn ist das Ergebnis von pipe.<br />Statt F darf auch E benutzt werden, wobei sich hier quotierte E empfehlen. Dann wird x als Resultat der vorangegangenen Funktion definiert.</td>
        <td>neu</td>
    </tr>
</tbody>
<tbody class="math-functions">
    <tr class="grouplabel"><th colspan="5">Vergleiche & Bedingungen</td></tr>
    <tr>
        <td>=</td>
        <td>E1 E2...En</td>
        <td>B</td>
        <td>Sind alle E2...En gleich E1 ?</td>
    </tr><tr>
        <td>!=</td>
        <td>E1 E2...En</td>
        <td>B</td>
        <td>Sind alle E2...En ungleich E1 ?</td>
    </tr><tr>
        <td>&gt;</td>
        <td>E1 E2...En</td>
        <td>B</td>
        <td>Ist E1 gr&ouml;&szlig;er als alle E2...En ?</td>
    </tr><tr>
        <td>&lt;</td>
        <td>E1 E2...En</td>
        <td>B</td>
        <td>Ist E1 kleiner als alle E2...En ?</td>
    </tr><tr>
        <td>and</td>
        <td>B...</td>
        <td>B</td>
        <td>Sind alle B...i wahr?</td>
    </tr><tr>
        <td>or</td>
        <td>B...</td>
        <td>B</td>
        <td>Ist eines von B...i wahr?</td>
    </tr><tr>
        <td>not</td>
        <td>B</td>
        <td>B</td>
        <td>Negiertes Bi</td>
    </tr><tr>
        <td>assert</td>
        <td>B</td>
        <td>B(true) | Er</td>
        <td>Wenn Bi wahr ist, wird wahr ausgegeben, andernfalls ein Fehler</td>
        <td>neu</td>
    </tr><tr>
        <td>is-null</td>
        <td>E</td>
        <td>B</td>
        <td>Ist E null?</td>
        <td>neu</td>
    </tr><tr>
        <td>not-null</td>
        <td>E</td>
        <td>B</td>
        <td>Ist E nicht null?</td>
        <td>neu</td>
    </tr><tr>
        <td>is-identical</td>
        <td>E...</td>
        <td>B</td>
        <td>Sind alle E... identisch?</td>
        <td>neu</td>
    </tr>
</tbody>
<tbody class="math-functions">
    <tr class="grouplabel"><th colspan="5">Bitweise Operationen</td></tr>
    <tr>
        <td>bitand</td>
        <td>Z1 Z2</td>
        <td>Z</td>
        <td>Und der Bits von Z1 und Z2</td>
    </tr><tr>
        <td>bitior</td>
        <td>Z1 Z2</td>
        <td>Z</td>
        <td>Inklusives Oder der Bits von Z1 und Z2</td>
    </tr><tr>
        <td>bitxor</td>
        <td>Z1 Z2</td>
        <td>Z</td>
        <td>Exklusives Oder der Bits von Z1 und Z2</td>
    </tr><tr>
        <td>bitnor</td>
        <td>Z1 Z2</td>
        <td>Z</td>
        <td>Nicht Oder der Bits von Z1 und Z2</td>
    </tr><tr>
        <td>biteqv</td>
        <td>Z1 Z2</td>
        <td>Z</td>
        <td>&Auml;quivalenz der Bits von Z1 und Z2</td>
    </tr><tr>
        <td>bitash</td>
        <td>Z1 Z2</td>
        <td>Z</td>
        <td>Arithmetische Verschiebung der Bits von Z1 um Z2 Stellen</td>
    </tr><tr>
        <td>bitlsh</td>
        <td>Z1 Z2</td>
        <td>Z</td>
        <td>Logische Verschiebung der Bits von Z1 um Z2 Stellen</td>
    </tr><tr>
        <td>bitcount</td>
        <td>Z</td>
        <td>Z</td>
        <td>Anzahl aller 1-Bits in Zi</td>
    </tr><tr>
        <td>bitnot</td>
        <td>Z</td>
        <td>Z</td>
        <td>Negiert die Bits von Zi</td>
    </tr>
</tbody>
<tbody class="math-functions">
    <tr class="grouplabel"><th colspan="5">Arithmetik</td></tr>
    <tr>
        <td>+</td>
        <td>Q...</td>
        <td>Q</td>
        <td>Summiert alle Qs</td>
    </tr><tr>
        <td>-</td>
        <td>Q1 ... Qn</td>
        <td>Q</td>
        <td>Subtrahiert Q2 ... Qn von Q1</td>
    </tr><tr>
        <td>*</td>
        <td>Q...</td>
        <td>Q</td>
        <td>Multipliziert alle Qs</td>
    </tr><tr>
        <td>/</td>
        <td>Q1 ... Qn</td>
        <td>Q</td>
        <td>Dividiert Q1 durch Q2 ... Qn</td>
    </tr><tr>
        <td>power</td>
        <td>Q...</td>
        <td>Q</td>
        <td>Q1 hoch Q2 ... hoch Qn.<br />Nota bene: *(sqrt x) = (power x 1/2)</td>
        <td>neu</td>
    </tr><tr>
        <td>truncate</td>
        <td>Q</td>
        <td>Z</td>
        <td>Entfernt den Nachkommaanteil von Q</td>
    </tr><tr>
        <td>sin</td>
        <td>Q</td>
        <td>Q</td>
        <td>Sinus von Qi</td>
    </tr><tr>
        <td>cos</td>
        <td>Q</td>
        <td>Q</td>
        <td>Kosinus von Qi</td>
    </tr><tr>
        <td>tan</td>
        <td>Q</td>
        <td>Q</td>
        <td>Tangens von Qi</td>
    </tr><tr>
        <td>asin</td>
        <td>Q</td>
        <td>Q</td>
        <td>Arcus-Sinus von Qi</td>
    </tr><tr>
        <td>acos</td>
        <td>Q</td>
        <td>Q</td>
        <td>Arcus-Kosinus von Qi</td>
    </tr><tr>
        <td>atan</td>
        <td>Q</td>
        <td>Q</td>
        <td>Arcus-Tangens von Qi</td>
    </tr><tr>
        <td>abs</td>
        <td>Q</td>
        <td>Q</td>
        <td>Betrag von Qi</td>
        <td>neu</td>
    </tr><tr>
        <td>ceiling</td>
        <td>Q</td>
        <td>Q</td>
        <td>Aufgerundetes Qi</td>
        <td>neu</td>
    </tr><tr>
        <td>floor</td>
        <td>Q</td>
        <td>Q</td>
        <td>Abgerundetes Qi</td>
        <td>neu</td>
    </tr><tr>
        <td>log</td>
        <td>[Q1]:10 Q2</td>
        <td>Q</td>
        <td>Logarithmus von Q2 zur Basis Q1</td>
        <td>neu</td>
    </tr><tr>
        <td>round</td>
        <td>Q</td>
        <td>Q</td>
        <td>Kaufm&auml;nnisch gerundetes Qi</td>
        <td>neu</td>
    </tr><tr>
        <td>max</td>
        <td>Q...</td>
        <td>Q</td>
        <td>Gr&ouml;&szlig;ter Wert von Q...</td>
        <td>neu</td>
    </tr><tr>
        <td>min</td>
        <td>Q...</td>
        <td>Q</td>
        <td>Kleinster Wert von Qi</td>
        <td>neu</td>
    </tr><tr>
        <td>mod</td>
        <td>Q1 Q2</td>
        <td>Q</td>
        <td>Rest der Division Q1 durch Q2</td>
        <td>neu</td>
    </tr><tr>
        <td>max-num</td>
        <td>V</td>
        <td>N</td>
        <td>Maximale durch den Typ Number darstellbare Zahl</td>
        <td>neu</td>
    </tr><tr>
        <td>min-num</td>
        <td>V</td>
        <td>N</td>
        <td>Minimale durch den Typ Number darstellbare Zahl</td>
        <td>neu</td>
    </tr><tr>
        <td>max-int</td>
        <td>V</td>
        <td>N</td>
        <td>Maximaler durch den Typ Number darstellbarer Integer<br />(I.d.R gleich Javas Integer.MAX_VALUE)</td>
        <td>neu</td>
    </tr><tr>
        <td>min-int</td>
        <td>V</td>
        <td>N</td>
        <td>Minimaler durch den Typ Number darstellbarer Integer<br />(I.d.R gleich Javas Integer.MIN_VALUE)</td>
        <td>neu</td>
    </tr><tr>
        <td>numerator</td>
        <td>Q</td>
        <td>N</td>
        <td>Z&auml;hler der Bruchdarstellung von Q</td>
        <td>neu</td>
    </tr><tr>
        <td>denominator</td>
        <td>Q</td>
        <td>N</td>
        <td>Nenner der Bruchdarstellung von Q</td>
        <td>neu</td>
    </tr>
</tbody>
<tbody class="symbols-and-functions">
    <tr class="grouplabel"><th colspan="5">Symbole & Funktionen</td></tr>
    <tr>
        <td>def</td>
        <td>schluessel1=E wert1=E ... schluesselN=E wertN=E</td>
        <td>E</td>
        <td>Bindet Werte an Schl&uuml;ssel im aktuellen Kontext</td>
    </tr><tr>
        <td>overload</td>
        <td>schluessel1=E wert1=F ... schluesselN=E wertN=F</td>
        <td>E</td>
        <td>Sofern der Schl&uuml;ssel bereits in einem h&ouml;heren oder diesem Kontext definiert ist, wird dieser mit einem neuen Wert &uuml;berschieben. Andernfalls wird der Wert an einen neuen Schl&uuml;ssel im aktuellen Kontext gebunden.</td>
    </tr><tr>
        <td>overwrite</td>
        <td>schluessel1=E wert1=E ... schluesselN=E wertN=E</td>
        <td>E</td>
        <td>Sofern der Schl&uuml;ssel bereits in einem h&ouml;heren oder diesem Kontext definiert ist, wird dieser mit einem neuen Wert &uuml;berschieben. Andernfalls wird der Wert an einen neuen Schl&uuml;ssel im aktuellen Kontext gebunden.</td>
    </tr><tr>
        <td>let</td>
        <td>schluessel1=E wert1=E ... schluesselN=E wertN=E ausdruck=E</td>
        <td>E</td>
        <td>Bindet Werte an Schl&uuml;ssel in einem Unterkontext und gibt Resultat von ausdruck zur&uuml;ck</td>
    </tr><tr>
        <td>call</td>
        <td>F args=E/L</td>
        <td>E</td>
        <td>Ruft die Funktion F mit den Argumenten args auf</td>
    </tr><tr>
        <td>foreach</td>
        <td>St F</td>
        <td>L</td>
        <td>Wendet F auf jedes Element in St an. Dabei sind die &uuml;bergebenen Argumente: Wert, Index, Struktur</td>
        <td>ge&auml;ndert</td>
    </tr><tr>
        <td>multi-call</td>
        <td>F(>= A1...An) L1...Ln</td>
        <td>L</td>
        <td>Evaluiert F so viele Male, wie die L&auml;nge der l&auml;ngsten Liste. Sei Index ein Z&auml;hler der Durchl&auml;ufe, so sind die an F &uuml;bergebenen Argumente im Index-ten Durchlauf L1[Index] bis Ln[Index] sowie Index. Das Ergebnis von multi-call ist eine Liste aller Resultate</td>
        <td>neu</td>
    </tr>
</tbody>
<tbody class="list-functions">
    <tr class="grouplabel"><th colspan="5">Listenoperationen</td></tr>
    <tr>
        <td>get</td>
        <td>L N</td>
        <td>E</td>
        <td>Ntes Element in L</td>
    </tr><tr>
        <td>length</td>
        <td>L</td>
        <td>N</td>
        <td>Anzahl der Elemente von L</td>
    </tr><tr>
        <td>add</td>
        <td>L [N] E</td>
        <td>L</td>
        <td>Erweitert L um E [an Stelle N]</td>
    </tr><tr>
        <td>set</td>
        <td>L N E</td>
        <td>L</td>
        <td>Setzt in L an Stelle N E ein</td>
    </tr><tr>
        <td>remove</td>
        <td>L N</td>
        <td>L</td>
        <td>Entfernt N-tes Element in L</td>
    </tr><tr>
        <td>sublist</td>
        <td>L N1 N2</td>
        <td>L</td>
        <td>Liste der Elemente N1 bis N2 von Li</td>
    </tr><tr>
        <td>random</td>
        <td>L</td>
        <td>E</td>
        <td>Zuf&auml;lliges Element von L</td>
    </tr><tr>
        <td>range</td>
        <td>[N1]:0 N2 {[N3]:1 != 0}</td>
        <td>L</td>
        <td>Eine Liste aller Zahlen in N3-er Schritten von N1 bis N2. Ist N3 negativ, so wird heruntergez&auml;hlt.</td>
        <td>neu</td>
    </tr><tr>
        <td>append</td>
        <td>L...</td>
        <td>L</td>
        <td>Verbindet alle L... von links nach rechts zu einer Liste. Ist L... leer, so wird eine leere Liste ausgegeben.</td>
        <td>neu</td>
    </tr><tr>
        <td>sort</td>
        <td>L F(a b)</td>
        <td>L</td>
        <td>Sortiert L durch Anwendung von F. F ist eine Funktion, die entweder -1 0 oder 1 ausgibt, abh&auml;ngig davon, ob a gleich/gr&ouml;&szlig;er/kleiner b ist</td>
        <td>neu</td>
    </tr><tr>
        <td>shuffle</td>
        <td>L</td>
        <td>L</td>
        <td>Mischt L zuf&auml;llig</td>
        <td>neu</td>
    </tr><tr>
        <td>reverse</td>
        <td>L</td>
        <td>L</td>
        <td>L r&uuml;ckw&auml;rts</td>
        <td>neu</td>
    </tr><tr>
        <td>flatten</td>
        <td>L</td>
        <td>L</td>
        <td>Li wird als Baumstruktur angesehen. Lo sind dann die Bl&auml;tter derselben.</td>
        <td>neu</td>
    </tr><tr>
        <td>keep</td>
        <td>L F(value=E index=N list=L)->B</td>
        <td>L</td>
        <td>Liste aller Elemente von Li, f&uuml;r die F true ist</td>
        <td>neu</td>
    </tr><tr>
        <td>find</td>
        <td>L F(value=E index=N list=L)->B</td>
        <td>N</td>
        <td>Index des ersten Elementes von Li, f&uuml;r das F true ist. Existiert ein solches nicht, wird -1 returnt.</td>
        <td>ver&auml;ndert</td>
    </tr>
</tbody>
<tbody class="string-functions">
    <tr class="grouplabel"><th colspan="5">Zeichenkettenoperationen</td></tr>
    <tr>
        <td>code-points</td>
        <td>S</td>
        <td>L(N)</td>
        <td>Liste aller Unicode-Code-Points von S</td>
        <td>neu</td>
    </tr><tr>
        <td>codes-to-string</td>
        <td>L(N)</td>
        <td>S</td>
        <td>F&uuml;gt alle Unicode-Code-Points zu einem S zusammen</td>
        <td>neu</td>
    </tr><tr>
        <td>chars</td>
        <td>S</td>
        <td>L(S)</td>
        <td>Liste aller Zeichen von S</td>
        <td>ver&auml;ndert</td>
    </tr><tr>
        <td>to-uppercase</td>
        <td>S</td>
        <td>S</td>
        <td>Verwandelt S in Gro&szlig;buchstaben</td>
        <td>neu</td>
    </tr><tr>
        <td>to-lowercase</td>
        <td>S</td>
        <td>S</td>
        <td>Verwandelt S in Kleinbuchstaben</td>
        <td>neu</td>
    </tr><tr>
        <td>join</td>
        <td>E...</td>
        <td>S</td>
        <td>Wandelt jedes von E... in ein S um und verbindet diese</td>
        <td>neu</td>
    </tr><tr>
        <td>split</td>
        <td>S1 S2</td>
        <td>L(S)</td>
        <td>Spaltet S1 an jedem S2</td>
    </tr><tr>
        <td>regex</td>
        <td>S1 S2 S3</td>
        <td>S</td>
        <td>Ersetzt in S3 jeden Text, f&uuml;r den S1 zutrifft, durch S2</td>
    </tr><tr>
        <td>decimal</td>
        <td>Q</td>
        <td>S</td>
        <td>Dezimale Repr&auml;sentation von Q. Aus 1/4 wird "0.75"</td>
        <td>neu</td>
    </tr>
</tbody>
<tbody class="io-functions">
    <tr class="grouplabel"><th colspan="5">Eingabe-Ausgabe</td></tr>
    <tr>
        <td>include</td>
        <td>E...</td>
        <td>E</td>
        <td>Liest Skriptdateien, deren Namen in E...i angegeben werden, parst und evaluiert sie. Letztes Ergebnis wird ausgegeben</td>
    </tr><tr>
        <td>io-read-string</td>
        <td>E</td>
        <td>S</td>
        <td>Liest Datei mit dem Namen E ein und gibt deren Text aus.<br /> Ist E eine L(I("url") S), dann wird der textuelle Inhalt der Website mit der URL S ausgegeben.</td>
    </tr><tr>
        <td>io-read-bytes</td>
        <td>E</td>
        <td>L(N)</td>
        <td>Liest Datei mit dem Namen E ein und gibt eine Liste der Bytes aus</td>
    </tr><tr>
        <td>printc</td>
        <td>E</td>
        <td>E</td>
        <td>Wandelt E in einen String um und schreibt diesen in die Ausgabe</td>
        <td>umbenannt</td>
    </tr><tr>
        <td>printv</td>
        <td>E</td>
        <td>E</td>
        <td>Wandelt E in einen String um, wie er durch join entstehen w&uuml;rde, und schreibt diesen in die Ausgabe</td>
        <td>neu</td>
    </tr><tr>
        <td>input</td>
        <td>E...</td>
        <td>E</td>
        <td>Ruft die Eingabe-Prozedur mit den Operanden als Argumente auf und gibt deren Ergebnis zur&uuml;ck</td>
        <td>neu</td>
    </tr><tr>
        <td>output</td>
        <td>E...</td>
        <td>E</td>
        <td>Ruft die Ausgabe-Prozedur mit den Operanden als Argumente auf und gibt deren Ergebnis zur&uuml;ck</td>
        <td>neu</td>
    </tr>
</tbody>
</table>

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
