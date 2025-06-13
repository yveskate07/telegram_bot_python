
# 📦 Créer un Bot Telegram avec Python – Documentation Complète

Basée sur l’article de Moraneus : [Building Telegram Bot with Python](https://medium.com/@moraneus/building-telegram-bot-with-python-telegram-bot-a-comprehensive-guide-7e33f014dc79)

---

## 🌱 1. Préparer l’environnement

- **Créer un environnement virtuel**  
  ```bash
  python3 -m venv venv
  source venv/bin/activate
  ```

- **Installer les dépendances**  
  ```bash
  pip install -r requirements.txt
  ```

---

## 🤖 2. Créer votre bot Telegram

1. Ouvrir Telegram et parler à **BotFather**
2. Envoyer la commande `/newbot`
3. Choisir un nom et un @username pour votre bot
4. **Copier le token** donné par BotFather (gardez-le secret !)

---

## 💻 3. Écrire le code du bot

Créer un fichier `bot.py` :

```python
from telegram.ext import Application, CommandHandler, MessageHandler, filters

async def start(update, context):
    await update.message.reply_text("Bienvenue !")

async def echo(update, context):
    await update.message.reply_text(update.message.text)

def main():
    token = "VOTRE_TOKEN_ICI"
    app = Application.builder().token(token).build()
    app.add_handler(CommandHandler("start", start))
    app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, echo))
    app.run_polling()

if __name__ == "__main__":
    main()
```

---

## 🔁 4. Ajouter des fonctionnalités de conversation

Pour une interaction plus poussée avec l’utilisateur :

- Définir des **états** (ex. `CAR_TYPE`, `CAR_COLOR`, …)
- Utiliser `ConversationHandler` pour guider l'utilisateur étape par étape

---

## ▶️ 5. Lancer le bot

```bash
source venv/bin/activate
python bot.py
```

- Le bot sera actif
- Vous pouvez tester `/start` et envoyer du texte depuis Telegram

---

## 🎯 6. Tester et itérer

- Ajouter plus de **CommandHandler** pour d’autres commandes
- Utiliser **MessageHandler** pour gérer d'autres types de messages (photos, documents, etc.)

---

## 🚀 7. Aller plus loin

- Intégration de claviers inline
- Connexion à des APIs externes
- Hébergement sur un serveur avec webhook
- Gestion des erreurs, logs, base de données

---

## 📝 Résumé des étapes

| Étape                 | Description |
|----------------------|-------------|
| 1. Environnement      | Création d’un venv + `pip install` |
| 2. Bot Telegram       | Création via BotFather |
| 3. Code initial       | CommandHandler, MessageHandler |
| 4. ConversationHandler| Gestion d’états |
| 5. Exécution          | `python bot.py` |
| 6. Extensions         | Plus de fonctionnalités |
| 7. Déploiement        | Webhook, serveur, persistance |

---

Bonne création de bot ! 🚀
