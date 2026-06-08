---
author: volles-kaennchen
created: 29-05-2026 18:02:32
updated: 2026-06-08T22:46:40+02:00
topic: Baukasten
tags:
  - JavaScript
  - html
---

# 🩹 Neurodermitis-Protokoll – Personalisierung

> [!abstract] Einleitung
> Dieser Baustein dokumentiert die personalisierbaren Inhalte des Neurodermitis-Protokolls.
> Der vollständige Code liegt in der `index.html` des Ordners.

---

## 📌 Inhalt 

```
Zustand der Haut
--------------------------
[ ] hydrierte Haut     [ ] trockene Bereiche
[ ] schuppiges Ekzem   [ ] nässendes Ekzem

Beschwerden
--------------------------
[ ] Juckreiz     [ ] warme Haut

Lokalisation von Rötungen
[ ] ganzes Gesicht  [ ] Stirn & Augenbrauen
[ ] Wangen  [ ] Mundrose  [ ] Hals

Begleitsymptome
--------------------------
[ ] hormonelle Akne    [ ] verstopfte Poren
[ ] spannende Haut     [ ] stechende Haut
```


## 🎨 Visuelles Design


Hier ist Platz für ein Images des Layouts.



## 💎 Die Code Bausteine

### HTML
```html
<div id="neurodermitis-panel" class="sub-panel hidden">

    <div class="section-divider">Zustand der Haut</div>

    <div class="checkbox-row">
        <label class="checkbox-label"><input type="checkbox" id="neuro-hydriert"> hydrierte Haut</label>
        <label class="checkbox-label"><input type="checkbox" id="neuro-trocken"> trockene Bereiche</label>
    </div>

    <div class="section-divider">Beschwerden</div>

    <div class="checkbox-row">
        <label class="checkbox-label"><input type="checkbox" id="neuro-juckreiz"> Juckreiz</label>
        <label class="checkbox-label"><input type="checkbox" id="neuro-warm"> warme Haut</label>
    </div>

    <div class="checkbox-row">
        <label class="checkbox-label"><input type="checkbox" id="neuro-entzuendung"> Entzündungen</label>
        <label class="checkbox-label"><input type="checkbox" id="neuro-naessend"> nässendes Ekzem</label>
    </div>

    <div class="checkbox-row">
        <label class="checkbox-label"><input type="checkbox" id="neuro-schuppig"> schuppiges Ekzem</label>
    </div>

    <div class="section-divider">Lokalisation von Rötungen & Ekzem</div>

    <div class="checkbox-row">
        <label class="checkbox-label"><input type="checkbox" id="neuro-stirn"> Stirn & Augenbrauen</label>
        <label class="checkbox-label"><input type="checkbox" id="neuro-gesicht"> ganzes Gesicht</label>
    </div>

    <div class="checkbox-row">
        <label class="checkbox-label"><input type="checkbox" id="neuro-wangen"> Wangen</label>
        <label class="checkbox-label"><input type="checkbox" id="neuro-mundrose"> Mundrose</label>
        <label class="checkbox-label"><input type="checkbox" id="neuro-hals"> Hals</label>
    </div>

    <div class="section-divider">Begleitsymptome</div>

    <div class="checkbox-row">
        <label class="checkbox-label"><input type="checkbox" id="neuro-akne"> hormonelle Akne</label>
        <label class="checkbox-label"><input type="checkbox" id="neuro-poren"> verstopfte Poren</label>
    </div>

    <div class="checkbox-row">
        <label class="checkbox-label"><input type="checkbox" id="neuro-spannung"> spannende Haut</label>
        <label class="checkbox-label"><input type="checkbox" id="neuro-stechen"> stechende Haut</label>
    </div>

</div>
```


### JavaScript

#### Regeln Code und Design:
- Es soll keine Begrenzung an der Auswahl der Checkboxen geben. (alles darf angehakt werden)

```js
// Panel zurücksetzen (in togglePanel)
if (panelId === 'neurodermitis-panel') {

    document.getElementById("neuro-hydriert").checked = false;
    document.getElementById("neuro-trocken").checked = false;
    document.getElementById("neuro-entzuendung").checked = false;
    document.getElementById("neuro-naessend").checked = false;
    document.getElementById("neuro-schuppig").checked = false;
    document.getElementById("neuro-juckreiz").checked = false;
    document.getElementById("neuro-warm").checked = false;
    document.getElementById("neuro-gesicht").checked = false;
    document.getElementById("neuro-stirn").checked = false;
    document.getElementById("neuro-wangen").checked = false;
    document.getElementById("neuro-mundrose").checked = false;
    document.getElementById("neuro-hals").checked = false;
    document.getElementById("neuro-akne").checked = false;
    document.getElementById("neuro-poren").checked = false;
    document.getElementById("neuro-spannung").checked = false;
    document.getElementById("neuro-stechen").checked = false;

}

// Daten laden (in selectDay)
const neurodermitisAktiv = data.neurodermitisAktiv || false;

document.getElementById("regler-neurodermitis").checked = neurodermitisAktiv;

if (neurodermitisAktiv) {

    document.getElementById("neurodermitis-panel").classList.remove("hidden");

    const det = data.neurodermitisDetails || {};

    document.getElementById("neuro-hydriert").checked = det.hydriert || false;
    document.getElementById("neuro-trocken").checked = det.trocken || false;
    document.getElementById("neuro-entzuendung").checked = det.entzuendung || false;
    document.getElementById("neuro-naessend").checked = det.naessend || false;
    document.getElementById("neuro-schuppig").checked = det.schuppig || false;
    document.getElementById("neuro-juckreiz").checked = det.juckreiz || false;
    document.getElementById("neuro-warm").checked = det.warm || false;
    document.getElementById("neuro-gesicht").checked = det.gesicht || false;
    document.getElementById("neuro-stirn").checked = det.stirn || false;
    document.getElementById("neuro-wangen").checked = det.wangen || false;
    document.getElementById("neuro-mundrose").checked = det.mundrose || false;
    document.getElementById("neuro-hals").checked = det.hals || false;
    document.getElementById("neuro-akne").checked = det.akne || false;
    document.getElementById("neuro-poren").checked = det.poren || false;
    document.getElementById("neuro-spannung").checked = det.spannung || false;
    document.getElementById("neuro-stechen").checked = det.stechen || false;

}

// Daten speichern (in saveData)
const neurodermitisAktiv = document.getElementById("regler-neurodermitis").checked;

const neuroDetails = {};

if (neurodermitisAktiv) {

    neuroDetails.hydriert = document.getElementById("neuro-hydriert").checked;
    neuroDetails.trocken = document.getElementById("neuro-trocken").checked;
    neuroDetails.entzuendung = document.getElementById("neuro-entzuendung").checked;
    neuroDetails.naessend = document.getElementById("neuro-naessend").checked;
    neuroDetails.schuppig = document.getElementById("neuro-schuppig").checked;
    neuroDetails.juckreiz = document.getElementById("neuro-juckreiz").checked;
    neuroDetails.warm = document.getElementById("neuro-warm").checked;
    neuroDetails.gesicht = document.getElementById("neuro-gesicht").checked;
    neuroDetails.stirn = document.getElementById("neuro-stirn").checked;
    neuroDetails.wangen = document.getElementById("neuro-wangen").checked;
    neuroDetails.mundrose = document.getElementById("neuro-mundrose").checked;
    neuroDetails.hals = document.getElementById("neuro-hals").checked;
    neuroDetails.akne = document.getElementById("neuro-akne").checked;
    neuroDetails.poren = document.getElementById("neuro-poren").checked;
    neuroDetails.spannung = document.getElementById("neuro-spannung").checked;
    neuroDetails.stechen = document.getElementById("neuro-stechen").checked;

}

// In appData-Objekt
neurodermitisAktiv: neurodermitisAktiv,

neurodermitisDetails: neurodermitisAktiv ? neuroDetails : null,
```
