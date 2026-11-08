# Add-a-README

Ce dépôt inclut les [agent-skills](https://github.com/addyosmani/agent-skills) d'Addy Osmani (licence MIT, voir `.claude/AGENT-SKILLS-LICENSE`), copiés dans `.claude/` : ils sont actifs automatiquement dans Claude Code, sans installation.

Commandes : /spec, /plan, /build, /test, /review, /webperf, /code-simplify, /constraints, /ship

## OmniRoute

[OmniRoute](https://github.com/cheaper-inference/OmniRoute) (licence MIT) est une passerelle IA locale : un seul endpoint (`http://localhost:20128`) qui route vers des centaines de fournisseurs, avec bascule automatique. Il s'intègre à Claude Code de deux façons.

### 1. Installer et démarrer OmniRoute

```bash
npm install -g omniroute
omniroute            # tableau de bord + API sur http://localhost:20128
```

Dans le tableau de bord, connectez au moins un fournisseur et créez une clé API. Pour le serveur MCP, activez-le dans **Settings** avec le transport `streamable-http`.

### 2. Serveur MCP (déjà configuré dans ce dépôt)

Le fichier `.mcp.json` déclare le serveur MCP `omniroute` (`http://localhost:20128/api/mcp/stream`). Au premier lancement de Claude Code dans ce dépôt, approuvez-le, puis vérifiez avec `/mcp`.

- Pour une instance distante, définissez `OMNIROUTE_MCP_URL` (ex. `https://mon-vps:20128/api/mcp/stream`). Hors loopback, OmniRoute exige une clé avec le scope `manage` ou `mcp:connect` : ajoutez-la avec `claude mcp add --transport http --scope local omniroute <url> --header "Authorization: Bearer <clé>"` (la clé reste alors hors du dépôt).

### 3. (Optionnel) Utiliser OmniRoute comme backend des modèles

Pour faire passer les requêtes de Claude Code par OmniRoute, lancez :

```bash
omniroute launch                 # lance claude avec ANTHROPIC_BASE_URL / ANTHROPIC_AUTH_TOKEN injectés
```

Ou, de façon permanente, dans `.claude/settings.local.json` (non versionné, ne commitez jamais la clé) :

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "http://localhost:20128",
    "ANTHROPIC_AUTH_TOKEN": "<votre clé API OmniRoute>",
    "CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY": "1"
  }
}
```

`ANTHROPIC_BASE_URL` ne prend pas de suffixe `/v1`. Redémarrez Claude Code après modification.
