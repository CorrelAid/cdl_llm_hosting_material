## Alternativen zum vollständigen Self-Hosting

Self-Hosting ist nicht immer die optimale Lösung. Je nach Anforderungen können andere Ansätze sinnvoller sein.

Vor der Entscheidung für Self-Hosting oder Cloud-APIs sollte geprüft werden, ob z.B. ein RAG-System die richtige Technologie ist. Oft reicht eine klassische Volltextsuche mit verbessertem Wissensmanagement. 

### Managed LLM-Services

#### DSGVO-konforme Lösungen
- **OpenAI auf Azure / Claude auf AWS**: State-of-the-Art-Modelle mit EU-Rechenzentren und DSGVO-konformen Verträgen

#### LLM-Anbieter in der EU
- **[Scaleway Generative APIs](https://www.scaleway.com/en/generative-apis/)**: Open-Weight-Modelle in französischen Rechenzentren
- **[IONOS Generative APIs](https://www.ionos.de/)**: LLM-Services mit Hosting in Deutschland

#### Kosteneffiziente API-Aggregatoren
- **[OpenRouter](https://openrouter.ai/)**: Große Modellauswahl, sehr wettbewerbsfähige Preise (US-Datenschutz)

## Kostenvergleich: Self-Hosting vs. API-Services (Parrotpark)

### Self-Hosting: Scaleway GPU-Instanz (L4)
- Automatisierte Bereitstellung nur während Arbeitszeiten (10h/Tag, 5 Tage/Woche)
- Berechnung: $0.75\,\text{€/h} \times (10\,\text{h} \times 5\,\text{Tage} \times 4\,\text{Wochen}) = 150\,\text{€}$
- Mit MwSt. (19%): **178.50€/Monat**

### Cloud-API: OpenRouter (gleiches Modell)
- Für 178.50€ erhält man bei 50/50 Input/Output-Split: **3,154M Tokens**

### Tatsächlicher Verbrauch (2 Wochen Parrotpark-Evaluation)
- Input: 329,503 Tokens | Output: 103,083 Tokens | Gesamt: 432,586 Tokens
- **API-Kosten: $0.027** (€0.02)
- Hochgerechnet auf Monat: ca. €0.05