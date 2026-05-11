# ⚡ DÉMARRAGE RAPIDE (5 minutes)

## 🎯 Résultat final
Chaque **lundi à 8h**, un rapport Word avec les nouvelles sorties musicales francophones arrive automatiquement dans ton repo.

---

## ✅ ÉTAPES

### 1️⃣ Obtenir ta clé API Claude (2 min)

```
1. Va sur https://console.anthropic.com/
2. Login avec ton compte Anthropic (create un si besoin)
3. Clique "API Keys" → "Create Key"
4. Copie la clé (commence par "sk-ant-")
5. Garde-la de côté ⚠️ NE LA PARTAGE PAS!
```

### 2️⃣ Fork ce repo sur GitHub (1 min)

```
1. Clique "Fork" en haut à droite
2. Create fork (appuie sur le bouton)
3. Attends quelques secondes
4. Tu es sur https://github.com/TON_USERNAME/music-veille-agent
```

### 3️⃣ Ajouter ton API key en secret GitHub (1 min)

```
Sur GitHub (ton fork):
1. Settings (onglet en haut à droite)
2. Secrets and variables → Actions
3. New repository secret
4. 
   Nom: ANTHROPIC_API_KEY
   Valeur: sk-ant-xxxxxxxxxxxx (ta clé)
   
5. Add secret
```

### 4️⃣ Activer le workflow automatique (1 min)

```
1. Clique "Actions" (onglet en haut)
2. Tu vois "Weekly French Music Veille Report"
3. Clique dessus
4. Clique "Enable workflow"
5. C'est tout! 🎉
```

### 5️⃣ Tester maintenant (optionnel)

```
1. Actions → "Weekly French Music Veille Report"
2. "Run workflow" (dropdown en haut à droite)
3. Attends 30-60 secondes
4. Regarde les logs verts ✅
5. Reviens dans "Code" → dossier "reports"
6. Télécharge le .docx généré
```

---

## 🎵 Et après?

**Automatiquement chaque lundi à 8h du matin:**
- L'agent Claude tourne
- Cherche les sorties musicales
- Génère un rapport Word
- Le commit dans le repo
- Tu reçois une notif GitHub

**Pour récupérer les rapports:**
- Code → dossier "reports"
- Télécharge le fichier le plus récent

---

## 🛠️ Si tu veux customizer

### Changer l'heure
```
.github/workflows/music-veille.yml
Ligne: cron: '0 7 * * 1'
Change le '7' par l'heure que tu veux (UTC)
```

### Ajouter d'autres genres
```
generate_music_report.py
Fonction: _get_system_prompt()
Ajoute tes genres dans "Domaines à couvrir"
```

### Ajouter d'autres pays
```
Même fichier, même fonction
Change "musique française" par ce que tu veux
```

---

## ✨ Voilà!

Tu as maintenant un **agent musical automatisé** 🎵

Chaque lundi = nouvelle veille = zéro effort de ta part

**Questions?** Voir le README.md pour plus de détails.

**Bon écoute!** 🎧
