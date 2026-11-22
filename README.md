# VictoryStats

Application web communautaire pour enregistrer et consulter des statistiques Fortnite. Interface française, thème sombre rouge/noir, comptes joueurs, historique des parties et classement filtrable par région.

> **Important :** les statistiques sont saisies manuellement par les joueurs. Le projet n'utilise pas d'API Fortnite officielle, ne récupère pas les données Epic Games et n'est pas affilié à Epic Games.

## Fonctionnalités

- Inscription et connexion avec mot de passe haché (PBKDF2-HMAC-SHA256) et jeton JWT.
- Profil joueur : pseudo, plateforme et région.
- Enregistrement de parties Build, Zero Build et Reload.
- Statistiques : parties, éliminations, victoires, win rate, place moyenne et éliminations par partie.
- Historique des 10 dernières parties.
- Classement communautaire trié par victoires puis éliminations, filtrable par région.
- API documentée automatiquement via Swagger.
- Tests automatisés de base.

## Installation

Prérequis : Python 3.11 ou plus récent.

### Windows (PowerShell)

```powershell
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
Copy-Item .env.example .env
```

Ouvre `.env` et remplace `SECRET_KEY` par une clé aléatoire. Tu peux en générer une avec :

```powershell
python -c "import secrets; print(secrets.token_urlsafe(48))"
```

Puis démarre le serveur :

```powershell
uvicorn app.main:app --reload
```

Ouvre ensuite **http://127.0.0.1:8000**. La documentation de l'API est disponible sur **http://127.0.0.1:8000/docs**.

### Linux / macOS

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
python -c "import secrets; print(secrets.token_urlsafe(48))"
# Colle la clé générée dans .env, puis :
uvicorn app.main:app --reload
```

## Tests

```bash
pytest -v
```

## Structure

```text
VictoryStats/
├── app/
│   ├── main.py          # Routes FastAPI
│   ├── config.py        # Configuration
│   ├── database.py      # Connexion SQLAlchemy
│   ├── models.py        # Tables SQL
│   ├── schemas.py       # Validation Pydantic
│   ├── security.py      # Hachage et JWT
│   ├── services.py      # Calculs et classement
│   └── static/          # Interface HTML/CSS/JS
├── tests/               # Tests API
├── .env.example
├── requirements.txt
└── README.md
```

## Endpoints principaux

| Méthode | Route | Description |
|---|---|---|
| GET | `/api/health` | Vérifier que l'API fonctionne |
| POST | `/api/auth/register` | Créer un compte |
| POST | `/api/auth/login` | Se connecter |
| GET | `/api/me` | Profil et statistiques personnelles |
| PATCH | `/api/players/me` | Modifier son profil |
| POST | `/api/stats` | Ajouter une partie |
| GET | `/api/stats/recent` | 10 dernières parties |
| GET | `/api/leaderboard` | Classement (`?region=EU`) |

## Avant une mise en ligne publique

Cette version est une base fonctionnelle pour un projet personnel ou un prototype. Avant d'ouvrir le service au public :

- Utiliser PostgreSQL et des migrations Alembic plutôt que `create_all`.
- Définir une clé secrète forte dans un gestionnaire de secrets.
- Servir le site en HTTPS, limiter les tentatives de connexion et ajouter du rate limiting.
- Ajouter vérification d'e-mail, réinitialisation de mot de passe et suppression de compte.
- Définir une politique de confidentialité, des sauvegardes et des règles de modération.
- Ajouter des protections anti-triche pour les statistiques déclarées par les joueurs.
- Ne jamais demander ni stocker les identifiants Epic Games des joueurs.
