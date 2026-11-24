# Aufgabe KM3-04 — CSS-Selektoren

Name: Stefan Barbut (3CAIF) &nbsp;&nbsp;&nbsp; Abgabe: nachgeholt

Dateien:

- [`demo.html`](demo.html) / [`demo.css`](demo.css): Kopie des Selektoren-Labors, erweitert um Abschnitt **8 · Eigene Erweiterung** (Beispiel 1)
- [`loesung.html`](loesung.html) / [`loesung.css`](loesung.css): Beispiel 3 (HTML aus der Angabe unverändert)

---

## Beispiel 1: Das Labor erweitern

Die neue Struktur steht in `demo.html` im Abschnitt `<section id="erweiterung">`.
An Zeilen und Listeneinträgen gibt es keine Klassen oder IDs.

**a)** Speisekarte (Kopfzeile + 6 Gerichte)

```
1) #erweiterung thead th
2) #erweiterung tbody tr:nth-child(even)
3) #erweiterung tbody tr:last-child
```

**b)** Geordnete Liste mit 5 Schritten

```
2. Schritt: #erweiterung ol li:nth-child(2)      vorletzter: #erweiterung ol li:nth-last-child(2)
```

**c)** Zwei `<div class="kasten">`, nur der mit Bild wird formatiert:

```css
#erweiterung .kasten:has(img) { border-left: 6px solid var(--ok); }
```

Warum `:has()` und nicht `:is()`? `:has(img)` schaut **in** das Element hinein.
Es trifft den Kasten, wenn darin ein `img` steckt, und funktioniert damit wie ein
Eltern-Selektor. `:is()` prüft nur, ob das Element **selbst** zu einem der
angegebenen Selektoren passt. `.kasten:is(img)` würde ein Element verlangen, das
gleichzeitig `.kasten` und `img` ist, und trifft deshalb keinen der beiden Kästen.

---

## Beispiel 2: Selbsttest im Prüfungsformat

- [x] Ein Selektor wie `p.hinweis` trifft nur Absätze, die diese Klasse tragen.
- [x] `h2 ~ p` trifft alle Absätze nach einer `h2`, die dieselbe Ebene haben.
- [ ] `:nth-child(2)` und `:nth-of-type(2)` treffen immer dasselbe Element.
- [x] Bei gleichem Gewicht gewinnt die später notierte Regel.

**a)** Aussage 3 ist falsch. `:nth-child(2)` zählt **alle** Geschwister-Elemente,
egal welcher Tag. `:nth-of-type(2)` zählt nur Geschwister **desselben Tags**.
Beispiel: `<h3>` · `<p>A</p>` · `<p>B</p>`. Hier trifft `p:nth-child(2)` den
Absatz A, weil er das zweite Kind ist. `p:nth-of-type(2)` trifft dagegen den
Absatz B, weil er der zweite `p` ist. Die beiden Selektoren treffen also
verschiedene Elemente.

**b)**

| Ziel | Mein Selektor |
|------|---------------|
| alle Links, deren Ziel auf `.pdf` endet | `a[href$=".pdf"]` |
| alle Absätze außer denen mit der Klasse `hinweis` | `p:not(.hinweis)` |
| das erste `li` in jeder `ul` | `ul > li:first-child` |
| jedes Element, das ein `img` enthält | `:has(img)` |
| das Häkchen, das direkt vor einem `label` steht | `input[type="checkbox"]:has(+ label)` |

Zur letzten Zeile: Gesucht ist das Häkchen selbst, also die Checkbox, und nicht
das `label`. `:has(+ label)` trifft das Element, dessen direkter Nachbar ein
`label` ist. Wenn stattdessen das Label eines angehakten Kästchens gemeint ist,
lautet der Selektor `input[type="checkbox"]:checked + label`.

**c)** Der Fehler: `:nth-child(1)` zählt alle Kinder des Containers. Steht vor
dem ersten Absatz z. B. eine Überschrift, ist der Absatz nicht das erste Kind
und die Regel trifft gar nichts. Richtig:

```css
/* Der erste Absatz im Container soll fett sein. */
p:first-of-type { font-weight: bold; }
```

---

## Beispiel 3: Transfer (Prüfungsaufgabe)

Die Lösung steht in [`loesung.css`](loesung.css), das HTML in [`loesung.html`](loesung.html) ist unverändert.

```css
/* a) */
article.testbereich section[data-topic^="css-adv"] {
    border: 2px dashed red;
}

/* b) */
article.testbereich section[data-topic^="css-adv"] li:nth-child(3) > a {
    color: green;
    font-weight: bold;
    transition: color 1s;
}
article.testbereich section[data-topic^="css-adv"] li:nth-child(3) > a:hover {
    color: red;
}

/* c) Kopfzeile ist Zeile 1 → Hauptgericht ist Zeile 3 */
table.preise tr:nth-child(3) { background: #fff4cc; }
table.preise tr:last-child   { border-top: 2px solid #888888; }
```

**d)** Das Attribut `data-topic` steht schon im HTML und beschreibt das Thema der
Sektion. Mit `[data-topic^="css-adv"]` brauche ich deshalb keine zusätzliche Klasse
im HTML, die das HTML hier ohnehin nicht ändern darf. Der Präfix-Selektor `^=`
trifft außerdem automatisch **alle** Sektionen, deren Thema mit `css-adv` beginnt,
auch solche, die später dazukommen.
