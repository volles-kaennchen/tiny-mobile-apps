---
author: volles-kaennchen
created: 29-05-2026 18:02:32
updated: 2026-06-08T22:46:28+02:00
topic: Baukasten
tags:
  - JavaScript
  - html
---

# 🧠 Migräne-Protokoll – Personalisierung

> [!abstract] Einleitung
> Dieser Baustein dokumentiert die personalisierbaren Inhalte des Migräne-Protokolls.
> Der vollständige Code liegt in der `index.html` des Ordners.

---

## 📌 Inhalt definieren
```
Dauer (in Stunden):
[               ]

Stärke (Einzelauswahl)
--------------------------
[ ] Leicht   [ ] Mittel   [ ] Stark

Lokalisierung (Einzelauswahl)
--------------------------
[ ] Einseitig   [ ] Beidseitig

Art der Beschwerden:
--------------------------
[ ] Pulsierend
[ ] Drückend
[ ] Stechend

Welches Auge ist betroffen?
--------------------------
[ ] Linkes Auge   [ ] Rechtes Auge

Begleitsymptome & Aura
--------------------------
[ ] Glaskörperchentrübung
[ ] Blitze, Zacken & Reflektionen
[ ] Verschwommen sehen
[ ] Lichtempfindlich
[ ] Verlangsamte Reaktionszeit
[ ] Schwindel
[ ] Übelkeit / Erbrechen
[ ] Nacken / Rückenschmerzen
```


## 🎨 Visuelle Ansicht



Hier ist Platz für ein Images des Layouts.



##  💎 Die Code Bausteine

#### HTML
```html
<div id="migraene-panel" class="sub-panel hidden">

    <label for="migraene-dauer">Dauer (in Stunden):</label>

    <input type="number" id="migraene-dauer" class="inline-input" min="0" step="0.5" placeholder="bitte eingeben">

    <div class="section-divider">Stärke (Einzelauswahl)</div>

    <div class="checkbox-row">
        <label class="checkbox-label"><input type="checkbox" class="migraene-staerke" value="Leicht" onclick="handleSingleSelection('.migraene-staerke', this)"> Leicht</label>
        <label class="checkbox-label"><input type="checkbox" class="migraene-staerke" value="Mittel" onclick="handleSingleSelection('.migraene-staerke', this)"> Mittel</label>
        <label class="checkbox-label"><input type="checkbox" class="migraene-staerke" value="Stark" onclick="handleSingleSelection('.migraene-staerke', this)"> Stark</label>
    </div>

    <div class="section-divider">Lokalisierung (Einzelauswahl)</div>

    <div class="checkbox-row">
        <label class="checkbox-label"><input type="checkbox" id="migraene-einseitig" class="migraene-lokal" onclick="handleSingleSelection('.migraene-lokal', this)"> Einseitig</label>
        <label class="checkbox-label"><input type="checkbox" id="migraene-beidseitig" class="migraene-lokal" onclick="handleSingleSelection('.migraene-lokal', this)"> Beidseitig</label>
    </div>

    <label style="margin-top: 10px;">Art der Beschwerden:</label>

    <label class="checkbox-label"><input type="checkbox" id="migraene-pulsierend"> Pulsierend</label>
    <label class="checkbox-label"><input type="checkbox" id="migraene-drueckend"> Drückend</label>
    <label class="checkbox-label"><input type="checkbox" id="migraene-stechend"> Stechend</label>

    <label style="margin-top: 10px;">Welches Auge ist betroffen?</label>

    <div class="checkbox-row">
        <label class="checkbox-label"><input type="checkbox" id="migraene-auge-links"> Linkes Auge</label>
        <label class="checkbox-label"><input type="checkbox" id="migraene-auge-rechts"> Rechtes Auge</label>
    </div>

    <div class="section-divider">Begleitsymptome & Aura</div>

    <label class="checkbox-label"><input type="checkbox" id="migraene-glaskoerper"> Glaskörperchentrübung</label>
    <label class="checkbox-label"><input type="checkbox" id="migraene-blitze"> Blitze, Zacken & Reflektionen</label>
    <label class="checkbox-label"><input type="checkbox" id="migraene-verschwommen"> Verschwommen sehen</label>
    <label class="checkbox-label"><input type="checkbox" id="migraene-licht"> Lichtempfindlich</label>
    <label class="checkbox-label"><input type="checkbox" id="migraene-verlangsamt"> Verlangsamte Reaktionszeit</label>
    <label class="checkbox-label"><input type="checkbox" id="migraene-schwindel"> Schwindel</label>
    <label class="checkbox-label"><input type="checkbox" id="migraene-uebelkeit"> Übelkeit / Erbrechen</label>
    <label class="checkbox-label"><input type="checkbox" id="migraene-nacken"> Nacken / Rückenschmerzen</label>

</div>
```

#### JavaScript

#### Regeln Code und Design:
- Stärke → Einzelauswahl (nur eine der 3 Optionen wählbar)
- Lokalisierung → Einzelauswahl (nur eine der 2 Optionen wählbar)
- Alle anderen Checkboxen ohne Begrenzung

```js
// Panel zurücksetzen (in togglePanel)
} else if (panelId === 'migraene-panel') {

    document.getElementById("migraene-dauer").value = "";
    document.querySelectorAll(".migraene-staerke").forEach(cb => cb.checked = false);
    document.getElementById("migraene-einseitig").checked = false;
    document.getElementById("migraene-beidseitig").checked = false;
    document.getElementById("migraene-pulsierend").checked = false;
    document.getElementById("migraene-drueckend").checked = false;
    document.getElementById("migraene-stechend").checked = false;
    document.getElementById("migraene-auge-links").checked = false;
    document.getElementById("migraene-auge-rechts").checked = false;
    document.getElementById("migraene-glaskoerper").checked = false;
    document.getElementById("migraene-blitze").checked = false;
    document.getElementById("migraene-verschwommen").checked = false;
    document.getElementById("migraene-licht").checked = false;
    document.getElementById("migraene-verlangsamt").checked = false;
    document.getElementById("migraene-schwindel").checked = false;
    document.getElementById("migraene-uebelkeit").checked = false;
    document.getElementById("migraene-nacken").checked = false;

}

// Daten laden (in selectDay)
const migraeneAktiv = data.migraeneAktiv || false;

document.getElementById("regler-migraene").checked = migraeneAktiv;

if (migraeneAktiv) {

    document.getElementById("migraene-panel").classList.remove("hidden");

    const mig = data.migraeneDetails || {};

    document.getElementById("migraene-dauer").value = mig.dauer || "";
    document.getElementById("migraene-einseitig").checked = mig.einseitig || false;
    document.getElementById("migraene-beidseitig").checked = mig.beidseitig || false;
    document.getElementById("migraene-pulsierend").checked = mig.pulsierend || false;
    document.getElementById("migraene-drueckend").checked = mig.drueckend || false;
    document.getElementById("migraene-stechend").checked = mig.stechend || false;
    document.getElementById("migraene-auge-links").checked = mig.augeLinks || false;
    document.getElementById("migraene-auge-rechts").checked = mig.augeRechts || false;
    document.getElementById("migraene-glaskoerper").checked = mig.glaskoerper || false;
    document.getElementById("migraene-blitze").checked = mig.blitze || false;
    document.getElementById("migraene-verschwommen").checked = mig.verschwommen || false;
    document.getElementById("migraene-licht").checked = mig.licht || false;
    document.getElementById("migraene-verlangsamt").checked = mig.verlangsamt || false;
    document.getElementById("migraene-schwindel").checked = mig.schwindel || false;
    document.getElementById("migraene-uebelkeit").checked = mig.uebelkeit || false;
    document.getElementById("migraene-nacken").checked = mig.nacken || false;

    const savedStaerke = mig.staerke || "";
    document.querySelectorAll(".migraene-staerke").forEach(cb => {
        cb.checked = (cb.value === savedStaerke);
    });

}

// Daten speichern (in saveData)
const migraeneAktiv = document.getElementById("regler-migraene").checked;

const migraeneDetails = {};

if (migraeneAktiv) {

    migraeneDetails.dauer = document.getElementById("migraene-dauer").value;

    const staerkeObj = document.querySelector(".migraene-staerke:checked");
    migraeneDetails.staerke = staerkeObj ? staerkeObj.value : "";

    migraeneDetails.einseitig = document.getElementById("migraene-einseitig").checked;
    migraeneDetails.beidseitig = document.getElementById("migraene-beidseitig").checked;
    migraeneDetails.pulsierend = document.getElementById("migraene-pulsierend").checked;
    migraeneDetails.drueckend = document.getElementById("migraene-drueckend").checked;
    migraeneDetails.stechend = document.getElementById("migraene-stechend").checked;
    migraeneDetails.augeLinks = document.getElementById("migraene-auge-links").checked;
    migraeneDetails.augeRechts = document.getElementById("migraene-auge-rechts").checked;
    migraeneDetails.glaskoerper = document.getElementById("migraene-glaskoerper").checked;
    migraeneDetails.blitze = document.getElementById("migraene-blitze").checked;
    migraeneDetails.verschwommen = document.getElementById("migraene-verschwommen").checked;
    migraeneDetails.licht = document.getElementById("migraene-licht").checked;
    migraeneDetails.verlangsamt = document.getElementById("migraene-verlangsamt").checked;
    migraeneDetails.schwindel = document.getElementById("migraene-schwindel").checked;
    migraeneDetails.uebelkeit = document.getElementById("migraene-uebelkeit").checked;
    migraeneDetails.nacken = document.getElementById("migraene-nacken").checked;

}

// In appData-Objekt
migraeneAktiv: migraeneAktiv,

migraeneDetails: migraeneAktiv ? migraeneDetails : null,
```
