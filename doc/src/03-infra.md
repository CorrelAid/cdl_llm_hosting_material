## Worauf wird selbst gehostet?

LLMs lassen sich prinzipiell auf fast allen Arten von Infrastruktur betreiben – sogar auf einem [Raspberry Pi](https://www.maketecheasier.com/run-llm-on-raspberry-pi/). Die praktisch erreichbare Geschwindigkeit, Modellqualität und Länge des In- und Outputs unterscheiden sich jedoch erheblich je nach Hardware. Dieses Kapitel beschreibt die Hardware-Anforderungen für LLM-Hosting, den damit verbundenen Ressourcenverbrauch und die verschiedenen Infrastruktur-Optionen von lokal bis Cloud.

Self-Hosting kann an verschiedenen Orten erfolgen:

- **Lokal (On-Premises)**: Betrieb auf dem eigenen Laptop oder auf dedizierten Servern, die in den Räumlichkeiten einer Organisation oder Privatperson stehen. Dies bedeutet volle Kontrolle über das Setup, aber auch volle Verantwortung für Hardware und Wartung.
- **Cloud**: Server werden in verschiedenen Formen von Cloud-Providern zur Miete angeboten – entweder als virtualisierte Maschine (VM) oder als dedizierte physische Maschine (Bare Metal). Flexibel skalierbar, aber mit laufenden Kosten.
- **Hybrid (Co-Hosting)**: Kauf eigener Server, die jedoch in den Räumen eines Cloud-Providers betrieben und gewartet werden. Kombination aus Eigentum und professioneller Infrastruktur.

## Hardware-Anforderungen

### CPU vs GPU

Für das Hosting von LLMs stellt sich zunächst die grundlegende Frage: CPU oder GPU?

**CPU-Inferenz** ist möglich und für sehr kleine Modelle oder Testszenarien praktikabel. Moderne CPUs können kleine quantisierte Modelle durchaus betreiben. Die Inferenz ist deutlich langsamer als auf GPUs, was für produktive Systeme mit Nutzerinteraktion meist inakzeptabel ist.

**GPU-Inferenz** ist für Self-Hosting von LLMs praktisch der Standard. GPUs sind für die massiv parallelen Matrizenoperationen optimiert, die bei der Textgenerierung anfallen, wewegen die Geschwindigkeitsunterschiede immens sind.

**Marktlage**: Der GPU-Markt für LLM-Inferenz wird dominiert von **NVIDIA**, deren CUDA-Ökosystem den De-facto-Standard darstellt. Die meisten Inference-Server und ML-Frameworks sind primär für NVIDIA-GPUs optimiert. **AMD** holt jedoch auf: ROCm (AMDs Alternative zu CUDA) wird zunehmend besser unterstützt, und AMD-GPUs bieten oft ein besseres Preis-Leistungs-Verhältnis. Für kritische Produktivsysteme ist NVIDIA derzeit die sicherere Wahl, für experimentelle Setups kann AMD eine kostengünstige Alternative sein.

### GPU-Spezifikationen

Bei der Auswahl von GPUs für LLM-Hosting sind mehrere Faktoren entscheidend:

**VRAM-Größe**: Der GPU-Speicher (VRAM) ist oft der limitierende Faktor. Ein Modell muss vollständig in den VRAM passen, um effizient zu laufen

Gängige GPU-VRAM-Größen:
- Consumer-GPUs (z.B. RTX 4090): 24 GB
- Professional GPUs (z.B. A100): 40 GB oder 80 GB
- High-End Datacenter GPUs (z.B. H100, H200): 80 GB bzw. 141 GB (H200)

Die H200 mit 141 GB VRAM stellt aktuell das obere Ende des Spektrums dar und ermöglicht den Betrieb sehr großer Modelle auf einer einzelnen GPU.

**GPU-Generationen**: Neuere GPU-Generationen unterstützen moderne Datentypen und Quantisierungstechniken besser als ältere Generationen. Die Unterstützung für mache Datentypen in neueren Generationen eingeführt. Bei der Hardwareauswahl sollte daher nicht nur die VRAM-Größe, sondern auch die GPU-Generation berücksichtigt werden, da dies direkten Einfluss auf verfügbare Quantisierungsoptionen und Performance hat.

## Ressourcenverbrauch

Der Betrieb von LLM-Infrastruktur ist ressourcenintensiv. Die beiden Hauptfaktoren sind Kühlung und Stromverbrauch, die eng miteinander verbunden sind.

### Kühlung

Leistungsfähige GPUs erzeugen erhebliche Abwärme, die abgeführt werden muss, um Überhitzung und Hardware-Schäden zu verhindern. Die Wahl des Kühlungssystems hat direkten Einfluss auf Energieeffizienz und Umweltauswirkungen.

**Luftkühlung** ist der Standard bei den meisten Cloud-Providern und für lokales Hosting. Sie ist technisch einfach und zuverlässig: Ventilatoren transportieren kühle Luft an die Hardware und führen erwärmte Luft ab. Der Nachteil: Luftkühlung ist energieintensiv, da große Luftmengen bewegt werden müssen und die Kühleffizienz begrenzt ist. In Rechenzentren muss zusätzlich die Raumtemperatur reguliert werden, was den Energieaufwand weiter erhöht. Anbieter wie Hetzner setzen ausschließlich auf Luftkühlung.

**Wasserkühlung** (bzw. Flüssigkeitskühlung) ist deutlich effizienter: Wasser kann Wärme  besser transportieren als Luft bei gleichem Volumen. Moderne Flüssigkeitskühlsysteme können Abwärme direkt am Chip abführen und reduzieren den Energieaufwand für Kühlung erheblich . Allerdings ist Wasserkühlung technisch aufwändiger und teurer in der Installation. Zudem verdunstet ein Teil des Kühlwassers, was zu relevantem Wasserverbrauch führt

**Transparenz und Nachhaltigkeit**: Die Umweltauswirkungen der Kühlsysteme sind oft schwer nachvollziehbar. Life Cycle Assessments (LCAs), wie sie beispielsweise Mistral AI veröffentlicht hat, bieten einen systematischen Ansatz zur Bewertung von Energieverbrauch und Umweltauswirkungen über den gesamten Lebenszyklus – von der Hardware-Produktion über den Betrieb bis zur Entsorgung.

### Strom

CPU, GPU und andere Teile eines Servers, wie das Kühlungssystem, verbrauchen Strom. Besonders hoch ist der Verbrauch bei GPUs. Datacenter-GPUs haben einen erheblichen Energiebedarf, der je nach Modell und Last zwischen einigen hundert Watt bis über 700 Watt liegen kann. Der Gesamtstromverbrauch eines Systems liegt deutlich höher, da CPU, RAM, Netzwerk und Kühlung hinzukommen.

Bei lokalem Hosting ist der Strompreis langfristig oft der Haupt-Kostenpunkt. Bei Cloud-Hosting sind diese Stromkosten in den Mietpreisen enthalten.

## Infrastruktur-Optionen

Nach der Klärung der Hardware-Anforderungen und des Ressourcenverbrauchs stellt sich die Frage: Wo wird die Infrastruktur betrieben? Es gibt grundsätzlich drei Optionen: lokal, Cloud oder hybrid.

### Lokal (On-Premises)

Bei lokalem Hosting wird die Hardware selbst betrieben – auf dem eigenen PC/Laptop, einem Home-Server oder in einem eigenen Rechenzentrum. Der eigenet PC oder Laptop hat jedoch keine mit in der Cloud genutzen vergleichbaren GPU


### Cloud

Cloud-Hosting bietet flexible Mietoptionen für Server bei externen Anbietern. Es lassen sich drei Modelle unterscheiden:

**Dedicated Server**: Miete dedizierter Server mit GPUs (monatlich oder stündlich). Der Server läuft kontinuierlich. Anbieter wie Hetzner oder Scaleway bieten solche Optionen in der EU an.

**Serverless / On-Demand**: Server werden nur bei Bedarf gestartet und Abrechnung erfolgt pro Minute. Ideal für sporadische Nutzung. Anbieter wie Modal oder Beam Cloud spezialisieren sich auf ML-Workloads.

**Co-Location**: Eigene Hardware wird in einem Rechenzentrum eines Providers betrieben, der Strom, Kühlung und Netzwerk bereitstellt. Kombination aus Eigentum und professioneller Infrastruktur.
