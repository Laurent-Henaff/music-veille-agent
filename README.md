# 🎵 Music Veille Agent
**Agent IA pour suivre les nouveautés musicales françaises chaque semaine**

Un agent Claude qui génère automatiquement un rapport complet sur les sorties musicales francophones **tous les lundis à 8h du matin**, directement dans ton GitHub.

---

## ✨ Fonctionnalités

✅ **Veille automatique** - Chaque lundi, généré automatiquement  
✅ **Multi-turn agent** - Claude discute avec lui-même, analyse en profondeur  
✅ **Rapport Word professionnel** - Téléchargeable, shareable  
✅ **Historique complet** - Tous les rapports conservés dans le repo  
✅ **Gratuit** - GitHub Actions inclus, Claude API économe (~$1-2/mois)  
✅ **Customizable** - Modifie le prompt pour tes genres préférés

---

## 📋 TABLE DES MATIÈRES

1. [Installation rapide (5 min)](#installation-rapide)
2. [Configuration API Claude](#configuration-api)
3. [Utilisation](#utilisation)
4. [Personnalisation](#personnalisation)
5. [Troubleshooting](#troubleshooting)

---

## 🚀 Installation Rapide

### Étape 1: Fork ou clone ce repo

**Option A - Fork (recommandé pour autom. complète)**
```bash
# Sur GitHub.com:
# 1. Clique "Fork" en haut à droite
# 2. Tu vas sur ton fork: https://github.com/TON_USERNAME/music-veille-agent
```

**Option B - Clone local**
```bash
git clone https://github.com/TON_USERNAME/music-veille-agent.git
cd music-veille-agent
```

### Étape 2: Configurer la clé API Claude

#### Sur GitHub Web (idéal pour automation):

1. Va dans **Settings** → **Secrets and variables** → **Actions**
2. Clique **New repository secret**
3. Nom: `ANTHROPIC_API_KEY`
4. Valeur: Ton API key Claude de https://console.anthropic.com/

```
Pour obtenir ta clé:
1. Va sur https://console.anthropic.com/
2. Create API Key
3. Copie-la entièrement (commence par "sk-ant-")
4. Colle-la dans GitHub Secrets
```

#### En local (pour tester):

```bash
# Créer un fichier .env
echo "ANTHROPIC_API_KEY=sk-ant-..." > .env

# Installer python-dotenv
pip install python-dotenv

# Le script chargera automatiquement
```

### Étape 3: Teste le setup

```bash
# Installe les dépendances
pip install -r requirements.txt

# Lance le script manuellement pour tester
python generate_music_report.py

# ✅ Un fichier doit apparaître dans ./reports/veille_YYYY-MM-DD.docx
```

### Étape 4: Active l'automation

1. Va dans **Actions** sur GitHub
2. Clique sur **Weekly French Music Veille Report** (workflow)
3. Clique **Enable workflow**
4. C'est fait! 🎉

---

## ⚙️ Configuration API

### Authentification Claude API

```bash
# Option 1: Variable d'environnement (recommandé en production)
export ANTHROPIC_API_KEY="sk-ant-..."

# Option 2: Fichier .env (développement local)
# Crée un fichier .env à la racine:
ANTHROPIC_API_KEY=sk-ant-...

# Option 3: GitHub Secrets (automation)
# Voir "Installation Rapide" Étape 2
```

### Pricing

Claude API est **très bon marché**:

- **1 rapport/semaine** = ~0.50 tokens
- **Claude Opus** = $15/MTok input, $75/MTok output
- **Coût mensuel estimé** = $0.50 - $2

👉 Bien moins cher que n'importe quel service de streaming!

---

## 📖 Utilisation

### Automatique (la vraie magie)

Aucune action nécessaire. **Chaque lundi à 8h UTC** (9h Paris en hiver):

```
🤖 GitHub Actions s'active
   ↓
📡 Appelle Claude
   ↓
🎵 Génère la veille
   ↓
📤 Commit le rapport
   ↓
📧 Notification GitHub
```

**Pour voir le rapport:**
1. Va dans **Reports** folder du repo
2. Télécharge le `.docx` le plus récent
3. Ouvre-le dans Word / Google Docs

### Manuel (si tu veux tester/relancer)

```bash
# Génère un rapport tout de suite
python generate_music_report.py

# Ou sur GitHub:
# Actions → "Weekly French Music Veille Report" → "Run workflow"
```

### Recevoir les rapports par email

**Option 1: GitHub Notifications**
- Settings → Notifications → Watch
- Tu reçois une notification quand le commit est poussé

**Option 2: Auto-email (avancé)**
Ajoute une action GitHub pour email:

```yaml
- name: 📧 Send email
  uses: dawidd6/action-send-mail@v3
  with:
    server_address: ${{ secrets.MAIL_SERVER }}
    server_port: 465
    username: ${{ secrets.MAIL_USERNAME }}
    password: ${{ secrets.MAIL_PASSWORD }}
    subject: "🎵 Music Veille - $(date +%Y-%m-%d)"
    to: ton-email@example.com
    from: music-agent@github.com
    body: "Ton rapport music veille est prêt! Voir: https://github.com/..."
```

---

## 🎨 Personnalisation

### Modifier les genres musicaux

Ouvre `generate_music_report.py` et modifie `_get_system_prompt()`:

```python
def _get_system_prompt(self) -> str:
    return """Tu es un expert en musique française 2026...
    
    Domaines à couvrir:
    - Jazz français
    - Électro/Techno Paris
    - Reggae francophone
    - Metal français
    """
```

### Changer l'heure de lancement

Ouvre `.github/workflows/music-veille.yml`:

```yaml
on:
  schedule:
    - cron: '0 7 * * 1'  # Lundi 7h UTC
    # Change à ton heure préférée:
    # '30 7 * * 1'      = Lundi 7h30 UTC
    # '0 20 * * 0'      = Dimanche 20h UTC (veille lundi matin)
    # '0 8 * * 2'       = Mardi 8h UTC
```

[Générateur Cron](https://crontab.guru/) pour les heures custom

### Modifier la structure du rapport

Ouvre `step_4_export_word()` dans `generate_music_report.py`:

```python
def step_4_export_word(self):
    doc = Document()
    
    # Ajoute tes propres sections:
    doc.add_heading("🔥 LES HITS", level=1)
    doc.add_paragraph("...")
    
    doc.add_heading("👶 JEUNES TALENTS", level=1)
    doc.add_paragraph("...")
```

---

## 📊 Résultats Attendus

**Exemple de rapport généré:**

```
🎵 VEILLE MUSIQUE FRANÇAISE
Semaine du 13/05/2026

📻 SORTIES & ACTUALITÉS
- Stromae annonce nouvel album (prod. SOPHIE)
- Charlotte Cardin feat. Aya Nakamura - single surprise
- Polo & Pan nouveau livestream YouTube
- Festival Rock en Seine 2026: lineup dévoilée
...

🎯 ANALYSE DE LA SEMAINE
Thème: Renaissance de la synthwave parisienne
Artiste à suivre: [nom émergent]
Top 3 personnel: [classement avec justifs]

🎧 À ÉCOUTER
Playlist: 
1. [Chanson] - Artiste (2026)
2. ...
```

---

## 🐛 Troubleshooting

### Le workflow ne s'exécute pas

**Vérifications:**
1. ✅ Secrets GitHub configurés? (Settings → Secrets → ANTHROPIC_API_KEY)
2. ✅ Workflow activé? (Actions → Enable workflows)
3. ✅ Permissions écrites? (Settings → Actions → Read and write permissions)

**Solution:**
```bash
# Regarde les logs détaillés:
# GitHub → Actions → [Workflow] → [Run] → Logs
```

### Erreur API: "Unauthorized"

```
❌ Error: 401 Unauthorized
```

→ Ta clé API est invalide ou expirée

**Fix:**
1. Va sur https://console.anthropic.com/
2. Régénère ta clé
3. Mets à jour le secret GitHub

### Erreur: "quota exceeded"

→ Tu as dépassé ta limite Claude API

**Fix:**
1. Va sur https://console.anthropic.com/ → Billing
2. Augmente ton quota
3. C'est très bon marché (~$0.50-2/mois pour 1 veille/semaine)

### Le document Word est vide

→ Claude n'a pas généré de contenu

**Debug:**
```bash
# Lance en local pour voir les logs complets
python generate_music_report.py

# Vérife que ton API key fonctionne:
python -c "from anthropic import Anthropic; print('✅ OK')"
```

---

## 🚀 Améliorations Futures

**À ajouter:**

- [ ] Notifications Slack/Discord chaque semaine
- [ ] Dashboard HTML des sorties
- [ ] Tracking d'artistes favoris
- [ ] Export Spotify playlist auto
- [ ] Multi-langue (EN, ES, IT)
- [ ] Analytics: trends par genre

**Contributions bienvenues!** 🤝

---

## 📄 Licence

MIT - Tu peux faire ce que tu veux avec ce code

---

## 🆘 Support

### Besoin d'aide?

1. Vérifie le [Troubleshooting](#troubleshooting)
2. Regarde les logs GitHub: Actions → [Workflow] → [Run]
3. Teste en local: `python generate_music_report.py`

### Contact

- 📧 Issues sur GitHub
- 🐦 Twitter/X
- 💬 GitHub Discussions

---

## 🎵 Bon écoute! 

Profite de ta veille automatisée et découvre les pépites musicales chaque semaine! 🎸✨

**P.S.** - Si tu découvres des artistes cool via cet agent, share-les sur les réseaux avec `#MusicVeilleAgent` !
