# Open LLMs

## Was ist ein LLM?

LLMs (Large Language Models) sind große neuronalen Netzwerke. Das ist eine Struktur aus vielen miteinander verbundene Schichten aus Modellen biologischer Neuronen (basierend auf dem, was 1943 über Neuronen bekannt war) {{#cite mcculloch_logical_1943}}. Die Neuronen führen einzelne Rechnungen aus, bei denen konfigurierbare Parameter verwendet werden. Beim Training dieser Modelle werden Millionen oder sogar Milliarden dieser Parametern nach und nach angepasst, bis der Output des Modells den Trainingsdaten entspricht. 

Die bekanntesten LLMs sind **generativ**, haben also als Input und als Output Text. Andere LLMs haben als Output Zahlen in Form von Vektoren, der den Text-Input semantisch repräsentieren soll, sogenannte **Embeddings**.

Generell wird der Output hergestellt durch zahlreiche Matrizenrechnungen, die sich besonders effizient auf spezialisierter Hardware wie GPUs (Graphics Processing Unit/Grafikkarten) ausführen lassen, da sie dort in großer Zahl gleichzeitig passieren können. 

**Open-Weight-Modelle** sind als Dateien verfügbar, die sowohl die Architektur als auch die trainierten Parameter eines Modells enthalten und selbst gehostet werden können. Für das Self-Hosting von LLMs stehen zahlreiche solcher Modelle zur Verfügung. Dieses Kapitel gibt einen Überblick über die wichtigsten Aspekte bei der Auswahl und Bewertung von Modellen: von Lizenzfragen über technische Optimierungen bis hin zu Bezugsquellen und Bewertungsmetriken.

## Open Weight vs. Open Source

Im Kontext von Large Language Models wird häufig von Open-Source-Modellen gesprochen, obwohl die meisten dieser Modelle technisch gesehen Open-Weight-Modelle (hier auch Open Modelle genannt) sind. Der Unterschied mag unwichtig klingen, hat jedoch Bedeutung für die Einordnung dessen, was die Urheber der LLMs wirklich bereitstellen:

- **Open-Weight-Modelle** stellen die trainierten Modellparameter (Weights) öffentlich zur Verfügung. Nutzer:innen können diese Modelle herunterladen, ausführen und für eigene Zwecke verwenden. Allerdings sind oft weder die Trainingsdaten noch der vollständige Trainingscode verfügbar. Beispiele: Llama (Meta), Mistral (Mistral AI), Qwen (Alibaba).

- **Open-Source-Modelle** im engeren Sinne gehen weiter: Sie veröffentlichen nicht nur die Modellparameter, sondern auch die Trainingsdaten, den Trainingscode und die gesamte Entwicklungspipeline. Dies ermöglicht vollständige Reproduzierbarkeit und Transparenz. Beispiele: OLMo (Allen Institute for AI), Pythia (EleutherAI).

Für Self-Hosting sind beide Varianten geeignet, wobei Open-Weight-Modelle deutlich verbreiteter sind. Die Wahl zwischen beiden hängt davon ab, wie wichtig vollständige Transparenz und Reproduzierbarkeit für den jeweiligen Anwendungsfall sind.

## Lizenzen

Die Lizenzen von LLMs unterscheiden sich erheblich und haben direkten Einfluss darauf, wie die Modelle genutzt werden dürfen:

**Permissive Lizenzen** (z.B. Apache 2.0, MIT): Erlauben kommerzielle Nutzung, Modifikation und Weiterverbreitung mit minimalen Einschränkungen. Beispiele: Mistral-Modelle, Qwen-Modelle.

**Restriktive Community-Lizenzen**: Erlauben Nutzung und Modifikation, schränken aber kommerzielle Nutzung ein oder verlangen Lizenzgebühren ab einer bestimmten Nutzerzahl. Beispiel: Llama-Modelle haben eine Custom-Lizenz[^1], die bei über 700 Millionen monatlichen Nutzern eine separate Vereinbarung erfordert.

**Research-Only-Lizenzen**: Beschränken die Nutzung auf nicht-kommerzielle Forschungszwecke. Solche Modelle eignen sich nicht für produktive Self-Hosting-Szenarien.


Vor dem Einsatz eines Modells sollte die Lizenz sorgfältig geprüft werden, insbesondere bei kommerzieller Nutzung oder bei Organisationen mit vielen Nutzer:innen. Die Lizenzen sind in der Regel auf den Modell-Seiten bei Hugging Face oder auf den Websites der Entwickler dokumentiert.

## Quantisierung

Quantisierung ist eine Technik zur Reduzierung der Modellgröße und des Speicherbedarfs, die für Self-Hosting von zentraler Bedeutung ist.

**Funktionsweise**: Bei der Quantisierung werden die Modellparameter von höherer Präzision (z.B. 16-Bit oder 32-Bit Floating Point) in niedrigere Präzision (z.B. 8-Bit, 4-Bit oder sogar 2-Bit) konvertiert. Dies reduziert den benötigten GPU-Speicher (VRAM) erheblich.

**Auswirkungen**: Ein Modell mit 70 Milliarden Parametern (70B) benötigt in voller Präzision (FP16) etwa 140 GB VRAM. Mit 4-Bit-Quantisierung reduziert sich der Bedarf auf ca. 35 GB – ein Unterschied, der darüber entscheidet, ob ein Modell auf verfügbarer Hardware überhaupt lauffähig ist.

**Trade-offs**: Quantisierung führt zu leichten Qualitätseinbußen bei den Modellantworten. Moderne Quantisierungsmethoden wie GPTQ, AWQ oder GGUF minimieren diese Verluste jedoch stark. In vielen Anwendungsfällen ist der Qualitätsunterschied zwischen 4-Bit und voller Präzision kaum wahrnehmbar, während die Geschwindigkeit deutlich zunimmt.

**Praktische Bedeutung**: Für Self-Hosting sind quantisierte Modelle oft die einzige praktikable Option. Die meisten Inference-Server unterstützen verschiedene Quantisierungsformate, und auf Hugging Face sind bereits quantisierte Versionen beliebter Modelle verfügbar.

## Hugging Face

[Hugging Face](https://huggingface.co/) ist die zentrale Plattform für Open-Weight- und Open-Source-LLMs. Sie fungiert als Repository, Community-Hub und Entwicklungsplattform.

**Model Hub**: Tausende von Modellen sind verfügbar, von kleinen spezialisierten Modellen bis zu großen Frontier-Modellen. Jedes Modell verfügt über eine Seite mit Beschreibung, Lizenzinformationen, Nutzungsbeispielen und Download-Links. Filter ermöglichen die Suche nach Modellgröße, Aufgabe, Lizenz und weiteren Kriterien.

**Praktische Nutzung**: Modelle können direkt über die Webseite heruntergeladen oder programmgesteuert über die Hugging Face API bezogen werden. Die meisten Inference-Server können Modelle automatisch von Hugging Face laden, indem lediglich der Modellname angegeben wird (z.B. `mistralai/Mistral-7B-Instruct-v0.2`).

**Zusätzliche Ressourcen**: Neben Modellen hostet Hugging Face auch Datasets und Spaces (interaktive Demos). Die Plattform bietet umfangreiche Dokumentation und eine aktive Community, die bei Fragen hilft.

**Alternativen**: Während Hugging Face die dominante Plattform ist, gibt es Alternativen wie ModelScope (chinesischer Markt) oder direkte Downloads von Entwickler-Websites. Für die meisten Self-Hosting-Projekte ist Hugging Face jedoch die erste Anlaufstelle.

## Metriken

Die Bewertung von LLMs erfolgt anhand verschiedener Metriken und Benchmarks, die unterschiedliche Fähigkeiten testen:

**LLM-Leaderboards**: Plattformen wie das [Open LLM Leaderboard](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard) von Hugging Face aggregieren Benchmark-Ergebnisse und ermöglichen den direkten Vergleich von Modellen. Diese Leaderboards werden kontinuierlich aktualisiert und sind hilfreich für eine erste Orientierung.

**Aufgabenbezogene Benchmarks**: Verschiedene Benchmarks testen spezifische Fähigkeiten:
- **MMLU (Massive Multitask Language Understanding)**: Testet Wissen aus 57 Themenbereichen (STEM, Geisteswissenschaften, Sozialwissenschaften)
- **GSM8K**: Mathematische Problemlösung (Grundschulmathematik)
- **HumanEval**: Programmierfähigkeiten (Code-Generierung)
- **HellaSwag**: Commonsense Reasoning
- **TruthfulQA**: Faktentreue und Resistenz gegen Fehlinformationen

**Humanities-fokussierte Tests**: Manche Benchmarks verwenden standardisierte Prüfungen aus verschiedenen Bildungssystemen, um die Modellleistung in geisteswissenschaftlichen Bereichen zu messen. Dies kann Examensaufgaben aus Literatur, Geschichte, Philosophie oder Ethik umfassen.

**Wichtige Einschränkungen**: Benchmarks sind nützliche Orientierungshilfen, aber keine vollständige Bewertung. Sie können anfällig für Overfitting sein (Modelle werden auf Benchmark-Performance optimiert), bilden nicht alle relevanten Fähigkeiten ab (z.B. Kreativität, Dialogfähigkeit) und sagen wenig über das Verhalten in spezifischen Anwendungsfällen aus. Für Self-Hosting-Projekte empfiehlt sich, Modelle auch anhand eigener, anwendungsspezifischer Tests zu evaluieren.
