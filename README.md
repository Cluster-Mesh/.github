# Cluster Mesh

**Cluster Mesh** è una company del gruppo [Reply](https://www.reply.com), specializzata sull'intero stack Microsoft: Azure, Microsoft 365, Fabric, Power Platform e AI.

Ci occupiamo di esplorare, documentare e costruire attorno alle tecnologie cloud-native, data platform e AI — con un approccio pratico e orientato alla produzione.

---

## 📂 Documentazione

| Documento | Descrizione |
|-----------|-------------|
| [Microsoft Fabric @ Build 2026 — Deep Dive](docs/fabric_build_2026_deepdive.md) | Approfondimento tecnico in italiano su ogni annuncio Fabric presentato a Microsoft Build 2026: Microsoft IQ, Rayfin, Fabric IQ GA, Operations Agents e altro. |

---

## 🤖 GitHub Copilot — Marketplace interno

Per ottenere il massimo da GitHub Copilot all'interno dell'organizzazione, manteniamo un **marketplace privato di agent plugin** con skill e agent personalizzati:

👉 [**.clumit-gh-copilot-plugins**](https://github.com/Cluster-Mesh/.clumit-gh-copilot-plugins)

### Installazione in VS Code

1. Assicurati di avere accesso all'organizzazione `Cluster-Mesh` su GitHub e di essere autenticato (`gh auth login` oppure credential helper di Git configurato).
2. Apri VS Code → `Cmd+Shift+P` → **GitHub Copilot: Manage Agent Plugin Sources** (oppure *Add Agent Plugin Source*).
3. Incolla l'URL del repository:
   ```
   https://github.com/Cluster-Mesh/.clumit-gh-copilot-plugins.git
   ```
4. Conferma. VS Code clona il repo in `~/.vscode/agent-plugins/` e rende disponibili gli agent e le skill definiti in `plugins/`.
5. Ricarica la finestra (`Cmd+Shift+P` → *Developer: Reload Window*) per attivarli.

> Per aggiornare i plugin dopo il primo clone vedi la sezione [Aggiornare i plugin](https://github.com/Cluster-Mesh/.clumit-gh-copilot-plugins#aggiornare-i-plugin).
