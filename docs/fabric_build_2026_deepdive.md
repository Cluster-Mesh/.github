# 📚 Microsoft Fabric @ Build 2026 — Deep Dive di ogni annuncio

> Documento di approfondimento in italiano: per **ogni singolo punto** dell'[overview](fabric_build_2026_overview.md), trovi qui **cos'è, come funziona, perché conta, scenari d'uso, link, sessioni**. Compagno tecnico al file overview.

📰 Fonti primarie:
- Hero blog Arun Ulag — https://azure.microsoft.com/en-us/blog/microsoft-build-2026-building-agentic-apps-with-microsoft-fabric-and-microsoft-databases/
- Bogdan Crivat (Analytics Stack) — https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Building-the-agentic-analytics-stack-Fabric-Analytics-at-Build/ba-p/5191634
- Fabric IQ semantic layer — https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Fabric-IQ-The-semantic-layer-powering-trusted-AI-agents-at/ba-p/5190739
- Power BI @ Build 2026 (Mohammad Ali) — https://community.fabric.microsoft.com/t5/Power-BI-Updates-Blog/Power-BI-at-Microsoft-Build-2026-The-Agentic-Era-of-analytics/ba-p/5191671
- Building in the Agentic Era with Power BI and Fabric (Sujata Narayana) — https://community.fabric.microsoft.com/t5/Power-BI-Updates-Blog/Building-in-the-Agentic-Era-with-Power-BI-and-Fabric/ba-p/5190754
- DAX User-Defined Functions (Generally Available) — https://community.fabric.microsoft.com/t5/Power-BI-Updates-Blog/DAX-User-Defined-Functions-Generally-Available/ba-p/5185738
- Copilot in web modeling (Preview) — https://community.fabric.microsoft.com/t5/Power-BI-Updates-Blog/Copilot-in-web-modeling-Preview/ba-p/5182287
- The Era of the Agentic Database Developer: Microsoft SQL @ Build 2026 — https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/The-Era-of-the-Agentic-Database-Developer-Microsoft-SQL/ba-p/5190062
- Web IQ — https://blogs.bing.com/search/June-2026/Announcing-Microsoft-Web-IQ
- Rayfin — https://github.com/microsoft/rayfin

---

## 🧭 Indice

0. [Il tema portante: il problema del contesto](#0)
1. [Microsoft IQ — la famiglia delle 4 IQ](#1)
2. [Rayfin — From prompt to production backend](#2)
3. [Web IQ — grounding API per agenti](#3)
4. [Fabric IQ GA](#4)
   - 4.1 [Layer 1 — Unified Data (OneLake)](#41)
   - 4.2 [Layer 2 — Business Intelligence (semantic model)](#42)
   - 4.3 [Layer 3 — Operational Intelligence (ontologie)](#43)
   - 4.4 [Graph in Fabric GA](#44)
   - 4.5 [Planning in Fabric GA](#45)
   - 4.6 [Fabric IQ in Foundry & Agent 365](#46)
   - 4.7 [Fabric IQ in M365 Copilot (Cowork / Chat)](#47)
   - 4.8 [Fabric IQ in GitHub Copilot CLI](#48)
   - 4.9 [Novità Ontology/MCP emerse ieri](#49)
5. [Operations Agents GA](#5)
6. [OneLake — shortcuts, Private Link, catalog in Foundry](#6)
7. [Fabric Data Warehouse — GPU acceleration (CoddSpeed)](#7)
   - 7.1 [Cache Cooldown Configurability](#71)
   - 7.2 [Reimagined Web UI](#72)
8. [Data Engineering & Data Science](#8)
   - 8.1 [Native Execution Engine esteso](#81)
   - 8.2 [Efficient Scaledown](#82)
   - 8.3 [Lakehouse Query Explorer](#83)
   - 8.4 [Fabric Runtime 2.0](#84)
   - 8.5 [MLflow 3 in Fabric](#85)
9. [Power BI & Agentic Analytics](#9)
   - 9.1 [Agent Skills for Power BI Preview](#91)
   - 9.2 [Copilot modifica semantic model](#92)
   - 9.3 [Fabric apps su semantic model](#93)
   - 9.4 [Agent Skills for Fabric OSS](#94)
   - 9.5 [Org Apps in Fabric — GA in arrivo + Audiences](#95)
   - 9.6 [DAX User-Defined Functions — GA](#96)
   - 9.7 [Copilot in Web Modeling (Preview)](#97)
10. [Fabric Data Agents — wave di enhancement](#10)
11. [Real-Time Intelligence & Business Events](#11)
12. [Microsoft Databases](#12)
    - 12.1 [Azure HorizonDB](#121)
    - 12.2 [Azure Database for PostgreSQL — Defender + migration tooling](#122)
    - 12.3 [Azure Cosmos DB — Linux Emulator GA, semantic rerank, agent memory toolkit](#123)
    - 12.4 [Database Hub in Fabric](#124)
   - 12.5 [Microsoft SQL: macro-area "Agentic Database Developer"](#125)
13. [Sicurezza, governance, capacity](#13)
14. [Fabric Data Factory](#14)

---

<a id="0"></a>
## 0. 🎯 Il tema portante: il problema del **contesto condiviso**

### Cos'è
Il messaggio di apertura del keynote di Arun Ulag a Build 2026: oggi i **modelli sono capaci**, ma ogni agente nasce **senza contesto** del business. Senza un layer condiviso di significato (chi è un cliente, cos'è un ordine, quali regole valgono), gli agenti **non possono coordinarsi, scalare, né operare in modo affidabile**.

### Perché conta
> *"The challenge is no longer model capability, but consistent, shared data context across the business."*
> — Arun Ulag, EVP Azure Data

In pratica: un'azienda non scala da 1 agente di POC a 100 agenti in produzione finché ogni agente continua a **rilearnare il modello di business** da prompt e dati frammentati.

### Risposta di Microsoft a Build 2026
Costruire una **AI-ready data foundation** end-to-end che includa:
1. **Dati unificati** (OneLake)
2. **Semantica governata** (Power BI semantic model)
3. **Ontologie operative** (Fabric IQ)
4. **Agenti capaci di agire** (Operations Agents, Data Agents)
5. **Backend agent-ready** per le app (Rayfin)
6. **Grounding esterno** (Web IQ)

Tutto sotto il cappello **Microsoft IQ**.

---

<a id="1"></a>
## 1. 🧠 Microsoft IQ — la famiglia delle 4 IQ

### Cos'è
**Microsoft IQ** è un *concept* / cornice di prodotto introdotto al Build 2026 per descrivere il **layer di intelligenza condivisa** dell'enterprise. Quattro pillar interconnessi:

| IQ | Cosa cattura/modella | Dove vive |
|---|---|---|
| **Work IQ** | Come si lavora (segnali da Microsoft 365, Graph, attività) | Microsoft 365 |
| **Fabric IQ** | Come opera il business (dati + semantica + ontologie + live signals) | Microsoft Fabric |
| **Foundry IQ** | Knowledge riusabile dagli agenti (RAG, ground sources) | Microsoft Foundry |
| **Web IQ** | Contesto real-time dal web pubblico | Bing / API |

### Come funzionano insieme
- Un agente in Foundry può usare **Foundry IQ** per le sue knowledge source, **Fabric IQ** per i fatti enterprise, **Work IQ** per il contesto utente, **Web IQ** per fatti freschi dal web.
- È **MCP-native**: ogni IQ espone Model Context Protocol tool e può essere consumato da Foundry, Agent 365, M365 Copilot, GitHub Copilot CLI.

### Perché conta
È la prima volta che Microsoft mette **un nome ombrello** sull'intera offerta di "contesto per agenti", trasformando ciò che oggi sono prodotti separati (Fabric, Foundry, M365 Copilot, Bing) in un **system of intelligence** coerente.

---

<a id="2"></a>
## 2. 🐟 **Rayfin** — From prompt to production backend

> Documento dedicato: [rayfin.md](rayfin.md). Qui il **riassunto deep-dive** dei punti chiave.

### Cos'è
**Rayfin** è un **SDK + CLI open-source** (MIT, GitHub: [microsoft/rayfin](https://github.com/microsoft/rayfin)) che fa una cosa precisa: permette a developer e ad agenti di coding di **descrivere a parole o in codice** cosa deve fare il backend di un'applicazione, e ottenere automaticamente:
- **Database** con schema, vincoli, migrazioni
- **Autenticazione** (Fabric SSO + email/password)
- **Authorization / policy**
- **Storage**, code e eventi
- **Endpoint API tipizzati**

…tutto **deployato direttamente su Microsoft Fabric** come **artefatto di prima classe**.

### Come funziona — building blocks
- **Decorator TypeScript** (`@entity`, `@field`, `@policy`, `@action`) per definire data model + logica in modo dichiarativo
- **CLI** (`rayfin init`, `rayfin dev`, `rayfin deploy`, `rayfin migrate`, …)
- **Pacchetti npm** ufficiali (`@microsoft/rayfin-core`, `@microsoft/rayfin-fabric`, …)
- **Templates** (gallery `awesome-rayfin`) per kickstart use case comuni
- **Workflow GitHub-based**: tutto come codice nel repo → review, branch, CI/CD

### Perché conta
- I dati dell'app **atterrano direttamente in OneLake** → immediatamente integrabili con analytics, semantic model, agenti, real-time
- Niente più "infrastruttura collaterale" da mantenere fuori da Fabric
- Ponte naturale tra **vibe-coding / agent coding** e **enterprise-grade backend**

### Scenari d'uso
- App di **planning** che scrive su Fabric e legge dal semantic model
- App **operational** (inventory, ticketing) sopra OneLake con governance Fabric
- Backend per **Fabric Apps** (vedi documento dedicato `fabric_apps.md`)

### Partnership annunciata
**Replit** — i loro coding agent producono app Rayfin che escono direttamente in Fabric:
> *"Rayfin unlocks a new development model… Agents write the code. Fabric ships it quickly and safely."* — Amjad Masad, CEO Replit

### Sessione Build
**BRK225** — *Data, apps, and agents: the future of app dev with Microsoft Fabric* (3 giu, 13:30 PT) — https://aka.ms/Build-BRK225

---

<a id="3"></a>
## 3. 🌐 **Web IQ** — Grounding APIs per agenti

> Documento dedicato: [web_iq.md](web_iq.md).

### Cos'è
Suite di **API di grounding AI-native** (web, news, immagini, video) annunciata al Build 2026. È il **pilastro "web"** di Microsoft IQ. Sostituisce/affianca le API tradizionali di Bing search, ma è progettata **per agenti, non per browser**.

### Differenze chiave vs Bing Search API
| Caratteristica | Bing Search | Web IQ |
|---|---|---|
| Target | Browser, motore di ricerca | LLM, agenti |
| Output | Lista di link + snippet | **Evidence object** + chunk semantici |
| Protocollo | REST | **MCP-native** + REST |
| Ottimizzazione | CTR umani | **Token efficiency** + grounding satisfaction |
| Embedding | n/a | **Harrier (OSS)** + DiskANN3 |

### Come funziona — sotto il cofano
- **Foundation**: indice web di Bing
- **Embedding model**: **Harrier**, rilasciato open-source (vedi blog Bing)
- **Vector ANN**: **DiskANN3**
- **Output**: oggetti `Evidence` con `source_url`, `chunk`, `score`, `recency`, `attribution`
- **Metric ufficiale**: **GDSAT** (Grounding Data Satisfaction) — 79,05 su benchmark interno, ~2,5× il best alternative

### Numeri annunciati
- **P95 latency < 165 ms**
- **2,5× più veloce** del miglior competitor
- **Token efficiency** superiore (meno token, più rilevanza)
- Già in produzione su **Copilot, ChatGPT, Nasdaq**

### Perché conta
- Gli agenti hanno bisogno di fatti **freschi e attribuiti**, non di pagine HTML
- Risolve **hallucination** e **out-of-date knowledge** dei modelli
- È il **counterpart "external"** di Fabric IQ (che è "internal")

### Accesso
Limited access waitlist: https://aka.ms/webiq-waitlist

---

<a id="4"></a>
## 4. 🧠 **Fabric IQ** — General Availability

### Cos'è
**Fabric IQ è ora GA**. È il **shared context layer** di Fabric: un sistema integrato di **dati + semantica + ontologie + segnali live** che consente ad agenti e applicazioni di **ragionare nel linguaggio del business**.

### Architettura a 3 layer

```
┌─────────────────────────────────────────────────────────┐
│   Layer 3 — Operational Intelligence (ONTOLOGIES)       │
│   entità, relazioni, properties, regole, actions, live  │
├─────────────────────────────────────────────────────────┤
│   Layer 2 — Business Intelligence (SEMANTIC MODELS)     │
│   measures, hierarchies, definitions Power BI           │
├─────────────────────────────────────────────────────────┤
│   Layer 1 — Unified Data (ONELAKE)                      │
│   tabelle delta, mirroring, shortcuts, lakehouse, KQL   │
└─────────────────────────────────────────────────────────┘
```

<a id="41"></a>
### 4.1 Layer 1 — Unified Data (OneLake)
- Tutti i dati in un **unico data lake AI-ready**
- Open formats (Delta Parquet), governance unica, security unica
- **Source of truth** per i layer superiori

<a id="42"></a>
### 4.2 Layer 2 — Business Intelligence (semantic model)
- **Semantic model di Power BI** (Tabular / TMSL) usati come **fonte di verità per le metriche**
- Definiscono: relazioni, gerarchie, **measure DAX**, hierarchies, perspectives, RLS/OLS
- Gli agenti ragionano sulle measure (es. *"Net Sales LY"*) anziché su SUM raw

<a id="43"></a>
### 4.3 Layer 3 — Operational Intelligence (ontologie)

**Ontologia** = grafo concettuale che modella il business in 5 dimensioni:
- **Entità** (Customer, Order, Supplier, Site, Asset, …)
- **Relazioni** (Customer *places* Order; Supplier *delivers* Material)
- **Properties** (campi, tipi, vincoli)
- **Rules** (es. "se ritardo > 3gg allora flag RISK")
- **Actions** (cosa può fare un agente — es. *"crea ticket"*, *"riassegna ordine"*)

Connessione **live** ai segnali di **Fabric Real-Time Intelligence** (Eventstream, Eventhouse, Activator) → l'ontologia non è statica, riflette **lo stato del business adesso**.

GA prevista nei prossimi mesi.

<a id="44"></a>
### 4.4 Graph in Fabric — GA

**Cos'è**: grafo nativo Fabric, **relationship-first**, scalabile a livello enterprise.

**Cosa abilita**:
- Connettere entità di business, sistemi e segnali
- Capire come i cambiamenti **si propagano** (es. ritardo fornitore → impatto SKU → impatto store → impatto revenue)
- Query di grafo native a fianco di SQL e KQL

**Per chi**: data architect, analyst, operations agent.

<a id="45"></a>
### 4.5 Planning in Fabric — GA later this month

**Cos'è**: capacità di creare **plans, budgets, forecast, scenari** sopra i semantic model di Power BI.

**Differenza chiave vs altri tool di planning**: i risultati **possono essere riscritti dentro Fabric** → **closed-loop** tra planning ed execution sullo stesso sistema di dati.

**Scenari**:
- Financial planning (budget vs actual con write-back)
- S&OP (Sales & Operations Planning)
- Workforce planning

<a id="46"></a>
### 4.6 Fabric IQ in Microsoft Foundry & Agent 365

| Destinazione | Modalità |
|---|---|
| **Foundry** (preview) | Ontologie accessibili come **knowledge source** negli agenti custom |
| **Agent 365** (preview) | Fabric IQ esposto come **first-party MCP tool** |

→ Stessa ontologia governata, usata coerentemente in tutti gli agenti.

<a id="47"></a>
### 4.7 Fabric IQ in M365 Copilot (Cowork + Copilot Chat)
- Disponibile per **clienti Frontier** con licenza M365 Copilot
- Report Power BI e semantic model **compaiono direttamente nei flussi M365** (Cowork, Chat)
- L'utente può **scoprire dati**, **chiederne insight**, e l'agente può **agire** (creare follow-up, ticket, eventi)
- Combinato con **Work IQ** → unified intelligence layer

[Programma Frontier](https://www.microsoft.com/en-us/microsoft-365-copilot/frontier-individuals)

<a id="48"></a>
### 4.8 Fabric IQ in GitHub Copilot CLI
- Via **Agent Skills for Fabric** (OSS: https://github.com/microsoft/skills-for-fabric)
- Il developer dal **terminale** chiede in NL: *"come è andato l'uptake della feature X la settimana scorsa per i clienti enterprise?"*
- La CLI interroga Power BI / semantic model, **risposta grounded** con citazione metrica e definizione
- Workflow product/eng decisions ancorate a dati reali, senza switch di tool

Sessione Build: **OD812** — *Bringing Enterprise Ontology Directly into the Developer Workflow* — https://aka.ms/OD812

<a id="49"></a>
### 4.9 Novità Ontology/MCP emerse ieri

Dal post dedicato su Fabric IQ (pubblicato ieri), ci sono alcune precisazioni importanti da aggiungere:

| Area | Novità | Stato |
|---|---|---|
| **Ontology MCP** | Connessione di agenti esterni via MCP a entità/relazioni/mappature governate | Preview |
| **Foundry IQ integration** | Ontology utilizzabile come knowledge source negli agenti Foundry | Preview |
| **Modeling velocity** | Supporto a untyped properties, metadata enrichment, omonimie relazione/proprietà | Release wave |
| **Governance** | Item-level access control per collaborazione multi-team | Release wave |
| **Authoring UX** | Miglioramenti canvas/affidabilità query + self-guided walkthrough | Release wave |

**Perché conta**: riduce il tempo di modellazione ontologica e rende più semplice portare il contesto Fabric IQ dentro agenti esterni in modo governato.

---

<a id="5"></a>
## 5. 🤖 **Operations Agents — GA**

### Cos'è
Agenti **nativi Fabric**, integrati con Microsoft Foundry, progettati per il **loop chiuso operativo**: monitorare → ragionare → **agire**.

### Come funzionano
1. **Sense** — agganciati a Real-Time Intelligence (Eventstream/Eventhouse), monitorano stream di eventi
2. **Reason** — usano le **ontologie Fabric IQ** per interpretare i segnali in contesto di business
3. **Act** — eseguono **actions definite nell'ontologia** (creare ticket, riassegnare ordine, lanciare workflow Power Automate, notificare team)

### Differenza vs Data Agents
- **Data agent** = "rispondi a domande sui miei dati" (NL2SQL, RAG, conversational)
- **Operations agent** = "agisci sui miei dati / sul mio business in modo continuativo"

### Scenari concreti
- Supply chain: ritardo fornitore → re-route automatico ordini critici
- Retail: anomalia di vendita → ricomposizione assortimento
- Manutenzione: sensore IoT fuori soglia → ticket + ordine ricambio

### Governance
Tutte le actions sono **policy-gated** dall'ontologia → un agente non può fare cose che non sono dichiarate ammesse.

---

<a id="6"></a>
## 6. 🗂️ OneLake — shortcuts, Private Link, catalog in Foundry

### 6.1 Shortcuts a **SharePoint e OneDrive** (GA)

**Cos'è**: shortcut OneLake che puntano a contenuti di SharePoint Online e OneDrive for Business. Niente copia dati: i file (Excel, CSV) **diventano leggibili** come tabelle/file dentro Fabric.

**Perché conta**: in tante aziende metà del business gira su file Excel in SharePoint. Ora sono **immediatamente disponibili** per analytics e agenti, **senza ETL**.

[Docs](https://learn.microsoft.com/en-us/fabric/onelake/shortcuts/create-onedrive-sharepoint-shortcut)

### 6.2 Shortcuts da **Fabric Data Warehouse** (Preview)
Creare shortcut OneLake che puntano direttamente a tabelle del DW → consumabili come Delta da Spark/notebook senza export.

### 6.3 **Workspace-level Azure Private Link** per mirrored data sources (Preview)
Mirroring di sorgenti operazionali (Cosmos, Snowflake, Postgres, SQL DB) **dietro Private Link a livello workspace** → traffico privato, niente esposizione internet.

### 6.4 **OneLake catalog in Microsoft Foundry — GA**
- OneLake catalog (discovery + metadata di tutti gli asset OneLake) **embedded nell'esperienza Knowledge di Foundry**
- Un agent author in Foundry può **scoprire un dataset enterprise** e **collegarlo come knowledge source** in pochi click
- Closing del gap tra **data discovery** e **AI development**

Sessione Build: **OD815** — *Unify your entire data estate on a single, AI-ready data lake* — https://aka.ms/OD815

---

<a id="7"></a>
## 7. 🏎️ **Fabric Data Warehouse — GPU acceleration (CoddSpeed)**

### Cos'è
Fabric Data Warehouse diventa il **primo data warehouse fully managed con GPU acceleration**. La tecnologia si chiama **CoddSpeed** (in onore di E.F. Codd).

### Riconoscimento accademico
**Best Industry Paper Award @ SIGMOD 2026** — paper: *CoddSpeed: Hardware Accelerated Query Processing in Microsoft Fabric* — http://aka.ms/coddspeed

### Come funziona
- **NVIDIA accelerated computing** + **custom CUDA kernel** integrati direttamente nel motore SQL del DW
- Funziona su **SELECT T-SQL** standard, **senza riscrivere** query né schema
- Abilitazione **workspace-level setting** (toggle on/off)
- Monitoraggio integrato

### Numeri
- **Fino a 7×** più veloce di tre vendor competitor comparabili a **64-user concurrency** (benchmark interno Microsoft, maggio 2026)
- Il vantaggio **cresce con la concorrenza** (workload tipici degli agenti)
- **End-to-end response time** di workflow data-agent **fino a -50%**

### Customer proof
- **UNC Health**: *"up to 5× improvement in our query speeds"* — Shaun McDonald
- **WTW**: *"complex workloads running 3,4× faster at single concurrency"* — Andrew Bradbrook

### Perché conta per gli agenti
Un data agent traduce NL→SQL **in millisecondi**. Se il DW poi impiega secondi, l'esperienza non è conversazionale. GPU acceleration porta il DW alla **velocità degli agenti**.

### Disponibilità
**Early Access Preview a luglio 2026** — sign-up: http://aka.ms/GPU-FabricDW

Sessione Build: **OD813** — *Powering modern data analytics in Fabric Data Warehouse*

<a id="71"></a>
### 7.1 Cache Cooldown Configurability

**Cos'è**: parametro che permette di configurare **per quanto tempo** Fabric DW tiene "calda" la cache delle query.

**Perché conta**: trade-off **performance vs cost** in mano al cliente.
- Cache lunga → più query servite warm, latency bassa, ma compute più alto
- Cache corta → risparmio, ma più cold start

Cache node **scalano auto su/giù** in base alla domanda → mai over-provisioned.

**Stato**: disponibile *in the coming weeks* (giugno-luglio 2026).

<a id="72"></a>
### 7.2 Reimagined Web UI

Restyle completo dell'esperienza web del DW:
- **Object Explorer**, **Data Grid**, **IntelliSense** più veloci/smart
- Nuova **table overview page** (struttura tabella a colpo d'occhio)
- **Query management** semplificato: copy query, import/export `.sql`
- **Copilot Chat inline nel SQL editor** → resta nel flusso mentre itera SQL

---

<a id="8"></a>
## 8. ⚡ Data Engineering & Data Science

<a id="81"></a>
### 8.1 Native Execution Engine (NEE) — esteso

**Cos'è**: motore C++ vectorized che esegue query Spark in modo nativo, bypassando la JVM dove possibile.

**Novità a Build 2026**:
- **Complex data types** (array, map, struct) processati **interamente nel motore nativo**
- **UDF native** in **Python, Scala, Java** → niente più serializzazione JVM↔nativo per le UDF
- Supporto pieno per **Z-ORDER** e **Liquid Clustering** senza fallback
- **Zero code change**: la stessa pipeline va più veloce

**Perché conta**: la maggior parte dei dataset moderni (logs, eventi, JSON) ha tipi nested. Prima si tornava alla JVM; ora no.

<a id="82"></a>
### 8.2 Efficient Scaledown

**Problema**: in Spark, gli executor non possono morire se contengono **shuffle data** richieste da altri stage → cluster restano accesi inutilmente.

**Soluzione**: **Remote Shuffle Manager** che **offloada lo shuffle su Azure Blob Storage**.
- Executor possono **morire subito** dopo il loro lavoro
- Routing intelligente: chi serve i dati shuffle li recupera da Blob
- Cluster **scalano giù aggressivamente** → niente compute idle
- Particolarmente prezioso per workload **bursty AI-driven**

**Impatto**: cost saving significativo su pattern "esplosione carico + ritorno a zero" tipici degli agenti.

<a id="83"></a>
### 8.3 Lakehouse Query Explorer (Coming Soon)

**Cos'è**: esperienza di esplorazione query **in-context dentro il Lakehouse**, senza dover aprire un notebook.

**Cosa puoi fare**:
- Scrivere ed eseguire **Spark SQL** con IntelliSense e error prevention real-time
- Salvare il risultato come **Spark View**
- **Visualizzare** come chart, filtrare/ordinare interattivamente
- **Download** output (CSV, Parquet)
- **Promote** a notebook con un click per analisi avanzata

**Per chi**: data engineer, analyst, ad-hoc explorer.

**Perché conta per gli agenti**: superficie unificata di query → più facile per AI tool fare "explain this dataset".

<a id="84"></a>
### 8.4 Fabric Runtime 2.0

| Componente | Versione |
|---|---|
| Apache Spark | **4.1** |
| Delta Lake | **4.1** |
| Python | **3.13** |

Più: NEE esteso (vedi 8.1), ottimizzazioni Delta più profonde, integrazione più stretta con i servizi Fabric.

<a id="85"></a>
### 8.5 MLflow 3 in Fabric

**Cosa porta MLflow 3** (versione open-source recente):
- **Tracing** nativo per workflow LLM/GenAI
- **Logged Models** (modelli e versioni come asset di prima classe)
- **UX rinnovata** per esperimenti e run

**Tutto integrato in Fabric** → esperimenti, modelli, traces in un'unica esperienza.

Sessione Build: **OD818** — *The AI-native data engineer*

---

<a id="9"></a>
## 9. 📊 Power BI & Agentic Analytics

<a id="91"></a>
### 9.1 Agent Skills for Power BI (Preview)

**Cos'è**: skill agentic che permette a un AI agent di **costruire end-to-end un'esperienza Power BI**:
- Da **raw data** → **semantic model** → **report interattivo**
- Input ammessi: descrizione testuale, **screenshot** dell'outcome desiderato, requirement in linguaggio naturale
- Il flusso è **iterativo**: l'agent propone, l'utente raffina

**Perché conta**: democratizza la creazione di report enterprise. Anche chi non conosce DAX produce modelli e visual coerenti.

<a id="92"></a>
### 9.2 Copilot in Power BI può modificare i semantic model

**Cos'è**: Copilot ora può **modificare** un semantic model esistente (non solo proporre measure).

**Cosa fa**:
- Suggerisce e applica **best practice** (denormalizzazione, gerarchie, perspectives)
- Migliora le **performance** del modello
- Aumenta la **AI-readiness** (descrizioni, sinonimi, annotation che gli agenti useranno)

**Perché conta**: i semantic model **alimentano gli agenti**. Modelli AI-ready → agenti più accurati.

<a id="93"></a>
### 9.3 Fabric apps su semantic model (via Rayfin)

**Cos'è**: con **Rayfin**, un coding agent può generare **un'intera web app** che usa il semantic model come backend logico — ereditando measure, hierarchies, RLS.

**Esempi mostrati a Build**:
- App di **financial planning** con calendario Outlook integrato
- App di **inventory management**
- App di **pricing optimization**
- App di **persona-specific views** (UI diversa per ruolo)

**Perché conta**: il confine tra "report Power BI" e "app web custom" si dissolve. Stessa fonte di verità, esperienza utente su misura.

<a id="94"></a>
### 9.4 Open-source Agent Skills for Fabric

Repo: https://github.com/microsoft/skills-for-fabric

**Cos'è**: pacchetti di skill (in formato compatibile con Foundry, GitHub Copilot CLI, M365 Copilot) che permettono ad AI tool di interagire programmaticamente con Fabric.

**Esempi di skill inclusi**:
- Query semantic model in NL
- Lista artefatti workspace
- Esecuzione notebook
- Trigger pipeline
- Lettura ontologie

Sessione Build: **OD817** — *Agentic analytics with Power BI and Microsoft Fabric*

<a id="95"></a>
### 9.5 Org Apps in Fabric — GA in arrivo + Audiences

**Novità emersa ieri (post Build 2026)**: il team Power BI conferma la disponibilità **GA "nelle prossime settimane"** della nuova esperienza **Org Apps** in Fabric.

**Cosa abilita**:
- Più org app per workspace
- Esperienze curate e brandizzate per audience diverse
- Distribuzione unificata di contenuti Power BI e Fabric item (report, notebook, dashboard real-time, ecc.)
- **Audience control**: visibilità differenziata degli item per gruppi utenti

**Perché conta**:
completa il passaggio da "costruire analytics agentici" a "distribuirli in modo enterprise-ready" su larga scala.

<a id="96"></a>
### 9.6 DAX User-Defined Functions — GA

I **DAX UDF** sono ora **Generally Available** (non più preview), e diventano componenti di base del semantic layer agent-ready.

**Capability chiave in GA**:
- UDF come oggetti di modello riusabili e discoverable
- **Optional parameters** nelle signature
- Supporto type hints estesi (runtime type safety)
- Authoring/editing anche in web modeling e in Model View
- Tracciamento dipendenze su rename di table/column/measure
- `INFO.USERDEFINEDFUNCTIONS()` per catalogare le funzioni nel modello

**Perché conta per gli agenti**:
gli LLM possono invocare logica business tipizzata e governata (anziché rigenerare DAX ad hoc), migliorando consistenza e affidabilità delle risposte grounded su semantic model.

<a id="97"></a>
### 9.7 Copilot in Web Modeling (Preview)

**Novità post-Day1 rilevante**: in Power BI service arriva **Copilot in web modeling** (preview, rollout questa settimana), con focus su modifica guidata dei semantic model in browser.

**Cosa abilita**:
- Analisi conversazionale della qualità del modello (naming, struttura, relazioni)
- Modifiche schema assistite: rename table/column, creazione relazioni, generazione measure DAX
- Sessioni con consenso esplicito + **restore checkpoint** automatico per rollback

**Perché è una macro-area utile**:
rafforza il "semantic model as control plane" anche fuori dal desktop, ed è il ponte operativo tra governance BI e authoring agentico quotidiano nel servizio.

---

<a id="10"></a>
## 10. 💬 Fabric Data Agents — wave di enhancement

Riepilogo completo delle novità annunciate, ciascuna spiegata:

| Feature | Stato | Spiegazione |
|---|---|---|
| **Service Principal support** | Preview | Auth con SPN (no più solo delegated user). Sblocca backend service, automation, embedding in app custom multi-tenant. |
| **Observability in Microsoft Foundry** | Preview | I Data Agents compaiono in **Foundry Observability** con telemetry: latency, status, error, span. Debug e SLA monitoring centralizzati. |
| **Data Agents in M365 Copilot** | **GA** | Esperienza GA in M365 Copilot; supporto migliorato per **query long-running** (no più timeout su analisi complesse). |
| **AI-assisted setup** per SQL & Eventhouse | Preview | Guided wizard che suggerisce istruzioni, **source guidance**, example query. Riduce drasticamente l'onboarding time. |
| **Preview Runtime** | Disponibile | Runtime opt-in con i comportamenti che saranno default in futuro → team validano updates prima del rollout. |
| **NL2SQL + source routing migliorato** | Preview | Migliore traduzione NL→SQL e migliore scelta della **giusta fonte** in scenari multi-source. |
| **Code Interpreter (Python)** | Preview | Tool Python dentro il Data Agent → **forecasting, statistical analysis, data transformation** oltre la pura query. |
| **Model upgrade a GPT-5.X** | Disponibile | I Data Agents usano modelli GPT-5.X → **+~20% accuracy** in benchmark interni. |
| **Visualizations in Data Agents** | Coming Soon Preview | NL question → **chart inline** nell'esperienza data agent. |

### Perché tutto insieme conta
I Data Agents passano da "**chatbot SQL**" a **piattaforma agent-as-a-service** integrabile, osservabile, programmabile.

---

<a id="11"></a>
## 11. 🔁 Real-Time Intelligence & Business Events

[Blog: *What's new in Fabric Business Events*](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/What-s-new-in-Fabric-Business-Events/ba-p/5189137)

**Cos'è "Business Events"**: meccanismo per **pubblicare eventi significativi del business** (es. "ordine completato", "stock sotto soglia") e farli consumare da:
- Analytics (eventstream → eventhouse)
- Automation (Power Automate, Activator)
- App (webhook, websocket)
- Agenti (Operations Agent reagisce a eventi)

**Novità a Build 2026 (dettaglio)**:

| Capability | Stato | Cosa aggiunge |
|---|---|---|
| **Eventstream come publisher di Business Events** | Preview | Trasforma segnali low-level (es. CDC) in eventi business (es. `OrderCreated`) con filtering/enrichment/correlation |
| **Activator come publisher di Business Events** | Preview | Condizioni rilevate su report, dashboard real-time, query KQL o Warehouse SQL possono emettere Business Events |
| **Analisi Business Events in Eventhouse + Real-Time Dashboard** | Preview | Ogni evento pubblicato diventa queryable in KQL (tabella dedicata), senza pipeline aggiuntive |
| **Modello di capacity per Business Events** | **GA** | Metering trasparente per operazioni evento e listener/orari, con visibilità nella Capacity Metrics app |

### Perché conta
Business Events passa da feature di integrazione a pattern operativo completo: **publish → persist/query → visualize → automate** con governance e costing espliciti.

Sessione Build: **OD819** — *Real-Time Intelligence: Bringing event-driven AI apps & agents* — https://aka.ms/OD819

---

<a id="12"></a>
## 12. 🛢️ Microsoft Databases

<a id="121"></a>
### 12.1 **Azure HorizonDB** — Public Preview (NUOVO)

**Cos'è**: nuovo database PostgreSQL-compatible, **fully managed**, progettato da zero per le **esigenze delle app AI**.

**Specifiche tecniche**:
| Caratteristica | Valore |
|---|---|
| **Compatibilità** | PostgreSQL (drop-in per la maggior parte dei workload) |
| **Resilienza** | **Zone resilient by default** |
| **Storage** | Elastic fino a **128 TB** |
| **Compute** | Scale-out fino a **3.072 vCore** |
| **Commit latency** | **Sub-millisecond multi-zone** |
| **Vector search** | Nativo |
| **AI model management** | Integrato |
| **Connectivity** | Direct → **Microsoft Foundry** e **Microsoft Fabric** |

**Customer quote**:
> *"What stood out with HorizonDB is that it aligns closely with how we already think about the problem. Instead of stitching together multiple components, it brings transactional data, vector search, and AI capabilities into a single platform."*
> — Mohsin Shafqat, Director of Software Engineering, **NASDAQ**

> *"As our data demands have expanded exponentially because of our use of Azure AI to chat with our data, HorizonDB has come at the perfect time."*
> — Rand Morimoto, President, **Convergent Computing**

**Link**: http://aka.ms/azurehorizondb
**Sessione**: **BRK223** — *From rows to reasoning* — https://aka.ms/Build-BRK223

<a id="122"></a>
### 12.2 Azure Database for PostgreSQL — Defender + migration tooling

**1. Microsoft Defender for Cloud integration** (Preview)
- Assessment continuo di **sicurezza e compliance**
- Identifica misconfigurazioni
- Riduzione attiva del rischio

**2. Discovery & assessment tooling per migrazioni**
- Valuta ambienti **Oracle** e **PostgreSQL** esistenti
- Fornisce: **readiness insights**, **sizing guidance**, **cost estimates**
- Riduce il rischio di progetti di migrazione

Sessione: **OD822** — *Smarter PostgreSQL migrations* — https://aka.ms/OD822

<a id="123"></a>
### 12.3 Azure Cosmos DB

**1. Linux Emulator → GA**
- Build/test/validate app **localmente** su **Linux, macOS, Windows**
- **Nessuna dipendenza cloud** in dev → fee saving + workflow CI/CD più semplice

**2. Semantic Reranking** (Preview)
- Migliora la **rilevanza della ricerca** con contextual understanding integrato
- Risposta migliore senza dover montare un servizio esterno

**3. Agent Memory Toolkit** (Preview)
- Pattern standardizzato per **persistent memory** di agenti AI
- Stack: **Cosmos DB** (storage) + **Azure Durable Functions** (orchestrazione) + **Microsoft Foundry** models
- Riduce il boilerplate per costruire agenti con memoria a lungo termine

Customer quote OpenAI:
> *"[We chose Azure Cosmos DB] because of its automatic scaling and schema-less flexibility, allowing us to iterate quickly."*
> — Nick Cooper, Senior Technical Staff Member, **OpenAI**

Sessione: **OD820** — *Designing reliable multi-agent apps with Azure Cosmos DB*

<a id="124"></a>
### 12.4 Database Hub in Fabric — Private Preview

**Cos'è**: nuova esperienza dentro Fabric per **gestire centralmente** i Microsoft Databases (SQL DB, Cosmos, PostgreSQL, HorizonDB).

**Cosa abilita**:
- **Mirroring** automatico in OneLake
- **Operational data** + **analytical data** su unica foundation
- Tooling unificato per provisioning, monitoring, governance
- Da OneLake → trusted, contextual, **AI-ready**

[Blog dettaglio](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blogs/Advancing-Databases-for-the-Next-Generation-of-Applications/ba-p/5172237)

<a id="125"></a>
### 12.5 Microsoft SQL: macro-area "Agentic Database Developer"

Nel giro di verifica è emersa una macro-area non esplicitata abbastanza nel documento: le novità **Microsoft SQL @ Build 2026** che completano il lato "database developer workflow".

| Blocco | Novità chiave | Stato |
|---|---|---|
| **VS Code + MSSQL extension** | Schema Designer NL→schema + output T-SQL/ORM; Data API builder integration per endpoint REST/GraphQL/MCP | **GA** |
| **SQL Notebooks in VS Code** | Supporto `.ipynb` SQL con IntelliSense e mix SQL/Python | **GA** |
| **Provisioning Azure SQL da VS Code** | Provisioning diretto in editor | Preview |
| **SSMS goes Agentic** | GitHub Copilot Agent Mode per troubleshooting/refactor multi-step | Preview |
| **Security by design** | Entra server-level logins + fixed server roles | **GA** |
| **Crypto hardening** | TDE con AES key support | Preview |
| **Event-driven SQL** | Change Event Streaming verso Event Hubs (CloudEvents) | Preview |
| **Fabric Apps + SQL in Fabric** | App end-to-end sopra SQL database in Fabric senza infra separata | Preview |

**Perché conta**:
questa area chiude il gap tra "costruzione app agentiche" e "toolchain database enterprise"; è la controparte developer/DBA delle novità più data-platform già coperte in Fabric IQ, DW e Data Factory.

---

<a id="13"></a>
## 13. 🔒 Sicurezza, governance, capacity

Sessioni focus:
- **OD810** — *Build fast, not fragile on Microsoft Fabric* — https://aka.ms/OD810
- **OD816** — *Securing, scaling, and sustaining your data estate in Microsoft Fabric* — https://aka.ms/OD816

Topics chiave coperti:
- Workspace-level Azure Private Link per mirrored data
- RLS / OLS / Object-level security che si propaga dai semantic model
- Governance ereditata da Rayfin apps deployate in Fabric
- Capacity management e SKU sizing per workload agentic

Altri update collegati:
- **On-premises data gateway** maggio 2026 release (3000.318) — [blog](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/On-premises-data-gateway-May-2026-release/ba-p/5190820)
- **Translytical task flows** per in-report alerts in Power BI — [blog](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Solving-Power-BI-in-report-alerts-with-translytical-task-flows/ba-p/5190667)

---

<a id="14"></a>
## 14. 🏗️ Fabric Data Factory @ Build 2026

[Blog dedicato](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Build-2026-From-data-to-intelligence-Faster-with-Fabric-Data/ba-p/5191636)

**Posizionamento**: Data Factory è la **integration foundation** dello stack agentic. Senza dati freschi, puliti e contestualizzati, nessun agente è affidabile.

**Novità Build 2026 che vale la pena esplicitare**:

| Pillar | Novità principali | Stato |
|---|---|---|
| **Mission-critical integration** | Dataflow Gen2 diagnostics con OPDG logs | Preview |
| **Secure ingestion** | Copy Job / Copy Activity verso DW con topologie private (workspace private link + OAP + gateway) | **GA** |
| **Unified estate** | Mirroring con Workspace Private Link per Azure SQL DB, SQL Server, SAP Datasphere, SharePoint List | Preview |
| **Transformation** | Execute Query API (Power Query streaming) | **GA** |
| **Transformation** | Mapping Data Flows in Dataflow Gen2 (settimana 8 giugno), Warehouse→Lakehouse perf improvements, My Queries | Preview |
| **dbt in Fabric** | dbt pipeline activity, dbt API, export project | Preview |
| **Orchestration** | Refresh SQL analytics endpoint activity | **GA** |
| **Orchestration** | Approval activity, Materialized Lakeview refresh, conditional retries, improved canvas, connection/item refs, Airflow identity+variables, Airflow Copilot | Preview |
| **Copy Job distribution** | CDC SQL family, full/incremental switch, JSON edits, Activator trigger, truncate-before-full, nuove destinazioni (GBQ/MySQL/PostgreSQL) | **GA** |
| **Copy Job advanced** | SCD Type 2 esteso, auto-partitioning (Oracle/SAP HANA/Lakehouse), SAP ABAP add-on | Preview |
| **Agentic DI** | Data Factory Skills (authoring/consumption/diagnostics) + Airflow support nel MCP di Data Factory | Preview |
| **ADF→Fabric migration** | Tooling di migrazione MDF e accesso dal migrate pane | Preview |

### Perché conta
Con queste release, Data Factory non è solo "ETL in Fabric": diventa un layer **pro-code + low-code + agent-ready**, con percorso esplicito di migrazione da Azure Data Factory.

---

## 📅 Calendario completo sessioni Build 2026

### Breakout (live)

| ID | Titolo | Data/Ora (PT) | Link |
|---|---|---|---|
| **BRK223** | From rows to reasoning: Designing databases for AI apps and agents | Mar 2 giu, 14:30 | https://aka.ms/Build-BRK223 |
| **BRK224** | PepsiCo's blueprint for agentic AI | Mer 3 giu, 14:45 | https://aka.ms/Build-BRK224 |
| **BRK225** | Data, apps, and agents: the future of app dev with Microsoft Fabric | Mer 3 giu, 13:30 | https://aka.ms/Build-BRK225 |

### On-demand — Microsoft Fabric

| ID | Titolo | Link |
|---|---|---|
| OD810 | Build fast, not fragile on Microsoft Fabric | https://aka.ms/OD810 |
| OD811 | Powering the next AI frontier with a unified data platform | https://aka.ms/OD811 |
| OD812 | Bringing Enterprise Ontology Directly into the Developer Workflow | https://aka.ms/OD812 |
| OD813 | Powering modern data analytics in Fabric Data Warehouse | https://aka.ms/OD813 |
| OD815 | Unify your entire data estate on a single, AI-ready data lake | https://aka.ms/OD815 |
| OD816 | Securing, scaling, and sustaining your data estate in Microsoft Fabric | https://aka.ms/OD816 |
| OD817 | Agentic analytics with Power BI and Microsoft Fabric | https://aka.ms/OD817 |
| OD818 | The AI-native data engineer | https://aka.ms/OD818 |
| OD819 | Real-Time Intelligence: Bringing event-driven AI apps & agents | https://aka.ms/OD819 |

### On-demand — Microsoft Databases

| ID | Titolo | Link |
|---|---|---|
| OD820 | Designing reliable multi-agent apps with Azure Cosmos DB | https://aka.ms/OD820 |
| OD821 | Building Azure DocumentDB on open-source foundations | https://aka.ms/OD821 |
| OD822 | Smarter PostgreSQL migrations to power modern, intelligent apps | https://aka.ms/OD822 |
| OD823 | Faster AI Responses with Semantic Caching in Azure Managed Redis | https://aka.ms/OD823 |
| OD824 | Scalable Applications Without Polyglot tax: Azure SQL Hyperscale | https://aka.ms/OD824 |

---

## 🧭 Come orientarsi tra i 4 documenti di questa cartella

| File | Scope |
|---|---|
| [rayfin.md](rayfin.md) | Tutto su Rayfin (SDK, CLI, decorator, template, GitHub) |
| [fabric_apps.md](fabric_apps.md) | Tutto su Fabric Apps (Preview): quickstart, CLI, deploy, use case |
| [web_iq.md](web_iq.md) | Tutto su Web IQ (API grounding, Harrier, evidence, GDSAT, MCP) |
| [fabric_build_2026_overview.md](fabric_build_2026_overview.md) | Indice riassuntivo di **tutte le novità Fabric** a Build 2026 |
| **[fabric_build_2026_deepdive.md](fabric_build_2026_deepdive.md) ← QUESTO** | Approfondimento punto-per-punto dell'overview |

---

*Documento aggiornato al 4 giugno 2026, basato su blog ufficiali Microsoft pubblicati il 2-4 giugno 2026 per il Build 2026 (incluso il dettaglio post-day-1 su Power BI, Fabric IQ, Business Events e Data Factory).*
