## Alternativen

1. Sind LLMs uberhaupt die richtige Wahl für dein Problem? 

Beispiel Chatbot mit RAG: reicht vielleicht eine freitextsuche/bessrer wissensmanagement?

- OpenAI on Azure
- Openrouter
- Scalway generative APIs
- Ionos generative APIs



## Example Comparison (Parrotpark)


- Scaleway GPU-Instanz (L4)
  - Automatisierte Bereitstellung nur während Arbeitszeiten (10h/Tag)
  - Berechnung: $0.75\,\text{€/h} \times (10\,\text{h} \times 5\,\text{Tage} \times 4\,\text{Wochen}) = 150\,\text{€}$
  - Mit MwSt. (19%): **178.50€**

- Wie viel Tokens bekäme man für diesen Preis bei OpenRouter für das gleiche Modell?
  - Bei 50/50 Input/Output-Split: **3,154M Tokens** für €178.50

- Tatsächlicher Verbrauch während 2 Wochen Parrotpark Evaluation
  - Input: 329,503 Tokens | Output: 103,083 Tokens
  - **API-Kosten wären: $0.027** (€0.02)