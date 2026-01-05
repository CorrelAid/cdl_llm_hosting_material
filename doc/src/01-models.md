# Open LLMs

## Was ist ein LLM?

LLMs (Large Language Models) sind große neuronale Netzwerke. Das ist eine Struktur aus vielen miteinander verbundenen Schichten aus Modellen biologischer Neuronen (basierend auf dem, was 1943 über Neuronen bekannt war) {{#cite mcculloch_logical_1943}}. Diese Neuronenmodelle führen einzelne Rechnungen aus, bei denen konfigurierbare Parameter verwendet werden. Beim Training von LLMs werden Millionen oder sogar Milliarden dieser Parameter nach und nach angepasst, bis der Output des Modells den Trainingsdaten entspricht. 

Die bekanntesten LLMs sind **generativ**, haben also als Input und als Output (mindestens) Text. Andere LLMs haben als Output Zahlen in Form von Vektoren, die den Text-Input semantisch repräsentieren sollen, sogenannte **Embeddings**. **Multimodale LLMs** können z.B. auch Bilder verarbeiten und deren Inhalt sprachlich repräsentieren. 

Generell wird der Output hergestellt durch zahlreiche Matrizenrechnungen, die sich besonders effizient auf spezialisierter Hardware wie GPUs (Graphics Processing Unit/Grafikkarten) ausführen lassen, da sie dort in großer Zahl gleichzeitig passieren können. 

Modelle können als Dateien verfügbar gemacht werden, die sowohl die Architektur als auch die trainierten Parameter eines Modells enthalten und somit selbst betrieben werden können. Für das Self-Hosting von LLMs stehen zahlreiche solcher Modelle zur Verfügung. Dieses Kapitel gibt einen Überblick über die wichtigsten Aspekte bei der Modellauswahl und -bewertung.

## Open Weight vs. Open Source

Im Kontext von Large Language Models wird häufig von Open-Source-Modellen gesprochen, obwohl die meisten dieser Modelle technisch gesehen Open-Weight-Modelle (hier auch Open Modelle genannt) sind. Der Unterschied mag unwichtig klingen, hat jedoch Bedeutung für die Einordnung dessen, was die Urheber der LLMs wirklich bereitstellen:

- **Open-Weight-Modelle** stellen die trainierten Modellparameter (Weights) öffentlich zur Verfügung. Nutzer:innen können diese Modelle herunterladen, ausführen und für eigene Zwecke verwenden. Allerdings sind oft weder die Trainingsdaten noch der vollständige Trainingscode verfügbar.

- **Open-Source-Modelle** im engeren Sinne gehen weiter: Sie veröffentlichen nicht nur die Modellparameter, sondern auch die Trainingsdaten, den Trainingscode und die gesamte Entwicklungspipeline. Dies ermöglicht vollständige Reproduzierbarkeit und Transparenz.

Für Self-Hosting sind beide Varianten geeignet, wobei Open-Weight-Modelle deutlich verbreiteter sind. Die Wahl zwischen beiden hängt davon ab, wie wichtig vollständige Transparenz und Reproduzierbarkeit für den jeweiligen Anwendungsfall sind.

## Bekannte Open (Source) Modelle

Die Zahl der Open Modelle nahm in den letzten Jahren rasant zu. Die meisten der größten Tech-Unternehmen haben mittlerweile Open-Modelle entwickelt, darunter Google ([Gemma](https://deepmind.google/models/gemma/)), Microsoft ([Phi](https://azure.microsoft.com/en-us/products/phi)), Meta ([Llama](https://www.llama.com/)), oder IBM ([Granite](https://www.ibm.com/granite)). Andere bekannte Anbieter und Modelle sind:

- **Mistral AI**: [Mistral Small 3.1](https://mistral.ai/news/mistral-small-3-1)

- **OpenAI**: [gpt-oss](https://openai.com/index/introducing-gpt-oss/)

- **Allen Institute for Artificial Intelligence**: [Olmo](https://allenai.org/olmo)

## Hugging Face: Wo findet man Open Modelle und in welchen Varianten gibt es sie?

[Hugging Face](https://huggingface.co/) ist die zentrale Plattform für das Teilen von Open-Weight- und Open-Source-LLMs. Sie fungiert als Repository, Community-Hub und Entwicklungsplattform.

Tausende von Modellen sind verfügbar, von kleinen spezialisierten Modellen bis zu großen Frontier-Modellen. Jedes Modell verfügt über eine Seite mit Beschreibung, Lizenzinformationen, Nutzungsbeispielen und Download-Links. Filter ermöglichen die Suche nach Modellgröße, Aufgabe, Lizenz und weiteren Kriterien.

Modelle können direkt über die Webseite heruntergeladen oder programmgesteuert über die Hugging Face API bezogen werden. Die meisten [Inference-Server](./02-software.html) können Modelle automatisch von Hugging Face laden, indem lediglich der Modellname angegeben wird (z.B. `mistralai/Mistral-7B-Instruct-v0.2`).

### Varianten von Modellen

Die Varianten und damit zusammenhängende Benennung von Modellen kann etwas verwirrend sein. 

- `mistralai/Mistral-Small-24B-Instruct-2501`: Organisation (mistralai) / Modellfamilie (Mistral-Small) / Parameterzahl (24 Milliarden) / Fähigkeit (Instruct für Anweisungen) / Release-Datum (Januar 2025)

- `Qwen/Qwen3-VL-2B-Thinking-FP8`: Organisation (Qwen/Alibaba) / Familie (Qwen) / Generation (3) / Modalität (VL für Vision-Language) / Parameterzahl (2 Milliarden) / Fähigkeit (Thinking für Reasoning) / Quantisierung (FP8 für 8-bit Floating Point)

Oft werden neben Instruct oder Thinking Varianten auch Base-Varianten veröffentlicht, die noch nicht an das Ausführen von Anweisungen angepasst worden sind, sondern nur Sprache vervollständigen können.

### Quantisierung

Das **FP8** in `Qwen3-VL-2B-Thinking-FP8` steht für das Datenformat der Parameter des Modells. Dieses Modell wurde während oder nach dem Training quantisiert. Quantisierung ist eine Technik zur Reduzierung der Modellgröße und des Speicherbedarfs, die für Self-Hosting von zentraler Bedeutung ist. Bei der Quantisierung werden die Modellparameter von höherer Präzision (z.B. 16-Bit oder 32-Bit Floating Point) in niedrigere Präzision (z.B. 8-Bit, 4-Bit oder sogar 2-Bit) konvertiert. Dies reduziert den benötigten GPU-Speicher (VRAM) erheblich.

Quantisierung führt zu leichten Qualitätseinbußen bei den Modellantworten. Moderne Quantisierungsmethoden minimieren diese Verluste jedoch stark. In vielen Anwendungsfällen ist der Qualitätsunterschied kaum wahrnehmbar, während die Geschwindigkeit deutlich zunimmt.

Für Self-Hosting sind quantisierte Modelle oft die einzige praktikable Option. Die meisten Inference-Server unterstützen verschiedene Quantisierungsformate, und auf Hugging Face sind bereits quantisierte Versionen beliebter Modelle verfügbar.


## Auswahl von Modellen - Evaluation von LLMs

LLMs zu evaluieren ist aufgrund der Größe ihres Einsatzgebiets und der Art ihrer Fähigkeiten keine einfache Aufgabe. LLMs sind als Teil des Gebiets des Machine Learning eine KI-Technologie. Künstliche Intelligenz heißt so, weil diese Technologie in Relation zur menschlichen Intelligenz gestellt wird: Es besteht Ähnlichkeit bei Zielen, Herstellung oder Funktionsweise (bei Komponenten) von Intelligenz {{#cite unesco_ethik_2023}}. Misst man bei der Evaluation von LLMs deren Intelligenz? Das ist mindestens umstritten.

Eine pragmatische Herangehensweise setzt bei der konkreten Aufgabe an, die ein LLM erfüllen soll. Will man es z.B. zur Klassifikation deutscher Texte verwenden, kann man herkömmliche Metriken wie den [F-Score](https://de.wikipedia.org/wiki/Beurteilung_eines_bin%C3%A4ren_Klassifikators#Kombinierte_Ma%C3%9Fe) nutzen. Hat man bereits klassifizierte Texte, kann man Modelle selbst testen. 

Man kann sich jedoch auch an vorhandenen aufgabenbezogenen Benchmarks orientieren. Ein Beispiel dafür ist [`languagebench`](https://huggingface.co/spaces/fair-forward/languagebench), das neben Tasks wie Klassifikation auch zwischen der Performance auf unterschiedlichen Sprachen unterscheidet. 

Andere vorhandene Benchmarks testen LLMs auf einem breiteren, mehr mit Intelligenz verbundenen Level. [Humanities Last Exam](https://agi.safe.ai/) enthält Problemstellungen in akademischen Disziplinen. Auch wenn es sich um einen provokanten Titel handelt, schreiben die Urheber:innen des Tests, dass es sich um keinen generellen Test auf Intelligenz handele, sondern um eine "gezielte Messung von technischem Wissen und Denkvermögen". Wie man in der [Rangliste](https://dashboard.safe.ai/) sieht, sind große, proprietäre Modelle Open Modellen voraus. 

Benchmarks sind nützliche Orientierungshilfen, aber nehmen einem nicht vollständig die Entscheidung ab. Sie können anfällig für Overfitting sein (Modelle werden auf Benchmark-Performance optimiert), oder Daten aus Benchmarks sind in den Trainingsdaten enthalten (was die Ergebnisse verfälscht), bilden nicht alle vorstellbaren Aufgaben ab und sagen wenig über das Verhalten in spezifischen Anwendungsfällen aus. Für Self-Hosting-Projekte empfiehlt sich, Modelle auch anhand eigener, anwendungsspezifischer Tests zu evaluieren.

## Lizenzen

Bevor man ein selbst gehostetes Modell anderen zur Verfügung stellt oder in manchen Fällen sogar selbst nutzt, sollte man sich mit den Lizenzen von LLMs beschäftigen. Diese unterscheiden sich erheblich und geben vor wie die Modelle genutzt werden dürfen:

- **Permissive Lizenzen** (z.B. [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0.html), [MIT](https://opensource.org/license/mit)): Erlauben kommerzielle Nutzung, Modifikation und Weiterverbreitung mit minimalen Einschränkungen. Beispiele: Viele Modelle von [Mistral](https://huggingface.co/collections/mistralai/mistral-large-3), [Qwen-Modelle](https://huggingface.co/collections/Qwen/qwen3) oder Modelle aus der [GPT-OSS-Familie](https://huggingface.co/collections/openai/gpt-oss).

- **Restriktive Lizenzen**: Erlauben Nutzung und Modifikation, schränken aber kommerzielle Nutzung ein oder verlangen Lizenzgebühren ab einer bestimmten Nutzerzahl. Beispiel: [Llama-2-Modelle](https://ai.meta.com/llama/license/) und [Llama-3-Modelle](https://www.llama.com/llama3/license/). Sehr restriktiv sind die Lizenzen bei multimodalen Llama-3-Modellen {{#cite weatherbed_meta_2024}} oder bei allen Llama-4-Modellen {{#cite schwarze_meta_2025}}, da diese nicht in der EU verwendet werden dürfen.

Die Lizenzen sind in der Regel auf den Modell-Seiten bei Hugging Face oder auf den Websites der Entwickler dokumentiert.
