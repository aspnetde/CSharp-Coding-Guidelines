# C# Coding Style Guidelines

## Stil

### Geschweifte Klammern

Geschweifte Klammern, die einen Block definieren, werden in einer eigenen Zeile geöffnet und in einer eigenen Zeile wieder geschlossen. Richtig ist also:

    public void Foo()
    {

    }

Weniger schön hingegen ist:

    public void Bar() {

    }

Das mag penibel klingen, macht aber doch einen großen Unterschied.

### Verwendung von var

`var` verwenden wir in der Regel dort, wo der Typ einer Variablen bei deren Zuweisung klar ersichtlich wird:

    var output = new StringBuilder();

Häufig ist das deutlich lesbarer als die doch ziemlich redundante Deklaration mit explizitem Typ:

    StringBuilder output = new StringBuilder();

Bei primitiven Datentypen wie `int` und `double`, die Zahlen repräsentieren, ziehen wir hingegen den expliziten Typ vor. So ist leichter zu erkennen, ob es sich um Fließkomma- oder ganze Zahlen handelt.

## Namenskonventionen

### Ungarische Notation

Wird nicht verwendet — *never, ever*.

### Sprechende Namen

Variablen- und Methodennamen sollten treffend benannt werden, sodass leicht verständlich ist, wofür sie stehen.

    // Nicht gut. Wofür wird dieser StringBuilder eingesetzt?
    var stringBuilder = new StringBuilder();

    // Besser! Jetzt ist klar, was damit gemacht wird.
    var htmlOutput = StringBuilder();

### Aussprechbare Namen

Eine Variable, die einen Kontext repräsentiert, kann ruhig `context` heißen, aber bitte nicht `cxt`. Solche Abkürzungen sind schwer zu lesen und erschweren so das Verstehen von Code.

### Felder

Private Felder beginnen mit einem Unterstrich, sodass diese leicht von Parametern und lokalen Variablen unterschieden werden können.

    public class Foo
    {
        private int bar;
        
        public Foo(int bar)
        {
            _bar = bar;
        }
    }
