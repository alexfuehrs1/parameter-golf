# Parameter Golf Project Skills

Übersicht aller bisher für dieses Projekt definierten Skills mit Zweck, Einsatzbereich und konkretem Nutzen.

Dieses Dokument fasst die projektspezifischen Skills zusammen, die wir für den Parameter-Golf-Workflow definiert haben: Research-Steuerung, Compliance, Experiment-Tracking, Systems-Optimierung und sichere Implementierung.

## Skill-Gruppen

- **Research Core:** Steuert Strategie, Experimente, Laufzeit, Artefaktbudget und Compliance.  
  `benchmark-fidelity`, `proxy-experimentation`, `artifact-budget-discipline`, `reproducible-experiments`, `submission-compliance`, `hypothesis-driven-ablations`, `train-fast-fail-fast`, `evaluation-overfitting-guard`, `performance-profiling`

- **Research Memory:** Sichert Lernen aus Experimenten statt bloßer Metrik-Sammlungen.  
  `experiment-documentation-and-analysis`

- **Engineering Safety:** Hält Codex-Änderungen klein, testbar und nachvollziehbar.  
  `tdd-red-green-refactor`, `minimal-diff-implementation`, `verify-before-finish`

---

## Research Core

Steuert Strategie, Experimente, Laufzeit, Artefaktbudget und Compliance.

### `benchmark-fidelity`

**Zweck:** Hält alle Entscheidungen am echten Challenge-Ziel ausgerichtet statt an bequemen Proxy-Metriken.  
**Wann einsetzen:** Vor Architektur-, Quantisierungs-, Tokenizer- und Eval-Entscheidungen.

**Was der Skill konkret kann:**
- trennt echte Zielmetrik, Proxy-Metrik und Engineering-Metrik
- erzwingt Begründung, warum ein Proxy mit BPB korrelieren sollte
- markiert Fidelity-Risiken, wenn lokale Eval vom offiziellen Setup abweicht

### `proxy-experimentation`

**Zweck:** Reduziert Compute-Verschwendung durch billige Falsifikation vor teuren Runs.  
**Wann einsetzen:** Immer vor 8xH100-Läufen oder längeren Ablationen.

**Was der Skill konkret kann:**
- entwirft die günstigste Proxy-Version eines Experiments
- definiert Go/No-Go-Kriterien vor Full Runs
- erzwingt Eskalationspfad von Smoke Test bis Vollbudget

### `artifact-budget-discipline`

**Zweck:** Behandelt die 16-MB-Grenze als First-Class-Constraint.  
**Wann einsetzen:** Bei jeder Änderung an Modellgröße, Quantisierung, Packaging oder Export.

**Was der Skill konkret kann:**
- schätzt Einfluss auf Code-Bytes und Modell-Bytes
- macht Größenrisiken früh sichtbar
- bewertet Serialisierung, Kompression und Quantisierung gemeinsam

### `reproducible-experiments`

**Zweck:** Macht Runs vergleichbar, wiederholbar und auswertbar.  
**Wann einsetzen:** Bei jedem Proxy- und Full-Run.

**Was der Skill konkret kann:**
- erzwingt strukturierte Run-Logs mit Revision, Seed, Setup und Ergebnis
- macht Konfigurationsdrift sichtbar
- erleichtert saubere Baseline-Vergleiche

### `submission-compliance`

**Zweck:** Senkt Disqualifikationsrisiken und prüft Submission-Vollständigkeit.  
**Wann einsetzen:** Vor jedem `submit-ready`-Claim und bei eval-seitigen Tricks.

**Was der Skill konkret kann:**
- prüft Pflichtbestandteile wie Weights, Config, Log, Script und Write-up
- markiert Leakage-, Runtime- und Artefakt-Risiken
- erzwingt konservative Einschätzung bei unklarer Regel-Lage

### `hypothesis-driven-ablations`

**Zweck:** Sichert interpretierbare Experimente statt unklarer Change-Bundles.  
**Wann einsetzen:** Für alle Ablations- und Tuning-Runs.

**Was der Skill konkret kann:**
- erzwingt pro Experiment eine Haupt-Hypothese
- trennt Mechanismus, erwartete Metrikbewegung und Alternativerklärungen
- verhindert unklare Attribution bei vielen Änderungen zugleich

### `train-fast-fail-fast`

**Zweck:** Findet Defekte früh, bevor Zeit in lange Trainingsläufe fließt.  
**Wann einsetzen:** Direkt nach Codeänderungen am Trainingspfad.

**Was der Skill konkret kann:**
- führt frühe Checks für Import, Config, Forward/Backward und Optimizer-Step ein
- identifiziert frühe Abbruchbedingungen
- deckt numerische Instabilität und Shape-Probleme schnell auf

### `evaluation-overfitting-guard`

**Zweck:** Bewertet Eval-Tricks kritisch auf Robustheit und Regelkonformität.  
**Wann einsetzen:** Bei Sliding Eval, TTT, Caches und sonstigen Eval-seitigen Optimierungen.

**Was der Skill konkret kann:**
- prüft, ob nur erlaubte Information verwendet wird
- trennt echte Modellverbesserung von Eval-spezifischem Exploit
- markiert Leakage- und Overfitting-Risiken explizit

### `performance-profiling`

**Zweck:** Optimiert End-to-End-Wallclock statt nur theoretischer FLOPs.  
**Wann einsetzen:** Bei Throughput-, Loader- und Systems-Optimierung.

**Was der Skill konkret kann:**
- zerlegt Laufzeit in Startup, Loader, Forward, Backward, Optimizer, Sync und Eval
- identifiziert den größten Bottleneck
- schätzt realistische Zeitgewinne einzelner Maßnahmen

---

## Research Memory

Sichert Lernen aus Experimenten statt bloßer Metrik-Sammlungen.

### `experiment-documentation-and-analysis`

**Zweck:** Dokumentiert jede Änderung mit Kausalbewertung und nächstem Schritt.  
**Wann einsetzen:** Nach jedem Experiment oder beobachteter Regression/Verbesserung.

**Was der Skill konkret kann:**
- fasst Änderung, Hypothese, Setup, Ergebnis und Vergleich zusammen
- trennt Signal von Rauschen
- übersetzt Resultate in strategische Entscheidungen

---

## Engineering Safety

Hält Codex-Änderungen klein, testbar und nachvollziehbar.

### `tdd-red-green-refactor`

**Zweck:** Erzwingt einen disziplinierten TDD-Zyklus für stabile Infrastrukturarbeit.  
**Wann einsetzen:** Für Harnesses, Evaluationscode, Packaging, Logging und Utilities.

**Was der Skill konkret kann:**
- startet mit fehlschlagendem Test
- führt zur minimalen Implementierung bis grün
- erlaubt Refactor erst nach abgesicherter Funktionalität

### `minimal-diff-implementation`

**Zweck:** Hält Änderungen klein, reviewbar und regressionsarm.  
**Wann einsetzen:** Bei fast jeder Implementierungsaufgabe.

**Was der Skill konkret kann:**
- begrenzt Scope auf das Nötigste
- vermeidet opportunistische Refactors
- senkt Risiko unbeabsichtigter Seiteneffekte

### `verify-before-finish`

**Zweck:** Verhindert voreilige `fertig`-Claims.  
**Wann einsetzen:** Vor Abschluss jeder Aufgabe oder jedes PR-Schritts.

**Was der Skill konkret kann:**
- prüft Tests, Sanity Checks, Runtime- und Size-Implikationen
- erzwingt Nennung offener Risiken
- stellt sicher, dass nur beabsichtigte Dateien geändert wurden

---

## Empfohlene Einsatzreihenfolge

1. **Startphase:** `benchmark-fidelity`, `proxy-experimentation`, `artifact-budget-discipline`, `reproducible-experiments`
2. **Vor größeren Läufen:** `hypothesis-driven-ablations`, `train-fast-fail-fast`, `performance-profiling`
3. **Vor Submission oder aggressiver Eval:** `submission-compliance`, `evaluation-overfitting-guard`, `verify-before-finish`
4. **Laufend:** `experiment-documentation-and-analysis` als Projektgedächtnis
5. **Für Infrastruktur und Refactors:** `tdd-red-green-refactor` und `minimal-diff-implementation`

---

## Hinweis

Die Liste bildet die bisher in diesem Projekt definierten Skills ab. Sie ist bewusst auf den Parameter-Golf-Workflow zugeschnitten und nicht als allgemeines Skill-Framework gedacht.
