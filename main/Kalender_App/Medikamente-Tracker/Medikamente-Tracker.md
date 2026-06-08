---
author: volles-kaennchen
created: 29-05-2026 18:02:32
updated: 2026-06-08T22:35:10+02:00
topic: Baukasten
tags:
  - JavaScript
  - html
---

# 💊 Medikamente-Tracker – Personalisierung

> [!abstract] Einleitung
> Dieser Baustein dokumentiert die personalisierbaren Inhalte des Medikamente-Trackers.
> Der vollständige Code liegt in der `index.html` des  Ordners.

---

## 📌 Inhalt des Medikamente-Tracker

```
[ ] 1x Medikament1 (mg) morgens
[ ] 1x Medikament2 (mg) abends
[ ] 1x Medikament3 (mg)
[ ] 4x Globuli1
[ ] 4x Globuli2
[ ] 1x Vitamin1 (Einheiten)
[ ] 1x Schmerzmittel1 (mg)
[ ] 1x Schmerzmittel2 (mg)
```

## 🎨 Visuelles Design


Hier ist Platz für ein Images des Layouts.



##  💎 Die Code Bausteine

#### HTML
```html
<div id="medikamente-panel" class="sub-panel hidden">

    <label class="checkbox-label"><input type="checkbox" class="med-checkbox" value="ASS100">1x Medikament1 (mg) morgens</label>

    <label class="checkbox-label"><input type="checkbox" class="med-checkbox" value="Atorvastatin">1x Medikament2 (mg) abends</label>

    <label class="checkbox-label"><input type="checkbox" class="med-checkbox" value="DesloratadinADGC">1x Medikament3 (mg)</label>

    <label class="checkbox-label"><input type="checkbox" class="med-checkbox" value="ChinaC30">4x Globuli1</label>

    <label class="checkbox-label"><input type="checkbox" class="med-checkbox" value="ByroniaC30">4x Globuli2</label>

    <label class="checkbox-label"><input type="checkbox" class="med-checkbox" value="Dekristol">1x Vitamin1 (Einheiten)</label>

    <label class="checkbox-label"><input type="checkbox" class="med-checkbox" value="Novaminsulfon">1x Schmerzmittel1 (mg)</label>

    <label class="checkbox-label"><input type="checkbox" class="med-checkbox" value="Ibuprofen">1x Schmerzmittel2 (mg)</label>

</div>
```

### JavaScript

#### Regeln Code und Design:
- Es soll keine Begrenzung an der Auswahl der Checkboxen geben. (alles darf angehakt werden)

```js
// Panel zurücksetzen (in togglePanel)
} else if (panelId === 'medikamente-panel') {

    document.querySelectorAll(".med-checkbox").forEach(cb => cb.checked = false);

}

// Daten laden (in selectDay)
const medikamenteAktiv = data.medikamenteAktiv || false;

document.getElementById("regler-medikamente").checked = medikamenteAktiv;

if (medikamenteAktiv) {

    document.getElementById("medikamente-panel").classList.remove("hidden");

    const savedMeds = data.medikamente || [];

    document.querySelectorAll(".med-checkbox").forEach(cb => {
        cb.checked = savedMeds.includes(cb.value);
    });

}

// Daten speichern (in saveData)
const medikamenteAktiv = document.getElementById("regler-medikamente").checked;

const savedMeds = [];

if (medikamenteAktiv) {

    document.querySelectorAll(".med-checkbox:checked").forEach(cb => savedMeds.push(cb.value));

}

// In appData-Objekt
medikamenteAktiv: medikamenteAktiv,

medikamente: savedMeds,
```
