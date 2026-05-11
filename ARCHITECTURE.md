# 🎵 Architecture du Music Veille Agent

## FLUX HEBDOMADAIRE

```
┌─────────────────────────────────────────────────────────────┐
│                  LUNDI 8h UTC                               │
│              (GitHub Actions déclenche)                      │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│              1️⃣ RECHERCHE (Agent Step 1)                    │
│  Claude cherche les sorties musicales francophones          │
│  - Spotify/YouTube/Pitchfork                                │
│  - Artistes confirmés + émergents                           │
│  - News industrie                                            │
│  Output: ~800 mots de recherche                             │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│           2️⃣ ANALYSE INTELLIGENTE (Agent Step 2)            │
│  Claude réfléchit sur sa propre recherche:                  │
│  - Quel thème dominant?                                      │
│  - Artiste se démarquant?                                    │
│  - Tendances observées?                                      │
│  - Top 3 personnel + justifs                                 │
│  Output: ~500 mots d'analyse                                │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│         3️⃣ RECOMMANDATIONS (Agent Step 3)                   │
│  Claude suggère:                                             │
│  - Playlist 8-10 chansons                                    │
│  - Artiste émergent à suivre                                │
│  - Podcast/interview musique                                │
│  Output: ~300 mots de recs                                  │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│           4️⃣ EXPORT WORD (Agent Step 4)                     │
│  Crée un document professionnel:                             │
│  - Header formaté                                            │
│  - 3 sections (Sorties, Analyse, Recs)                      │
│  - Footer avec date/source                                   │
│  Output: veille_YYYY-MM-DD.docx                             │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│              5️⃣ COMMIT & NOTIFICATION                       │
│  GitHub Actions:                                             │
│  - Commit le fichier .docx                                   │
│  - Push dans le repo                                         │
│  - Notifie (optionnel: email)                               │
└─────────────────────────────────────────────────────────────┘
                          ↓
                    ✅ DONE!
            Rapport prêt à télécharger
```

---

## ARCHITECTURE TECHNIQUE

```
GitHub Repository
│
├── .github/workflows/
│   └── music-veille.yml
│       ├── Trigger: Cron (lundi 7h UTC)
│       ├── Python 3.11 setup
│       ├── Dependencies install
│       └── Run: python generate_music_report.py
│
├── generate_music_report.py
│   ├── MusicVeilleAgent class
│   │   ├── __init__() : Anthropic client setup
│   │   ├── step_1_research() : Web research prompt
│   │   ├── step_2_analysis() : Self-reflection
│   │   ├── step_3_recommendations() : Curated picks
│   │   ├── step_4_export_word() : Document generation
│   │   └── run() : Main loop
│   │
│   └── Calls Anthropic Claude API
│       ├── Model: claude-opus-4-6
│       ├── Tokens: ~2500 per run
│       ├── Cost: ~$0.50-2/month
│       └── Multi-turn conversation (memory)
│
├── reports/
│   ├── veille_2026-05-13.docx  ← Lundi
│   ├── veille_2026-05-20.docx  ← Lundi suivant
│   └── veille_2026-05-27.docx  ← etc...
│
├── requirements.txt
├── README.md
├── QUICKSTART.md
└── .gitignore
```

---

## MULTI-TURN CONVERSATION (Multi-tour)

```
┌─────────────────────────────────────────────────────────────┐
│         CONVERSATION CLAUDE (Memory across steps)            │
└─────────────────────────────────────────────────────────────┘

Turn 1 - USER:
  "Fais une veille sur les sorties musicales francophones 
   cette semaine. Focus: variété, pop, chanson française..."

Turn 1 - CLAUDE:
  [Génère ~800 mots sur les sorties]

Turn 2 - USER:
  "Basé sur ta recherche, quel est le thème dominant? 
   Top 3 personnel?"

Turn 2 - CLAUDE:
  [Répond avec contexte de Turn 1, sans répéter]
  [Memory: "Je viens de dire que Stromae sort un album"]

Turn 3 - USER:
  "Suggère une playlist + artiste émergent"

Turn 3 - CLAUDE:
  [Utilise les infos de Turn 1&2]
  [Crée une playlist cohérente]

🎯 RÉSULTAT: Agent cohérent qui se souvient & construit
```

---

## COÛTS & PERFORMANCE

### ⏱️ Temps d'exécution
```
Recherche (Step 1)      : 15-20s (Claude API + token gen)
Analyse (Step 2)        : 10-15s (plus rapide, moins tokens)
Recs (Step 3)           : 8-10s
Export (Step 4)         : 2-3s (local, très rapide)
─────────────────────────────────────
TOTAL                   : ~40-50 secondes
```

### 💰 Coûts

```
Tokens par run (moyenne):
- Input:  ~2000 tokens (ma conversation + prompt sys)
- Output: ~1500 tokens (réponse Claude)
- Total:  ~3500 tokens

Prix Claude Opus:
- Input:  $15 / 1M tokens = $0.03 par run
- Output: $75 / 1M tokens = $0.11 par run
- Total:  ~$0.14 par run

Par semaine (1 run):     $0.14
Par mois (4.33 runs):    $0.60
Par année (52 runs):     $7.28

✅ SUPER ÉCONOMIQUE!
```

---

## EXTENSIBILITÉ

### Ajouts faciles

**Envoyer par email:**
```python
# Ajoute à step_4_export_word():
import smtplib
msg = EmailMessage()
msg.send_to = "toi@example.com"
msg.attach(filename)
# Send!
```

**Slack notification:**
```python
# Après export:
import requests
requests.post(
    "https://hooks.slack.com/...",
    json={"text": f"📻 Rapport généré: {filename}"}
)
```

**Dashboard HTML:**
```python
# Step 5 (nouveau):
# Crée un index.html avec tous les rapports
# Push sur GitHub Pages
# Vois les 52 derniers rapports online
```

**Analytics:**
```python
# Tracking:
# - Artistes mentionnés par semaine
# - Genres dominants
# - Évolution tendances
```

---

## DEBUGGING

### Logs disponibles

```
GitHub Actions → Logs complets:
- Minute par minute de l'exécution
- Tous les appels API
- Erreurs détaillées
- Temps d'exécution

Local (python generate_music_report.py):
- Print statements détaillés
- Traceback complet si erreur
- Fichier .docx généré
```

---

## TL;DR - La magie en 30 secondes

```
1. Tu fork le repo
2. Tu ajoutes ton API key comme secret GitHub
3. Tu actives le workflow
4. BOOM! 🎉 Chaque lundi à 8h:
   - Agent Claude tourne
   - Cherche musique française
   - Génère rapport Word
   - L'ajoute au repo
   - Tu reçois une notif
5. Tu télécharges chaque semaine
```

**Temps de setup:** 5 minutes  
**Effort après:** 0 minute/semaine  
**Coût:** ~$0.50-2/mois  
**Valeur:** ∞ (tu es toujours à jour sur la musique française!)
