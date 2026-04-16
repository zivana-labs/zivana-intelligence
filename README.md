# zivana-intelligence

Market Intelligence Primitive for the Zivana Protocol — AI-powered 
economic pattern recognition, covenant viability scoring, reporting 
anomaly detection, and multilingual participant interfaces.

This repository contains the fifth core primitive of the Zivana 
Protocol. Where the other four primitives handle identity, trust, 
covenants, and distribution, the Intelligence primitive answers the 
question that every capital provider asks before committing to any 
economic covenant:

> *Is this covenant viable? And is it performing honestly?*

No existing infrastructure answers this question for informal economy 
participants. Their markets are underdocumented, their pricing is 
informal, and their revenue patterns do not fit the assumptions built 
into traditional financial risk models. This primitive is built 
specifically for them.

---

## What lives here

| Component | Purpose |
|---|---|
| `models/market-benchmark` | Learns price distributions, volume patterns, and margin norms for any product category in any jurisdiction from integrated marketplace data |
| `models/anomaly-detection` | Detects statistically anomalous reporting patterns in active covenants — flags deviations from expected ranges for enhanced review |
| `models/multilingual` | Participant-facing language models supporting Yoruba, Igbo, Hausa, Nigerian Pidgin English, Swahili, and English |
| `agents/market-intelligence` | Fetch.ai uAgent deployed on Agentverse — orchestrates all intelligence capabilities and serves outputs to the Zivana oracle layer |
| `oracle-bridge` | Connects agent intelligence outputs to Orcfax COOP fact statement format — makes AI-derived signals consumable by the Zivana distribution validator |
| `sdk` | ZVN.Intelligence namespace — the developer-facing API exposing all intelligence capabilities to application builders |

---

## The Four Intelligence Capabilities

### 1. Market Benchmarking

Learns what normal looks like for any economic activity in any 
jurisdiction by continuously monitoring integrated data sources. 
Produces price distributions, typical margin ranges, seasonal 
patterns, and volume norms for any product or service category.

```typescript
// Generic — works for any product category, any jurisdiction
ZVN.Intelligence.marketBenchmark({
  category: string,        // product or service type
  jurisdiction: string,    // ISO 3166 country or region code
  sources: string[],       // data source IDs to learn from
  period: DateRange
})
// Returns: price distribution, typical margin range,
// seasonal patterns, volume norms, confidence score
```

This capability is completely generic. A health savings application 
uses it to benchmark medical costs. A talent marketplace uses it to 
benchmark fair wages. A trade credit network uses it to benchmark 
commodity prices. The protocol does not know or care what the 
category is — it learns the pattern from the data.

---

### 2. Covenant Viability Scoring

Before a covenant is created, analyses the proposed terms against 
real market data to produce a viability assessment. Answers whether 
the covenant is structurally sound given what the market actually 
looks like — not what the participant hopes it will look like.

```typescript
ZVN.Intelligence.covenantViability({
  schema: string,           // covenant schema reference
  fundingAmount: number,    // amount in smallest currency unit
  category: string,         // economic activity type
  jurisdiction: string,     // market location
  proposedTerms: object     // schema-defined covenant terms
})
// Returns: viability score, realistic performance range,
// risk factors, comparable covenant performance,
// suggested term adjustments
```

The viability score is presented to both parties before covenant 
creation — not as a gate that blocks participation, but as informed 
context that helps both parties set realistic expectations. A low 
viability score does not prevent a covenant. It informs it.

---

### 3. Reporting Anomaly Detection

Monitors all reporting events across active covenants and detects 
statistically anomalous patterns that may indicate underreporting, 
overreporting, or other inconsistencies requiring explanation.

```typescript
ZVN.Intelligence.anomalyDetection({
  covenantId: string,
  reportedValue: number,      // reported figure in smallest unit
  period: DateRange,
  historicalReports: Report[]
})
// Returns: anomaly score, deviation from expected range,
// comparable covenant benchmarks, pattern classification,
// recommended action (proceed / flag / review)
```

**Critical design principle:** An anomaly flag is not an accusation. 
It is a prompt for explanation. Business has natural variance — a 
market disruption, a health event, a supply chain failure — can all 
produce legitimately low reporting periods. The flag creates a 
verifiable record that the reporting was anomalous and opens a 
structured explanation process. It does not trigger automatic 
penalties.

The statistical model uses LSTM-based time series analysis for 
sequential pattern detection and Isolation Forest for point anomaly 
detection. Language models are not used for numerical anomaly 
detection — these classical ML approaches are the correct tools 
for this problem.

---

### 4. Multilingual Participant Interface

Provides natural language interaction for protocol participants in 
their preferred language. Built on open-weight models specifically 
trained on African language data.

```typescript
ZVN.Intelligence.languageInterface({
  message: string,
  language: LanguageCode,    // yo (Yoruba), ig (Igbo),
                             // ha (Hausa), pcm (Pidgin),
                             // sw (Swahili), en (English)
  context: InterfaceContext  // onboarding / covenant / reporting
                             // / support / guidance
})
// Returns: response in requested language,
// suggested next actions, relevant protocol guidance
```

This capability exists because the communities Zivana exists to 
serve conduct their economic lives primarily in languages other 
than English. A protocol that only speaks English to Yoruba traders 
is not serving them — it is asking them to serve it.

---

## Technical Architecture

### Model Stack

| Model | Purpose | Approach |
|---|---|---|
| Market Benchmark | Price and volume learning | Time series regression on integrated data sources |
| Anomaly Detection | Reporting fraud signal | LSTM + Isolation Forest |
| Viability Scoring | Covenant feasibility | Gradient boosting on historical covenant performance |
| Language Interface | Multilingual participant support | Aya open-weight model (101 languages) + domain fine-tuning |

---

### Compute Infrastructure

All models run on **ASI Cloud** — the permissionless GPU cloud 
from the Artificial Superintelligence Alliance. This eliminates 
GPU infrastructure management overhead while maintaining full 
control over the models themselves.

Sensitive economic data is processed by models the protocol 
controls, not by third-party model providers. ASI Cloud provides 
the compute. Zivana controls the intelligence.

---

### Agent Orchestration

The **Zivana Market Intelligence Agent** is deployed on 
**Fetch.ai Agentverse** as a uAgent. It:

- Continuously monitors integrated data sources for market signals
- Computes and updates benchmark distributions per category and jurisdiction
- Responds to viability scoring requests from the covenant primitive
- Publishes anomaly signals to the Zivana oracle layer
- Serves multilingual interface requests from application builders

The agent is the orchestration layer. The models are the 
intelligence. The oracle bridge is the connection to the 
on-chain protocol.

---

### Oracle Bridge

Intelligence outputs reach the on-chain protocol through the 
Orcfax COOP fact statement format. The oracle bridge translates 
agent outputs into signed fact statements published on Cardano:

AI signal → Oracle bridge → COOP fact statement → Cardano on-chain → Zivana distribution validator

This keeps AI intelligence verifiably connected to on-chain 
execution without hardcoding AI decisions into smart contracts. 
The smart contract acts on oracle-attested facts. The AI 
produces those facts. The separation is intentional and 
architecturally sound.

---

## Ecosystem Connection

Zivana Intelligence is built on the 
[Artificial Superintelligence Alliance](https://superintelligence.io) 
ecosystem — specifically ASI Cloud for compute infrastructure and 
Fetch.ai Agentverse for agent orchestration. Both have established 
relationships with the Cardano ecosystem, making this a 
within-ecosystem integration rather than a cross-ecosystem 
dependency.

The intelligence capabilities built for the Zivana Protocol are 
designed to be composable and extensible. As the protocol matures, 
the market benchmarking and anomaly detection components will be 
made available as standalone services that any builder in the 
decentralised AI ecosystem can integrate — not locked inside Zivana.

This is what it means to build regeneratively at the AI layer: 
the intelligence we build for our own protocol becomes 
infrastructure that other builders inherit.

---

## Data Sources

### Primary Sources
Integrated e-commerce and mobile money platforms providing real 
transaction and pricing data for the markets the protocol serves.

### Secondary Sources
Government statistical data — National Bureau of Statistics 
(Nigeria) and equivalent agencies in other operating jurisdictions 
— providing commodity price surveys and market activity data.

### Protocol-Generated Sources
Historical covenant performance data accumulated on the Zivana 
Protocol itself. This is the most valuable long-term data source 
because it is specific to the exact economic actors and markets 
the protocol serves. It grows in value as the protocol scales.

---

## Development Status

> **Phase 1** — Model architecture design and initial training 
> data pipeline setup

> **Phase 2** — Market benchmark and anomaly detection models 
> trained and validated. Fetch.ai uAgent deployed on Agentverse. 
> Oracle bridge operational. ZVN.Intelligence SDK namespace 
> published.

> **Phase 3** — Multilingual interface models fine-tuned and 
> deployed. Intelligence capabilities made available as 
> composable services for other protocol builders.

---

## Getting Started

Prerequisites:
- Python 3.9+
- Node.js 18+
- ASI Cloud account — [cloud.asi.io](https://cloud.asi.io)
- Fetch.ai Agentverse account — [agentverse.ai](https://agentverse.ai)

```bash
git clone https://github.com/zivana-labs/zivana-intelligence.git
cd zivana-intelligence

# Python environment for model development
python -m venv venv
source venv/bin/activate    # Windows: venv\Scripts\activate
pip install -r requirements.txt

# Node environment for SDK and oracle bridge
npm install
```

Full setup guide: coming in Phase 1 documentation.

---

## Contributing

Read the [contributing guidelines](https://github.com/zivana-labs/.github/blob/main/CONTRIBUTING.md) 
before opening a pull request. All PRs target the `develop` branch.

Contributions to the multilingual model fine-tuning are especially 
welcome from contributors with native fluency in Yoruba, Igbo, 
Hausa, Nigerian Pidgin English, or Swahili. Language quality in 
the participant interface directly affects whether the protocol 
serves the communities it exists for.

## Licence

MIT — see [LICENSE](./LICENSE)

---

*Part of [Zivana Labs](https://github.com/zivana-labs) —  
built for African, open to the world.*
