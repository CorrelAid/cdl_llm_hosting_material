# Hosting-Infrastruktur

LLMs lassen sich fast auf allen Arten von Infrastuktur betreiben, sogar auf Raspberry Pis. Die mögliche Geschwindigkeit, Qualität und Länge des In- und Outputs kann sich dabei jedoch stark unterscheiden. Andere Vergleichsaspekte sind Kosten sowie damit zusammenhängend Strom- und Wasserverbrauch. 



- CPU vs GPU
- Vor allem Nvidia relevant, aber AMD zieht nach
- GPU VRAM Größen (H200 max) und Generationen (ältere Generationen können Modelle nicht in aktuellen Datentypen/Quantisierungsstufen betreiben)

- Wasser vs Luftkühlug
    - Hetzer nur Luftkühlung
    - Wasser verdampft bei Wasserkühlung (unterschiedliche Effiziengrade)
    - Intransparente Situation
    - Link Mistral LCA
- Strompreis bei lokalem Vertrieb langfristier Haupt-Kostenpunkt

- Cloud
    - dedicated GPU-Server (Hetzner, Scaleway) vs serverless (Modal, Beam Cloud)