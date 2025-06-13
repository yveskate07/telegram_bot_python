
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

1. Ouvrir Telegram et rechercher **BotFather**
2. Envoyer la commande `/newbot`
3. Choisir un nom et un @username pour votre bot
![bot father chat]('C:\Users\7MAKSACOD PC\PycharmProjects\python_telegram_bot\Capture d'écran 2025-06-13 181608.png')
4. **Copier le token** donné par BotFather 
5. Créer un fichier .env et inserer la paire clé-valeur:
```
TOKEN="7687267672:AAGHEtsFh4H8WAqCfPgL67_V48UVZugZ7eg"
```

---

## 💻 3. Écrire le code du bot

Passons maintenant au codage du bot. Veuillez créer un nouveau fichier Python, par exemple : ```my_telegram_bot``` et ouvrez-le dans votre éditeur de texte préféré. Suivez ensuite ces étapes pour créer votre bot.

### Importer des bibliothèques :

Commencez par importer les modules nécessaires et configurer la journalisation pour faciliter le débogage :

```python
import logging
from telegram import (ReplyKeyboardMarkup, ReplyKeyboardRemove, Update, InlineKeyboardButton, InlineKeyboardMarkup)
from telegram.ext import (Application, CallbackQueryHandler, CommandHandler, ContextTypes, ConversationHandler, MessageHandler, filters)

logging.basicConfig(format='%(asctime)s - %(name)s - %(levelname)s - %(message)s', level=logging.INFO)
logger = logging.getLogger(__name__)
```

### Définir les états de conversation

Les états d’un bot Telegram, notamment lorsqu’un gestionnaire de conversations est utilisé, servent de cadre pour gérer le flux d’interaction entre le bot et l’utilisateur. Il s’agit essentiellement de marqueurs ou de points de contrôle qui définissent la partie de la conversation actuellement engagée par l’utilisateur et déterminent la prochaine action du bot en fonction de ses informations.

Voici un aperçu plus général du rôle et des fonctionnalités des états dans la gestion des conversations des bots. Leurs objectifs et fonctionnalités dans le bot Telegram sont les suivants :

---

#### 1. Gestion séquentielle des flux

Les états permettent au bot de gérer un flux de conversation séquentiel. En passant d’un état à un autre, le bot peut guider l’utilisateur à travers une série d’étapes, de questions ou d’options dans un ordre logique.

---

#### 2. Connaissance du contexte

Ces informations aident le robot à maintenir le contexte d’une conversation. En connaissant l’état actuel, le robot comprend les informations fournies par l’utilisateur et celles qui sont encore nécessaires, ce qui lui permet de réagir de manière appropriée.

---

#### 3. Traitement des saisies utilisateur

Selon l’état actuel, le bot peut traiter les saisies utilisateur différemment.  
Par exemple, une saisie `<CAR_TYPE>` sera interprétée comme une indication du type de voiture à vendre, tandis qu’une saisie `<CAR_COLOR>` sera interprétée comme la couleur de la voiture.

---

#### 4. Implémentation de la logique conditionnelle

Les états permettent d’implémenter une logique conditionnelle dans la conversation.  
En fonction des réponses ou des choix de l’utilisateur, le bot peut décider d’ignorer certains états, de les répéter ou d’orienter l’utilisateur vers un autre chemin de conversation.

---

#### 5. Gestion des erreurs et répétition

Ils facilitent la gestion des erreurs et la répétition des questions si l’utilisateur fournit des réponses inexactes ou invalides.  
En suivant l’état actuel, le bot peut relancer l’utilisateur pour obtenir les informations correctes.

---

#### 6. Persistance de l’état

Dans les bots plus complexes, les états peuvent être stockés et conservés d’une session à l’autre, permettant aux utilisateurs de reprendre la conversation là où ils l’avaient laissée, même s’ils quittent temporairement le chat ou si le bot redémarre.


---

Énumérons les états pour que notre bot gère le flux :

```python
TYPE_VOITURE, COULEUR_VOITURE, DÉCISION_KILOMÉTRAGE_VOITURE, KILOMÉTRAGE_VOITURE, PHOTO, RÉSUMÉ = plage ( 6 )
```

## 🔁 4. Ajouter des fonctionnalités de conversation

Les gestionnaires de conversation des bots Telegram, notamment grâce à des bibliothèques comme `python-telegram-bot`, sont des outils puissants qui gèrent le flux des conversations en fonction des saisies utilisateur et d’états prédéfinis. Ils sont essentiels au développement de bots nécessitant une séquence d’interactions, comme la collecte d’informations, le guidage des utilisateurs dans les menus ou l’exécution de commandes dans un ordre précis.

Voici un aperçu détaillé du fonctionnement des gestionnaires de conversation et de leur rôle dans le développement de bots :

---

### Objectif et fonctionnalité

#### 1. Gestion des états conversationnels

Les gestionnaires de conversation suivent l’état actuel du dialogue avec chaque utilisateur. Ils déterminent la prochaine action du bot en fonction des informations saisies par l’utilisateur et de l’état actuel, permettant une progression fluide et logique des différentes étapes de l’interaction.

---

#### 2. Routage des entrées utilisateur

Ces entrées sont acheminées vers différentes fonctions de rappel en fonction de l’état actuel. Cela signifie qu’une même entrée peut produire des résultats différents selon la position de l’utilisateur dans la conversation.

---

#### 3. Gestion des commandes et du texte

Les gestionnaires de conversation peuvent faire la différence entre les commandes (comme `/start` ou `/help`) et les messages texte classiques, permettant aux développeurs de spécifier des réponses ou des actions distinctes pour chaque type d’entrée.

---

#### 4. Intégration avec les claviers et les boutons

Ils fonctionnent parfaitement avec les claviers personnalisés et les boutons intégrés, permettant aux développeurs de créer des interfaces interactives et conviviales au sein de la conversation.  
Les utilisateurs peuvent sélectionner des options ou naviguer parmi les fonctionnalités du bot grâce à ces éléments d’interface.

---

#### 5. Fonctions de repli et d’expiration

Les gestionnaires de conversation prennent en charge les fonctions de repli, qui peuvent être déclenchées lorsque l’utilisateur entre une entrée inattendue ou lorsque la conversation doit être réinitialisée.  
Ils peuvent également gérer les expirations, mettant fin automatiquement à une conversation après une période d’inactivité.

La mise en œuvre d’un gestionnaire de conversation implique généralement la définition de **points d’entrée**, d’**états** et de **solutions de secours** :

- **Points d’entrée** :  
  Ce sont des déclencheurs qui lancent la conversation.  
  Généralement, la commande `/start` est utilisée comme point d’entrée, mais vous pouvez définir plusieurs points d’entrée pour différents flux de conversation.

- **États** :  
  Comme indiqué précédemment, les états représentent différents points de la conversation.  
  Chaque état est associé à une ou plusieurs fonctions de rappel qui définissent le comportement du bot à ce stade.  
  Les développeurs associent les états à ces fonctions de rappel, dictant ainsi le déroulement de la conversation.

- **Fonctions de secours** :  
  Les fonctions de secours sont définies pour gérer les situations imprévues ou pour permettre de quitter ou de réinitialiser la conversation.  
  Une fonction de secours courante est une commande `/cancel` permettant aux utilisateurs d’interrompre la conversation à tout moment.

Ensuite, la fonction ```start``` de gestionnaire initie la conversation (point d'entrée), présentant à l'utilisateur une sélection de types de voitures :


```python
def  start ( update: Update, context: ContextTypes.DEFAULT_TYPE ) -> int : 
    """Démarre la conversation et demande à l'utilisateur quel est son type de voiture préféré."""
     reply_keyboard = [[ 'Berline' , 'SUV' , 'Sports' , 'Électrique' ]] 

    await update.message.reply_text( 
        '<b>Bienvenue dans le bot de vente de voitures !\n' 
        'Obtenons quelques détails sur la voiture que vous vendez.\n' 
        'Quel est votre type de voiture ?</b>' , 
        parse_mode= 'HTML' , 
        reply_markup=ReplyKeyboardMarkup(reply_keyboard, one_time_keyboard= True , resize_keyboard= True ), 
    ) 

    return CAR_TYPE
```

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
