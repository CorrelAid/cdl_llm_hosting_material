# Einleitung, Überblick und Definitionen

Dieses Material entstand auf der Grundlage eines Vortrags[^1] über Self-Hosting von LLMs, der auf dem [Datenfestival](https://www.youtube.com/watch?v=exOeJCLcuHQ) des [Civic Data Lab](https://civic-data.de/) gehalten wurde. Es soll den Vortrag zugänglicher machen, umfassend über das Thema informieren und einen praxisorientierten Überblick geben.

Unter Self-Hosting wird der (auf unterschiedlichen Leveln) eigenständige Betrieb von Infrastruktur verstanden, auf der in diesem Fall LLMs und mit diesen zusammenhängenden Software läuft.


## Warum Self-Hosting?

Die Nutzung proprietärer LLM-Dienste ist mit **Abhängigkeiten**, **Intransparenz** und mangelnder **Kontrolle** verbunden. Bei kommerziellen Anbietern bleibt unklar, wie Daten tatsächlich verarbeitet werden, wie die Modelle trainiert wurden und welche Eigenschaften sie besitzen. Auch die Funktionsweise eingebundener Tools wie Websuche sowie der tatsächliche Ressourcenverbrauch sind nicht nachvollziehbar. Self-Hosting adressiert diese Problematik und trägt zur **digitalen Souveränität** bei: Organisationen und Privatpersonen erlangen, je nach Art des Selfhostings, mehr Transparenz und Kontrolle über den LLM-Betrieb – von der Datenverarbeitung über die Modellauswahl bis hin zum Energie- und Ressourcenverbrauch.


## Aufbau der Dokumentation

Im Verlaufe dieses Materials wird aufeinander aufbauend in eine Reihe von Aspekten des Self-Hostings von LLMs eingeführt:

- Im [1. Kapitel](./01-models.md) wird sich zunächst mit LLMs an sich beschäftigt. Was ist das überhaupt? Das Kapitel enthält Erläuterungen zu LLMs an sich. Was verstehen wir darunter? Es erklärt außerdem den Unterschied zwischen Open-Weight- und Open-Source-Modellen, gibt einen Überblick über Lizenzmodelle und deren Implikationen, erläutert Quantisierung als zentrale Optimierungstechnik, stellt Hugging Face als primäre Bezugsquelle vor und beschreibt gängige Metriken und Benchmarks zur Modellbewertung.

- Im [2. Kapitel](./02-software.md) werden die Fragen beantwortet: Was wird selbst gehostet, wie werden LLMs betrieben und welche Komponenten werden außer dem LLM benötigt? Hier werden  konkrete Software-Komponenten für die verschiedenen Teile eines LLM-Systems vorgestellt. Von Inference-Servern über Chat-Interfaces bis zu API-Gateways und Vektor-Datenbanken. Als praktisches Beispiel wird Parrotpark vorgestellt, ein vollständiges self-gehostetes LLM-System, das die Integration verschiedener Open-Source-Komponenten demonstriert.

- Das [3. Kapitel](./02-infra.md) beantwortet die Frage: Worauf wird selbst gehostet? Das Kapitel enthält einen Vergleich zwischen den verschiedenen Infrastrukturoptionen. Außerdem behandelt das Kapitel  Hardware-Anforderungen (CPU vs. GPU, VRAM-Größen, GPU-Generationen) sowie Aspekte wie Ressourcenverbrauch und langfristige Kostenbetrachtungen.


- In [Kapitel 4](./03-alternatives.md) werden Alternativen zum vollständigen Self-Hosting, einschließlich hybrider Ansätze und kommerzieller Lösungen mit verschiedenen Compliance-Leveln vorgestellt.

[^1]: Die Slides des Vortrags lassen sich [hier](https://correlaid.github.io/datenfestival_ap3_session/) finden, begleitender Code [hier](https://github.com/CorrelAid/datenfestival_ap3_session).
