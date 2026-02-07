# Nothing To-Do — Setup-Anleitung

## So funktioniert's

Die App speichert deine Aufgaben automatisch als `nothing_todos.json` in deinem Google Drive.
Beim Öffnen wird diese Datei geladen, bei jeder Änderung automatisch aktualisiert.
Auf jedem Gerät mit dem gleichen Google-Konto sind deine Aufgaben sofort verfügbar.

```
Gerät A (Phone)  ──┐
                    ├──▶  Google Drive: nothing_todos.json
Gerät B (Windows) ──┘
```

---

## Einrichtung (einmalig, ca. 5 Minuten)

### Schritt 1: Google Cloud Projekt erstellen

1. Öffne https://console.cloud.google.com/
2. Oben links auf das Projekt-Dropdown → **Neues Projekt**
3. Name: `Nothing ToDo` → **Erstellen**
4. Warte bis das Projekt erstellt ist und wähle es aus

### Schritt 2: Google Drive API aktivieren

1. Im linken Menü: **APIs & Services** → **Library**
2. Suche nach `Google Drive API`
3. Klicke darauf → **Enable**

### Schritt 3: OAuth Consent Screen einrichten

1. **APIs & Services** → **OAuth consent screen**
2. Wähle **External** → **Create**
3. Fülle aus:
   - App name: `Nothing ToDo`
   - User support email: Deine E-Mail
   - Developer contact: Deine E-Mail
4. Klicke **Save and Continue** (Scopes überspringen)
5. Unter "Test users" → **Add Users** → Deine E-Mail hinzufügen
6. **Save and Continue** → **Back to Dashboard**
7. Optional: Klicke **Publish App** (damit der Token nicht nach 7 Tagen abläuft)

### Schritt 4: OAuth Client ID erstellen

1. **APIs & Services** → **Credentials**
2. **+ Create Credentials** → **OAuth client ID**
3. Application type: **Web application**
4. Name: `Nothing ToDo Web`
5. **Authorized JavaScript origins** — hier alle URLs eintragen, von wo du die App öffnest:
   ```
   http://localhost:8080
   http://localhost:3000
   https://DEINNAME.github.io
   ```
6. **Create** → **Client ID kopieren**

### Schritt 5: Client ID in die App eintragen

Öffne `index.html` in einem Texteditor. Ganz oben findest du:

```html
<meta name="google-client-id" content="DEINE_CLIENT_ID_HIER.apps.googleusercontent.com">
```

Ersetze den Platzhalter mit deiner Client ID. Speichern.

### Schritt 6: App hosten

Die App muss über einen Webserver laufen (nicht als lokale Datei).

**Option A — GitHub Pages (empfohlen, kostenlos):**
1. Erstelle ein Repository auf https://github.com/new (kann privat sein)
2. Lade alle 5 Dateien hoch
3. Settings → Pages → Branch: main → Save
4. URL: `https://DEINNAME.github.io/REPONAME/`
5. Diese URL auch unter "Authorized JavaScript origins" in Schritt 4 eintragen

**Option B — Netlify (am einfachsten):**
1. Gehe zu https://app.netlify.com/drop
2. Ziehe den Ordner mit allen Dateien rein
3. Fertig — URL unter Origins eintragen

**Option C — Lokal testen:**
```bash
cd /pfad/zum/ordner
python3 -m http.server 8080
# Dann: http://localhost:8080
```

---

## Nutzung

### Erster Start
1. URL öffnen → **Mit Google anmelden** → Konto wählen → Zugriff erlauben
2. Die App erstellt automatisch `nothing_todos.json` in deinem Drive

### Aufgaben verwalten
- **+** Button oder Taste **N** → Neue Aufgabe
- **○** Kreis antippen → Aufgabe erledigt (wandert zu "Done")
- **↺** in Done → Aufgabe wiederherstellen
- **🗑** → Aufgabe löschen

### Sync-Status (roter Punkt oben links)
- 🔴 pulsierend → Synchronisiert gerade
- 🟢 grün → Alles gespeichert
- 🟡 gelb → Offline-Modus
- 🔴 statisch → Fehler

### Automatischer Sync
- Jede Änderung wird nach 1,5 Sekunden automatisch in Drive gespeichert
- Beim Öffnen der App wird von Drive geladen
- Beim Wechsel zurück zur App (Tab-Switch, Phone entsperren) → Re-Sync
- Bei abgelaufenem Token → automatische stille Erneuerung

### Zweites Gerät
1. Selbe URL öffnen
2. Mit demselben Google-Konto anmelden
3. Alle Aufgaben sind da

### Als App installieren
- **Android (Chrome):** Menü → "App installieren" oder Banner unten
- **Windows (Chrome/Edge):** ⊕ in der Adressleiste → Installieren

---

## Dateien

| Datei | Beschreibung |
|-------|-------------|
| `index.html` | Die komplette App |
| `sw.js` | Service Worker (Offline) |
| `manifest.json` | PWA-Manifest |
| `icon-192.png` | App-Icon 192px |
| `icon-512.png` | App-Icon 512px |

Gesamtgröße: **~37 KB**

---

## Tastenkürzel (Desktop)

| Taste | Aktion |
|-------|--------|
| **N** | Neue Aufgabe |
| **Enter** | Bestätigen |
| **Esc** | Schließen |

---

## FAQ

**Die Google-Warnung "App nicht verifiziert" erscheint?**
Normal bei eigenen Apps. Klicke "Erweitert" → "Zu Nothing ToDo wechseln (unsicher)". Das ist sicher.

**Token läuft nach 7 Tagen ab?**
Klicke in der Cloud Console auf "Publish App" beim OAuth Consent Screen.

**Wo liegt die JSON-Datei?**
Im Root deines Google Drive. Du kannst sie dort auch sehen.

**Gleichzeitig auf zwei Geräten bearbeitet?**
Beim nächsten Öffnen werden die Daten zusammengeführt (Merge). Neuere Änderungen gewinnen.

**Funktioniert die App offline?**
Ja, dank Service Worker und localStorage. Sync passiert automatisch wenn du wieder online bist.

---

*Minimalistisch. Synchronisiert. Nothing.* 🔴⚫
