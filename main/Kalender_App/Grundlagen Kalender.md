---
author: volles-kaennchen
created: 08-06-2026 23:10:5
updated: 08-06-2026 23:10:5
topic: Flexbox
tags: #
---

# 📝 Grundlagen Kalender

>> [!abstract] Einleitung
>> Dieser Baustein dokumentiert die grundlegenden Funktionen des Kalenders. 

---

## 🛠️ **Kalenderansicht**

#### Regeln Code und Design:
- Monatsansicht Datum + Wochentag
- aktueller Tag hervorgehoben
- Wochentag beginnend mit Montag
- Kalender soll sich jeden Monat automatisch aktualisieren

### 🎨 Visuelles Design

-Screenshot einfügen-

###  💎 Die Code Bausteine vom Kalender

#### HTML 
```html
<div class="container">
    <h2>📅 Mein Kalender</h2>
    <div class="calendar-header">
        <button class="nav-btn" onclick="changeMonth(-1)">&lt;</button>
        <h3 id="calendar-month-year"></h3>
        <button class="nav-btn" onclick="changeMonth(1)">&gt;</button>
    </div>

    <table>
        <thead>
            <tr>
                <th>Mo</th><th>Di</th><th>Mi</th><th>Do</th><th>Fr</th><th>Sa</th><th>So</th>
            </tr>
        </thead>
        <tbody id="calendar-body"></tbody>
    </table>
</div>
```
#### JavaScript
```js
const monthNames = ["Januar", "Februar", "März", "April", "Mai", "Juni",
                    "Juli", "August", "September", "Oktober", "November", "Dezember"];

function generateCalendar() {
    const year = currentDate.getFullYear();
    const month = currentDate.getMonth();

    document.getElementById("calendar-month-year").innerText = `${monthNames[month]} ${year}`;

    // Woche beginnt Montag (0=So → 6, 1=Mo → 0, ...)
    let firstDayIndex = new Date(year, month, 1).getDay();
    firstDayIndex = firstDayIndex === 0 ? 6 : firstDayIndex - 1;

    const daysInMonth = new Date(year, month + 1, 0).getDate();
    const calendarBody = document.getElementById("calendar-body");
    calendarBody.innerHTML = "";

    let dateCounter = 1;
    const today = new Date();

    for (let i = 0; i < 6; i++) {
        let row = document.createElement("tr");
        let addedCells = false;

        for (let j = 0; j < 7; j++) {
            let cell = document.createElement("td");

            if (i === 0 && j < firstDayIndex) {
                cell.classList.add("empty");
            } else if (dateCounter > daysInMonth) {
                if (j > 0 && j < 7) {
                    cell.classList.add("empty");
                    row.appendChild(cell);
                }
                break;
            } else {
                cell.innerText = dateCounter;
                addedCells = true;

                const dayString = `${year}-${String(month + 1).padStart(2, '0')}-${String(dateCounter).padStart(2, '0')}`;
                cell.dataset.date = dayString;

                // Heutigen Tag hervorheben
                if (dateCounter === today.getDate() && month === today.getMonth() && year === today.getFullYear()) {
                    cell.classList.add("today");
                }

                // Ausgewählten Tag markieren
                if (dayString === selectedDateStr) {
                    cell.classList.add("selected-day");
                }

                // Punkt anzeigen wenn Daten vorhanden
                if (appData[dayString]) {
                    cell.classList.add("has-data");
                }

                cell.onclick = function() {
                    selectDay(this.dataset.date);
                };

                dateCounter++;
            }
            row.appendChild(cell);
        }
        if (addedCells) {
            calendarBody.appendChild(row);
        }
    }
}

function changeMonth(direction) {
    currentDate.setMonth(currentDate.getMonth() + direction);
    generateCalendar();
}
```

###  💎 Die Zeitbasierte Begrüßung

#### Regeln Code und Design:

- Zeitbasiere Begrüßung
	**Uhrzeiten & Begrüßungen:**
		- 05:00 – 11:59 Uhr → Guten Morgen! ☀️
		- 12:00 – 17:59 Uhr → Halli, Hallo, Hallöchen! 👋
		- 18:00 – 22:59 Uhr → Guten Abend! ✨
		- 23:00 – 04:59 Uhr → Nachti! 🌙

#### HTML 
```html
<h2 id="welcome-greeting" class="welcome-title">Guten Tag! 👋</h2>
```

#### JavaScript
```js
function updateGreeting() {
    const currentHour = new Date().getHours();
    let greetingText = "Hallo! 👋";

    if (currentHour >= 5 && currentHour < 12) {
        greetingText = "Guten Morgen! ☀️";
    } else if (currentHour >= 12 && currentHour < 18) {
        greetingText = "Halli, Hallo, Hallöchen! 👋";
    } else if (currentHour >= 18 && currentHour < 23) {
        greetingText = "Guten Abend! ✨";
    } else {
        greetingText = "Nachti! 🌙";
    }

    document.getElementById("welcome-greeting").innerText = greetingText;
}

// Aufruf beim Laden der Seite
window.onload = function() {
    updateGreeting();
    generateCalendar();
};
```

---

## 🛠️ **Sicherung der Daten**


### 🎨 Visuelles Design


Hier ist Platz für ein Images des Layouts.




#### Regeln Code und Design:

-  Speichern des ausgewählten Inhalts im Zwischenspeicher
- lokales speichern des Inhalts über Download Button als lesbare JSON Datei
- damit Exporte nicht überschrieben werden wird die Funktion ***exportDataAsFile()*** so angepasst, dass sie automatisch das aktuelle Tagesdatum des Exports (im Format JJJJ-MM-TT) ermittelt und an den Dateinamen anhängt (`tagebuch_export_2026-06-01.json`)


###  💎 Die Code Bausteine

#### HTML 
```html
<button type="button" class="btn-block btn-secondary" onclick="exportDataAsFile()">
    Als Datei (.json) exportieren
</button>
```

#### JavaScript
```js
// Lokaler Speicher — Daten laden beim Start
const STORAGE_KEY = "meine_tagebuch_app_daten";
let appData = JSON.parse(localStorage.getItem(STORAGE_KEY)) || {};

// Daten speichern in localStorage (in saveData)
localStorage.setItem(STORAGE_KEY, JSON.stringify(appData));
alert("Daten erfolgreich gespeichert!");
generateCalendar();

// Export als .json-Datei mit dynamischem Datum im Dateinamen
function exportDataAsFile() {
    if (Object.keys(appData).length === 0) {
        alert("Es gibt noch keine Daten zum Exportieren!");
        return;
    }

    // Heutiges Datum generieren (Format: JJJJ-MM-TT)
    const heute = new Date();
    const jahr = heute.getFullYear();
    const monat = String(heute.getMonth() + 1).padStart(2, '0');
    const tag = String(heute.getDate()).padStart(2, '0');
    const datumsString = `${jahr}-${monat}-${tag}`;

    const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(appData, null, 2));
    const downloadAnchor = document.createElement('a');
    downloadAnchor.setAttribute("href", dataStr);

    // Dateiname z.B.: tagebuch_export_2026-06-01.json
    downloadAnchor.setAttribute("download", `tagebuch_export_${datumsString}.json`);

    document.body.appendChild(downloadAnchor);
    downloadAnchor.click();
    downloadAnchor.remove();
}
```

---
