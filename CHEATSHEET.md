# 🎵 Music Veille Agent - CHEAT SHEET

## ⚡ SETUP EN 5 MIN

```bash
# 1. Obtenir clé API
👉 https://console.anthropic.com/ → Create API Key

# 2. Fork repo
👉 https://github.com/yourusername/music-veille-agent

# 3. Ajouter secret GitHub
👉 Settings → Secrets → ANTHROPIC_API_KEY = sk-ant-...

# 4. Activer workflow
👉 Actions → Enable

# 5. Done! Chaque lundi à 8h ✨
```

---

## 📁 FICHIERS CLÉS

| Fichier | Rôle |
|---------|------|
| `.github/workflows/music-veille.yml` | ⏰ Scheduler + launcher |
| `generate_music_report.py` | 🤖 Agent IA principal |
| `requirements.txt` | 📦 Dépendances Python |
| `reports/` | 📄 Rapports générés |

---

## 🎯 AGENT LOOP

```
1️⃣ RECHERCHE
   → Sorties musicales + artistes + news
   
2️⃣ ANALYSE
   → Thèmes, top 3, tendances
   
3️⃣ RECS
   → Playlist + émergent + podcast
   
4️⃣ EXPORT
   → Document Word professionnel
```

---

## ⚙️ PERSONNALISATION

### Changer l'heure
```yaml
# .github/workflows/music-veille.yml
cron: '0 7 * * 1'  # UTC
# 7 = hour, 1 = Monday
# https://crontab.guru pour aide
```

### Modifier les genres
```python
# generate_music_report.py
def _get_system_prompt(self):
    return """...
    Domaines: Variété, Pop, Jazz, etc.
    """
```

### Ajouter une étape (Step 5+)
```python
def step_5_slack_notification(self):
    # Ton code ici
    pass

# Appelle dans run():
self.step_5_slack_notification()
```

---

## 🔧 DEBUGGING

```bash
# Tester en local
python generate_music_report.py

# Voir logs GitHub
👉 Actions → [Run] → Logs

# Vérifier API key
export ANTHROPIC_API_KEY="sk-ant-..."
python -c "from anthropic import Anthropic; print('✅')"

# Voir rapport généré
ls reports/
```

---

## 💰 COÛTS

| Item | Coût |
|------|------|
| GitHub | Gratuit |
| GitHub Actions | Gratuit (inclus) |
| Claude API/run | ~$0.14 |
| Par mois (4 runs) | ~$0.60 |
| Par an | ~$7 |

✅ **Super bon marché!**

---

## 📞 HELP

### Workflow ne s'exécute pas
- ✅ Secret ANTHROPIC_API_KEY configuré?
- ✅ Workflow activé (Actions)?
- ✅ Permissions GitHub OK?

### API Error "Unauthorized"
- ✅ Clé API valide?
- ✅ Pas expirée?
- ✅ Correctement copiée (sans espaces)?

### Rapport vide
- ✅ Tester en local: `python generate_music_report.py`
- ✅ Vérifier logs: Actions → [Run] → Logs

---

## 📚 DOCS COMPLÈTES

- `README.md` - Guide complet
- `QUICKSTART.md` - Démarrage rapide
- `ARCHITECTURE.md` - Diagrammes + détails techniques

---

## 🚀 NEXT STEPS

1. ✅ Setup (5 min)
2. ✅ Activer (1 min)
3. ✅ Tester (2 min)
4. 🎉 Profiter!

Chaque lundi = nouvelle veille musicale

**Bon écoute!** 🎧✨
