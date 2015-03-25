# 69° C# Coding Style Guidelines

## Übersicht

- [Namenskonventionen](#Namenskonventionen)  
- [Struktur von Klassen (und Strukturen)](#Struktur)  
- [Groß- und Kleinschreibung](#Stil)  
- [PascalCase & camelCase](#PascalCaseCamelCase)
- [Ungarische Notation](#UngarischeNotation)
- [Namespaces](#Namespaces)
- [Geschweifte Klammern](#GeschweifteKlammern)
- [Leerzeichen](#Leerzeichen)
- [Verwendung von var](#Var)
- [Verzicht auf this](#This)
- [Verwendung des "private"-Modifizierers](#Private)
- [Leerzeilen](#Leerzeilen)
- [Lines of Code (LOC) in Klassen und Methoden](#LOC)
- [Kommentare](#Kommentare)

<a name="Namenskonventionen"/>
## Namenskonventionen

### Sprechende Namen

Alle Namen müssen treffend benannt werden, sodass leicht verständlich ist, wofür sie stehen.

Nicht gut (Wofür wird dieser StringBuilder eingesetzt?)

    var stringBuilder = new StringBuilder();

Besser! Jetzt ist klar, was damit gemacht wird.

    var htmlOutput = StringBuilder();

### Aussprechbare Namen

Eine Variable, die einen Kontext repräsentiert, kann ruhig `context` heißen, aber bitte nicht `cxt` oder gar `c`. Solche Abkürzungen sind schwer zu lesen und erschweren so das Verstehen von Code.

### Vermeidung von "Weasel Words"

Bezeichnungen wie `Manager`, `Processor`, `Service` o.ä. sind zu vermeiden, auch in Zusammensetzungen. Denn aus ihnen geht nicht hervor, was sie tun.

Stattdessen sollten sinnvolle, für sich sprechende Bezeichnungen der jeweiligen Klassen verwendet werden.

Ausnahmen bestätigen die Regel: dort, wo es sich um handelsübliche Bezeichnungen oder vom Framework vorgegebene Konventionen handelt, können Ausnahmen gemacht werden.

So ist ein `Repository` eine Menge, man könnte also statt `StoryRepository` schlicht auch `Stories` verwenden. Da dies jedoch potentiell schnell zu Missverständnissen mit Objekten in anderen Bereichen der Anwendung außerhalb des Datenzugriffs führt, kann `Repository` genutzt werden. Erst Recht, wenn die Aufteilung in `StoryReadRepository` und `StoryWriteRepository` erfolgt.

Ähnliches gilt bei ASP.NET MVC bspw. für Controller. Hier ist es schlicht Konvetion, den `StoryController` zu verwenden.

<a name="Struktur"/>
## Struktur von Klassen (und Strukturen)

Grundsätzlich sollten sich Klassen und Strukturen flüssig von oben nach unten lesen lassen, wobei eine darüber hinausgehende inhaltliche Strukturierung sinnvoll ist:

1. Konstanten
2. Private Felder
3. Öffentliche Eigenschaften (Properties)
4. Konstruktor(en)
5. Öffentliche Methoden
6. Private Methoden

Beispiel:

    public class Foo
    {
        private const int BAR = 1;

        private readonly string _fooBar;

        public string Whatever { get; set; }

        public Foo()
        {
            _fooBar = "barFoo";
        }

        public void MachWas()
        {
            Was();
        }

        private void Was()
        {
        }
    }


<a name="Stil"/>
## Groß- und Kleinschreibung

Grundsätzlich gilt: Namespaces, Klassen und Properties werden immer groß geschrieben.

**Falsch:**

	namespace whatstays
	{
		public class story
		{
			public string title { get; set; }
		}
	}

**Richtig:**

	namesapce Whatstays
	{
		public class Story
		{
			public string Title { get; set; }
		}
	}

Parameter oder (methoden-interne) Variablennamen werden klein geschrieben.

**Falsch:**

	public void Foo(string Bar)
	{
		string FirstName = Bar;
	}

**Richtig:**

	public void Foo(string bar)
	{
		string firstName = bar;
	}


<a name="PascalCaseCamelCase"/>
## PascalCase & camelCase

Werden Bezeichnungen aus mehreren Wörtern zusammengesetzt, so erfolgt dies per `CamelCase` (Anfangsbuchstabe klein, gilt für Parameter oder (methoden-interne) Variablennamen) bzw. `PascalCase (mit großem Anfangsbuchstaben, gilt für Namespaces, Klassen und Properties).

**Falsch:**

	public class Story_Repository
	{
		public IEnumerable<Story> find_by_Name(string story_name)
		{
			// ...
		}
	}

**Richtig:**

	public class StoryRepository
	{
		public IEnumerable<Story> FindByName(string storyName)
		{
			// ...
		}
	}

### Sonderfall: Private Felder

Private Felder beginnen **grundsätzlich** mit einem Unterstrich, sodass diese leicht von Parametern und lokalen Variablen unterschieden werden können.

**Falsch:**

    public class Foo
    {
        private int bar;
        
        public Foo(int bar)
        {
            this.bar = bar;
        }
    }

**Richtig:**

    public class Foo
    {
        private int _bar;
        
        public Foo(int bar)
        {
            _bar = bar;
        }
    }

### Sonderfall: Tests

Um die Lesbarkeit von automatisierten Tests sowohl im TestRunner als auch im konkreten Test-Code zu erhöhen, setzen wir hier ausnahmslos auf Unterstriche zur Trennung von Wörtern.

**Falsch:**

	[TestFixture]
	public class WennDieSonneScheint
	{
		[Test]
		public void WächstInMeinemGartenGras()
		{
			// ...
		}
	}

**Richtig:**

	[TestFixture]
	public class Wenn_die_Sonne_scheint
	{
		[Test]
		public void Wächst_in_meinem_Garten_Gras()
		{
			// ...
		}
	}

_Anmerkung: an dieser Stelle sind ausnahmsweise auch Umlaute und andere Sonderzeichen der Sprache erlaubt, in der der Test formuliert wird. Der Grund hierfür ist einfach: Tests sind häufig eher Spezifikationen, und diese lassen sich flüssiger erstellen und schreiben, wenn man auf die Formatierung nicht sonderlich viel Rücksicht nehmen muss. Siehe auch http://code.69grad.de/69-testing._ 

<a name="UngarischeNotation"/>
## Ungarische Notation

Bei der ungarischen Notation wird dem Variablennamen der Typ vorangestellt, was bei untypisierten Sprachen unter seltenen Umständen hilfreich sein kann, bei C# jedoch vollkommen nutzlos ist.

Ein Beispiel wäre

	strName = "Max Mustermann";
	intAlter = 69;

**Ungarische Notation wird nicht verwendet** — *never, ever*.

<a name="Namespaces"/>
## Namespaces

Für Namespaces gilt: so kurz wie möglich, so lang wie nötig.

Der Aufbau erfolgt nach folgendem Prinzip:

	{Kunde}.{Projekt}.{Logischer Bereich}.{…}

Richtig wäre:

	Whatstays.Website.Controllers

Falsch wären:

	WhatstaysWebsiteControllers
	Whatstays_Website.Controllers

<a name="GeschweifteKlammern"/>
## Geschweifte Klammern

Geschweifte Klammern, die einen Block definieren, werden in einer eigenen Zeile geöffnet und in einer eigenen Zeile wieder geschlossen. Richtig ist also:

    public void Foo()
    {
    }

Falsch hingegen ist:

    public void Bar() {
    }

<a name="Leerzeichen"/>
## Leerzeichen

Nach Methodennamen sowie nach öffnenden und vor schließenden Klammern werden keine Leerzeichen gesetzt.

**Falsch**

	public void Foo ( string param )
	{
	}

**Richtig**

	public void Foo(string param)
	{
	}

<a name="Var"/>
## Verwendung von var

`var` verwenden wir in der Regel dort, wo der Typ einer Variablen bei deren Zuweisung klar ersichtlich wird:

    var duck = new Duck();

Häufig ist das deutlich lesbarer als die doch ziemlich redundante Deklaration mit explizitem Typ:

    Duck duck = new Duck();

Bei primitiven Datentypen wie `int` und `double`, die Zahlen repräsentieren, ziehen wir hingegen den expliziten Typ vor. So ist leichter zu erkennen, ob es sich um Fließkomma- oder ganze Zahlen handelt.

<a name="This"/>
## Verzicht auf this

Auf die Verwendung von this wird vollständig verzichtet. Durch die Verwendung des Unterstrich `_` für private Felder gibt es hierzu auch keinen Zwang mehr.

<a name="Private"/>
## Verwendung des "private"-Modifizierers

Obwohl auf ihn ebenso verzichtet werden könnte, da alles ohne Modifizierer per default `private` ist, verwenden wir ihn für alle privaten Elemente. Grund ist der schnellere Überblick über öffentliche und nicht-öffentliche Elemente beim Einlesen in neuen Code, wenn die nicht-öffentlichen auch direkt als solche explizit gekennzeichnet sind.

<a name="Leerzeilen"/>
## Leerzeilen

Nach öffnenden und vor schließenden Klammern werden keine Leerzeilen eingefügt.

**Falsch:**

    public class Foo
    {

        private int _bar;
        
        public Foo(int bar)
        {

            _bar = bar;

        }

    }

**Richtig:**

    public class Foo
    {
        private int _bar;
        
        public Foo(int bar)
        {
            _bar = bar;
        }
    }

Innerhalb von Methoden werden Leerzeilen möglichst so eingesetzt, dass einzelne Funktionsblöcke innerhalb einer Methode separiert werden.

So würde z.B. Folgendes (bezogen auf die Formatierung) Sinn machen:

	public class Foo
	{
		private int _bar;

		public int Foo(int bar)
		{
			int foo = bar * 2;
			_bar = foo;
						
			return foo;
		}
	}

<a name="LOC"/>
## Lines of Code (LOC) in Klassen und Methoden

Ausnahmen bestätigen die Regel, die besagt: eine Klasse mit mehr als ca. 150 Zeilen Code ist schwieriger zu verstehen, als eine Klasse mit weniger Zeilen.

Nicht immer ist das einzuhalten, häufig aber macht das Aufteilen in mehrere Klassen durchaus Sinn (Single Responsibility Principle).

Laut "Clean Code" sollte eine Methode nicht mehr als 8 Zeilen Code enthalten. Das ist in der Praxis kaum möglich. Jedoch lässt sich wohl auch hier darauf achten, dass die prozedurale Abarbeitung "einer" Aufgabe in einer großen Methode häufig in viele kleine Methoden aufgebrochen werden kann.

<a name="Kommentare"/>
## Kommentare

Wir versuchen den Code grundsätzlich so zu schreiben, dass er sowohl von der Benennung der Objekte als auch von der konkreten Implementierung her leicht lesbar und damit verständlich ist.

Deshalb ist es häufig nicht sinnvoll, Code intensiv zu dokumentieren, da er für sich selbst sprechen sollte.

Erscheint beispielsweise die Implementierung einer Methode derart komplex, dass Kommentare sinnvoll werden, sollte zunächst geprüft werden, ob die Methode nicht besser in mehrere kleine Methoden aufgebrochen werden kann.

Ist dies nicht möglich oder erscheinen Kommentare dennoch sinnvoll, dann eigenen sich XML-Kommentare in C# am besten. Das gilt im Übrigen natürlich auch für Komponenten oder Libaries, die häufig von Dritten eingesetzt werden. Hier dienen XML-Kommentare innerhalb der üblichen Entwicklungsumgebungen als Quelle für die Hilfe-Texte, die die IntelliSense-Funktionen zur Verfügung stellen.

	/// <summary>
	/// Bar!
	/// </summary>
	public class Foo
	{
	}
