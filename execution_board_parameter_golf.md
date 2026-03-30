# Execution Board

## Parameter Golf Projekt

Operatives Board mit Tasks, Reihenfolge, Done-Kriterien und Stop/Go-Regeln. Abgeleitet aus der Quellensichtung und auf das Challenge-Ziel optimiert: niedrige **Roundtrip-BPB** bei harter **16-MB-Grenze** und **10+10-Minuten-Budget**.

## Leitprinzip

Zuerst Messung und Compliance stabilisieren, dann eine repo-nahe Baseline reproduzieren, danach Quantisierung, Loader/Systems und Eval optimieren. High-Risk-Forschung erst ganz zum Schluss.

## Prioritätsreihenfolge

**A Messung → B Baseline → C Quant → D Systems → E Eval → F Submission → G Forschungszweige**


## Board-Regel

Eine Änderung wird nur übernommen, wenn sie mindestens einen der echten Projektwerte verbessert: `roundtrip_bpb`, `artifact_bytes`, `steps_in_600s`, **Eval-Gain pro Sekunde** oder **Compliance-Sicherheit**.

---

# A — Messung und Compliance

Muss zuerst stehen. Ohne diese Lane optimierst du auf Phantom-Gewinne.

| ID | Task | Ziel / Output | Done-Kriterium |
|---|---|---|---|
| A1 | Roundtrip-Messung einbauen | Ziel: Echte Zielmetrik statt Proxy optimieren. Output: Logging für pre-quant BPB, roundtrip BPB, artifact bytes, eval seconds | Jeder Run schreibt diese Werte stabil. |
| A2 | Artefakt-Size-Gate | Ziel: 16-MB-Constraint früh erzwingen. Output: Automatischer Check `code + model < 16_000_000` | Run bricht bei Überschreitung klar ab. |
| A3 | Eval-Gate trennen | Ziel: Training, Sliding-Eval und TTT separat messbar machen. Output: Getrennte Timer und Reports | `train_seconds`, `eval_seconds`, `ttt_seconds` separat im Log. |
| A4 | Repro-Run-Template | Ziel: Vergleichbarkeit herstellen. Output: Einheitliches Run-Metadatenformat | Commit, Seed, Modell, Quant, Loader, Eval immer im Log. |

## Stop/Go

Diese Lane ist nicht optional. Erst wenn Logging, Size-Gate und Eval-Timer stabil sind, dürfen Baseline- oder Quant-Experimente als belastbar gelten.

---

# B — Repo-nahe Baseline

Ziel ist die schnellste Time-to-Signal mit minimalem Compliance-Risiko.

| ID | Task | Ziel / Output | Done-Kriterium |
|---|---|---|---|
| B1 | Baseline auswählen | Ziel: Schnellste Time-to-Signal. Output: Dokumentierter Baseline-Commit und Konfig | Baseline ist fest eingefroren. |
| B2 | 1-GPU-Proxy-Run | Ziel: Smoke-Test für Stabilität und Logging. Output: Kurzer Run mit vollständigem Report | Kein Shape-, Export- oder Eval-Fehler. |
| B3 | 3-Seed-Baseline | Ziel: Varianz verstehen. Output: Drei Run-Logs plus Mittelwert | Seed-Varianz ist bekannt. |
| B4 | Baseline-Roundtrip exportieren | Ziel: Referenz für Folgeexperimente. Output: Exportiertes Artefakt plus Roundtrip-BPB | Export ist reproduzierbar. |
| B5 | Baseline-Freeze | Ziel: Messanker schaffen. Output: Do-not-touch-Baseline-Konfig | Spätere Änderungen werden dagegen verglichen. |

## Stop/Go

Keine neue Architektur, kein Tokenizer-Wechsel, keine exotische Eval-Idee, bevor B1 bis B5 stabil stehen.

---

# C — Quantisierung

Erster harter Score-Hebel nach der Baseline. Erfolg zählt nur über den Roundtrip.

| ID | Task | Ziel / Output | Done-Kriterium |
|---|---|---|---|
| C1 | Quant-Gap-Benchmark | Ziel: Lücke zwischen pre-quant und roundtrip messen. Output: Report `delta = roundtrip - pre-quant` | Quant-Gap pro Run sichtbar. |
| C2 | GPTQ-lite Sweep | Ziel: Billige Rekonstruktionsverbesserung testen. Output: Kleine Sweep-Tabelle | Bestes Setting identifiziert. |
| C3 | Full GPTQ / verbesserte Rekonstruktion | Ziel: Stärkeren Weight-Roundtrip testen. Output: Vergleich gegen Lite | Roundtrip-BPB sinkt. |
| C4 | Clip- und Outlier-Sweep | Ziel: Ausreißerrobustheit verbessern. Output: Sweep-Report | Bestes Clipping-Fenster gewählt. |
| C5 | Late-QAT Ablation | Ziel: Quant-Gap weiter schließen. Output: Kurze QAT-Vergleiche | Klare Go/No-Go-Entscheidung. |
| C6 | Mixed int5/int6 Test | Ziel: Evtl. mehr Kapazität pro Byte. Output: Mixed-precision-Export | Nur behalten bei besserem Roundtrip. |

## Stop/Go

Ein Quant-Experiment ist nur erfolgreich, wenn **roundtrip BPB fällt**. Bessere pre-quant Loss ohne besseren Roundtrip gilt als Regression.

---

# D — Systems und Loader

Im 600-Sekunden-Regime ist mehr nutzbare Updates pro Sekunde oft wichtiger als eine elegante Modellidee.

| ID | Task | Ziel / Output | Done-Kriterium |
|---|---|---|---|
| D1 | Step-Time-Profiling | Ziel: Echten Bottleneck finden. Output: Zeitaufteilung pro Step | Größter Bottleneck benannt. |
| D2 | memmap Multi-Shard Loader | Ziel: I/O und Datenvielfalt verbessern. Output: Neuer Loader-Branch | Läuft stabil im Proxy. |
| D3 | Coprime-Stride Sampling | Ziel: Batch-Diversität erhöhen. Output: Sampling-Variante | Messbarer Vergleich gegen Baseline. |
| D4 | Async CPU Queue | Ziel: I/O überlappen. Output: Prefetch-Queue | GPU-Wartezeit sinkt. |
| D5 | CUDA Prefetch / H2D Overlap | Ziel: Step-Time senken. Output: Prefetch-Pfad | `steps_in_600s` steigt. |
| D6 | Loader A/B Test | Ziel: Nur nützliche Änderungen übernehmen. Output: Loader-Benchmark-Report | Besserer Loader eingefroren. |

## Stop/Go

Loader-Arbeit bleibt nur, wenn `steps_in_600s` oder `tokens/s` steigen oder der kurze Vergleichslauf BPB verbessert — ohne neue Instabilität.

---

# E — Evaluation

Eval ist eine eigene Optimierungsachse. Sie ist mächtig, aber compliance-sensitiv.

| ID | Task | Ziel / Output | Done-Kriterium |
|---|---|---|---|
| E1 | Sliding-Stride Sweep | Ziel: Bestes BPB/Zeit-Verhältnis finden. Output: Sweep über Strides | Bestes `delta BPB / delta time` identifiziert. |
| E2 | Legal-TTT Baseline | Ziel: Sauberen score-first Pfad bauen. Output: Referenzierbarer TTT-Run | Korrekt, reproduzierbar, unter Budget. |
| E3 | TTT Chunk-Sweep | Ziel: Adaption pro Chunk optimieren. Output: Chunk-Vergleich | Beste Chunkgröße fest. |
| E4 | TTT LR / Epochs Sweep | Ziel: Zeit und Gain austarieren. Output: Kleiner Sweep | Bestes Effizienzfenster fest. |
| E5 | Freeze-Blocks Ablation | Ziel: TTT billiger und stabiler machen. Output: Freeze-Vergleich | Gleicher oder besserer Gain bei weniger Zeit. |
| E6 | Sliding-only vs TTT | Ziel: Beste Eval-Strategie auswählen. Output: Head-to-head-Vergleich | Genau eine Default-Strategie gesetzt. |

## Stop/Go

TTT bleibt nur, wenn es unter **600 Sekunden Eval-Zeit** bleibt und gegen **Sliding-only** netto gewinnt. Erlaubt ist nur **backward-looking score-first TTT**.

---

# F — Submission-Härtung

Erst hier wird aus einer guten Experimentserie ein einreichbarer Record-Kandidat.

| ID | Task | Ziel / Output | Done-Kriterium |
|---|---|---|---|
| F1 | Submission-Ordner-Template | Ziel: Einreichung standardisieren. Output: Vollständiges Template | Alle Pflichtdateien vorhanden. |
| F2 | Log-Vollständigkeit prüfen | Ziel: Verifikation absichern. Output: Checkliste oder Script | Keine fehlenden Logs. |
| F3 | 3-Seed-Signifikanzpaket | Ziel: Record-Fähigkeit absichern. Output: Mittelwert, Std, Test | Verbesserung ist dokumentiert. |
| F4 | Dry-Run Submission | Ziel: Reproduzierbarkeit beweisen. Output: Kompletter Replay-Run | Start-to-finish erfolgreich. |
| F5 | Compliance Review | Ziel: Rote Zonen beseitigen. Output: Finale Risiko-Liste | Keine offene kritische Regelverletzung. |

## Stop/Go

Keine `submit-ready`-Aussage ohne Dry-Run, vollständige Logs, Artefaktcheck und expliziten Compliance-Review.

---

# G — Späte Forschungszweige

Nur angehen, wenn A bis F stabil sind. Hohes Upside, schlechteste Time-to-Signal.

| ID | Task | Ziel / Output | Done-Kriterium |
|---|---|---|---|
| G1 | Depth recurrence / parameter tying | Ziel: Mehr effektive Tiefe pro Byte. Output: Isolierter Branch | Billiger Proxy zeigt Signal. |
| G2 | Hybrid online mixer | Ziel: Kompressionspfad mit hohem Upside. Output: Formale Skizze plus Mini-Proxy | Korrekt normalisiert und kausal. |
| G3 | Tokenizer-first | Ziel: Evtl. anderer Byte/Token-Pareto. Output: Streng validierte Pipeline | Byte-Counting formal geprüft. |
| G4 | SSM / Mamba-artige Idee | Ziel: Long-context-Systemwette. Output: Separater Research-Branch | Step-Time- und BPB-Proof vorhanden. |
| G5 | TurboQuant-inspirierte Weight-Idee | Ziel: Scale-Overhead reduzieren. Output: Tiny-Ablation | Nur behalten bei besserem Roundtrip. |

## Stop/Go

Diese Lane ist bewusst kein Sprint-1-Thema. Vorher fehlt fast immer entweder die Messstabilität oder die Vergleichsbasis.

---

# Operativer Kanban-Zustand

## Backlog

- G1-G5

## Ready

- A1-A4
- B1-B5
- C1

## In Progress

Immer nur **ein großer Themenblock gleichzeitig**: erst A, dann B, dann C.

## Review

Jede Änderung mit:

- Hypothese
- erwartetem Effekt auf BPB / Artefakt / Runtime / Compliance
- billigstem Proxy
- Roundtrip-Ergebnis
- Baseline-Vergleich

## Nächste drei konkreten Schritte

- Roundtrip-Logging, Size-Gate und getrennte Eval-Timer im Code verankern.
- Einen repo-nahen Baseline-Stack einfrieren und mit 3 Seeds auswerten.
- Dann sofort Quant-Gap benchmarken; erst danach GPTQ- und Loader-Arbeit eskalieren.

## Quellenbasis

Das Board basiert auf der Quellensichtung der Projektunterlagen **„Strategieraum und Gewinnpfade für Parameter Golf“** und **„Parameter Golf Project Skills“**.
