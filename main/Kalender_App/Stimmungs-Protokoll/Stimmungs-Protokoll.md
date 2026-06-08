---
author: volles-kaennchen
created: 29-05-2026 18:02:32
updated: 2026-06-08T22:35:55+02:00
topic: Baukasten
tags:
  - JavaScript
  - html
---

# 😊 Stimmungs-Protokoll – Personalisierung

> [!abstract] Einleitung
> Dieser Baustein dokumentiert die personalisierbaren Inhalte des Stimmungs-Protokolls.
> Der vollständige Code liegt in der `index.html` des Ordners.

---

## 📌 Inhalt des Stimmungs-Protokoll

```
[ ] gut gelaunt         [ ] gereizt
[ ] tiefen entspannt    [ ] fatique
[ ] humorvoll           [ ] sensibel
[ ] sehr glücklich      [ ] niedergeschlagen
[ ] wissenshungrig      [ ] komisch
[ ] mutig               [ ] krank
```

## 🎨 Visuelles Design


Hier ist Platz für ein Images des Layouts.



##  💎 Die Code Bausteine

### HTML

```html
<div id="mood-panel" class="sub-panel hidden">

    <div class="checkbox-row">
        <label class="checkbox-label"><input type="checkbox" class="mood-checkbox" value="gut gelaunt">☺️ gut gelaunt</label>
        <label class="checkbox-label"><input type="checkbox" class="mood-checkbox" value="gereizt">😤 gereizt</label>
    </div>

    <div class="checkbox-row">
        <label class="checkbox-label"><input type="checkbox" class="mood-checkbox" value="tiefen entspannt">😌 tiefen entspannt</label>
        <label class="checkbox-label"><input type="checkbox" class="mood-checkbox" value="fatigue">🥱 fatigue</label>
    </div>

    <div class="checkbox-row">
        <label class="checkbox-label"><input type="checkbox" class="mood-checkbox" value="humorvoll">🤪 humorvoll</label>
        <label class="checkbox-label"><input type="checkbox" class="mood-checkbox" value="sensibel">😭 sensibel</label>
    </div>

    <div class="checkbox-row">
        <label class="checkbox-label"><input type="checkbox" class="mood-checkbox" value="sehr glücklich">🥹 sehr glücklich</label>
        <label class="checkbox-label"><input type="checkbox" class="mood-checkbox" value="niedergeschlagen">😔 niedergeschlagen</label>
    </div>

    <div class="checkbox-row">
        <label class="checkbox-label"><input type="checkbox" class="mood-checkbox" value="wissenshungrig">🤓 wissenshungrig</label>
        <label class="checkbox-label"><input type="checkbox" class="mood-checkbox" value="komisch">🫩 komisch</label>
    </div>

    <div class="checkbox-row">
        <label class="checkbox-label"><input type="checkbox" class="mood-checkbox" value="mutig">😎 mutig</label>
        <label class="checkbox-label"><input type="checkbox" class="mood-checkbox" value="krank">🤧 krank</label>
    </div>

</div>
```

### JavaScript

#### Regeln Code und Design:
- Es soll keine Begrenzung an der Auswahl der Checkboxen geben. (alles darf angehakt werden)

```js
// Panel zurücksetzen (in togglePanel)
} else if (panelId === 'mood-panel') {

    document.querySelectorAll(".mood-checkbox").forEach(cb => cb.checked = false);

}

// Daten laden (in selectDay)
const moodAktiv = data.moodAktiv || false;

document.getElementById("regler-mood").checked = moodAktiv;

if (moodAktiv) {

    document.getElementById("mood-panel").classList.remove("hidden");

    const savedMoods = data.allgemeinzustand || [];

    document.querySelectorAll(".mood-checkbox").forEach(cb => {
        cb.checked = Array.isArray(savedMoods) ? savedMoods.includes(cb.value) : (cb.value === savedMoods);
    });

}

// Daten speichern (in saveData)
const moodAktiv = document.getElementById("regler-mood").checked;

const savedMoods = [];

if (moodAktiv) {

    document.querySelectorAll(".mood-checkbox:checked").forEach(cb => savedMoods.push(cb.value));

}

// In appData-Objekt
moodAktiv: moodAktiv,

allgemeinzustand: savedMoods,
```
