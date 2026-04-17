# 🗺️ Roadmap — Portfolio RAG Chatbot avec Web Scraping

> Projet : système de scraping + ingestion de documents + chatbot RAG, déployé en Docker.
> Ordre : priorité décroissante (commence par le haut). Les blocs se construisent les uns sur les autres.
> 
> 📖 = doc officielle / texte  |  🎬 = vidéo  |  🧪 = exercices interactifs

---

## 🐍 Bloc 1 — Python fondations

> **Ressources principales (couvrent tout le bloc) :**
> - 📖 [docs.python.org — Le tutoriel officiel](https://docs.python.org/fr/3/tutorial/index.html) — en français, exhaustif, CTRL+F friendly
> - 🎬 [Corey Schafer — Python Tutorials](https://www.youtube.com/playlist?list=PL-osiE80TeTt2d9bfVyTiXJA-UTHn6WwU) — la référence YouTube, claire et progressive
> - 🧪 [exercism.org — Python track](https://exercism.org/tracks/python) — exercices corrigés avec feedback humain

- [ ] Environnements virtuels (`venv`, `pip`, `requirements.txt`)
  - 📖 [docs.python.org — venv](https://docs.python.org/fr/3/library/venv.html)
  - 🎬 [Corey Schafer — Virtual Environments](https://www.youtube.com/watch?v=APOPm01BVrk)

- [ ] Typage Python (`type hints`, `Optional`, `List`, `Dict`)
  - 📖 [mypy — cheatsheet type hints](https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html)
  - 🎬 [ArjanCodes — Type Hints (10 min)](https://www.youtube.com/watch?v=dgBCEB2jVU0)

- [ ] Gestion des exceptions (`try/except/finally`, exceptions custom)
  - 📖 [docs.python.org — Erreurs et exceptions](https://docs.python.org/fr/3/tutorial/errors.html)
  - 🎬 [Corey Schafer — Exceptions](https://www.youtube.com/watch?v=NIWwJbo-9_8)

- [ ] Lecture/écriture de fichiers (`open`, `pathlib`)
  - 📖 [docs.python.org — pathlib](https://docs.python.org/fr/3/library/pathlib.html)
  - 🎬 [Corey Schafer — File Objects](https://www.youtube.com/watch?v=Uh2ebFW8OYM)

- [ ] Manipulation de données avec `dataclasses` ou `pydantic`
  - 📖 [docs.python.org — dataclasses](https://docs.python.org/fr/3/library/dataclasses.html)
  - 📖 [docs.pydantic.dev — Getting started](https://docs.pydantic.dev/latest/concepts/models/)
  - 🎬 [ArjanCodes — Pydantic (15 min)](https://www.youtube.com/watch?v=502XOB0u8OY)

- [ ] Comprehensions (list, dict, generator)
  - 📖 [docs.python.org — Compréhensions de listes](https://docs.python.org/fr/3/tutorial/datastructures.html#list-comprehensions)
  - 🎬 [Corey Schafer — Comprehensions](https://www.youtube.com/watch?v=3dt4OGnU5sM)

- [ ] Manipulation de JSON (`json.loads`, `json.dumps`)
  - 📖 [docs.python.org — json](https://docs.python.org/fr/3/library/json.html)
  - 🎬 [Corey Schafer — JSON](https://www.youtube.com/watch?v=9N6a-VLBa2I)

- [ ] Variables d'environnement (`os.environ`, `python-dotenv`)
  - 📖 [python-dotenv — README](https://github.com/theskumar/python-dotenv)
  - 🎬 [Tech With Tim — python-dotenv (8 min)](https://www.youtube.com/watch?v=YdgIWTYQ69A)

- [ ] Logging (`logging` module — ne jamais utiliser `print` en prod)
  - 📖 [docs.python.org — logging HOWTO](https://docs.python.org/fr/3/howto/logging.html)
  - 🎬 [Corey Schafer — Logging (25 min)](https://www.youtube.com/watch?v=-ARI4Cz-awo)

- [ ] Async Python — `asyncio`, `async/await`
  - 📖 [docs.python.org — asyncio](https://docs.python.org/fr/3/library/asyncio.html)
  - 📖 [RealPython — Async IO in Python](https://realpython.com/async-io-python/) — long, très complet
  - 🎬 [ArjanCodes — Async/Await (20 min)](https://www.youtube.com/watch?v=2IW-ZEui4h4)

---

## 🌐 Bloc 2 — APIs REST & HTTP

> **Ressources principales :**
> - 📖 [fastapi.tiangolo.com — Tutoriel officiel](https://fastapi.tiangolo.com/tutorial/) — le meilleur tuto de doc officielle qui existe, vraiment
> - 🎬 [freeCodeCamp — FastAPI Full Course](https://www.youtube.com/watch?v=0sOvCWFmrtA) — regarde en 2x, navigue par chapitres

- [ ] Comprendre HTTP : verbes, status codes, headers
  - 📖 [MDN — Aperçu du protocole HTTP](https://developer.mozilla.org/fr/docs/Web/HTTP/Overview)
  - 📖 [httpstatuses.com](https://httpstatuses.com/) — référence rapide des status codes
  - 🎬 [Traversy Media — HTTP Crash Course (35 min)](https://www.youtube.com/watch?v=iYM2zFP3Zn0)

- [ ] Consommer une API avec `requests` (synchrone)
  - 📖 [requests.readthedocs.io — Quickstart](https://requests.readthedocs.io/en/latest/user/quickstart/)
  - 🎬 [Corey Schafer — Requests (45 min)](https://www.youtube.com/watch?v=tb8gHvYlCFs)

- [ ] Consommer une API avec `httpx` (async)
  - 📖 [python-httpx.org — Async support](https://www.python-httpx.org/async/)

- [ ] Authentification API : Bearer token, API key dans headers
  - 📖 [MDN — Authorization header](https://developer.mozilla.org/fr/docs/Web/HTTP/Headers/Authorization)

- [ ] Lire et comprendre une documentation API (Swagger/OpenAPI)
  - 📖 [swagger.io — What is OpenAPI?](https://swagger.io/docs/specification/about/)

- [ ] Créer une API avec **FastAPI** :
  - [ ] Routes, path params, query params → 📖 [FastAPI — First Steps](https://fastapi.tiangolo.com/tutorial/first-steps/)
  - [ ] Request body avec Pydantic models → 📖 [FastAPI — Request Body](https://fastapi.tiangolo.com/tutorial/body/)
  - [ ] Response models → 📖 [FastAPI — Response Model](https://fastapi.tiangolo.com/tutorial/response-model/)
  - [ ] Status codes et gestion d'erreurs HTTP → 📖 [FastAPI — Handling Errors](https://fastapi.tiangolo.com/tutorial/handling-errors/)
  - [ ] Middleware (CORS) → 📖 [FastAPI — CORS](https://fastapi.tiangolo.com/tutorial/cors/)
  - [ ] Background tasks → 📖 [FastAPI — Background Tasks](https://fastapi.tiangolo.com/tutorial/background-tasks/)
  - [ ] Documentation auto (`/docs`) — automatique avec FastAPI, rien à faire

- [ ] Streaming de réponses avec FastAPI
  - 📖 [FastAPI — StreamingResponse](https://fastapi.tiangolo.com/advanced/custom-response/#streamingresponse)
  - 🎬 [Pixegami — FastAPI Streaming (15 min)](https://www.youtube.com/watch?v=DWTYfuGTEAM)

---

## 🕷️ Bloc 3 — Web Scraping

> **Ressources principales :**
> - 📖 [realpython.com — Web Scraping with BeautifulSoup](https://realpython.com/beautiful-soup-web-scraper-python/)
> - 🎬 [freeCodeCamp — Web Scraping Python (1h)](https://www.youtube.com/watch?v=XVv6mJpFOb0)

- [ ] Comprendre le DOM HTML (structure, balises, classes, ids)
  - 📖 [MDN — Introduction au HTML](https://developer.mozilla.org/fr/docs/Learn/HTML/Introduction_to_HTML)
  - 🎬 [Traversy Media — HTML Crash Course (1h)](https://www.youtube.com/watch?v=UB1O30fR-EE)

- [ ] `requests` + `BeautifulSoup` pour pages statiques :
  - 📖 [beautiful-soup-4.readthedocs.io](https://beautiful-soup-4.readthedocs.io/en/latest/)
  - 🎬 [Keith Galli — BeautifulSoup (1h)](https://www.youtube.com/watch?v=GjKQ6V_ViQE)
  - [ ] Sélecteurs CSS (`find`, `find_all`, `select`)
  - [ ] Extraction de texte, liens, attributs

- [ ] `Playwright` (Python) pour pages dynamiques (JavaScript-rendered) :
  - 📖 [playwright.dev/python — Getting Started](https://playwright.dev/python/docs/intro)
  - 🎬 [freeCodeCamp — Playwright Python (1h30)](https://www.youtube.com/watch?v=H2-5ecFwHHQ)
  - [ ] Lancer un browser headless
  - [ ] Attendre des éléments (`.wait_for_selector`)
  - [ ] Gérer scroll et interactions

- [ ] Gérer les cas courants : pagination, rate limiting, User-Agent spoofing
  - 📖 [scrapeops.io — Python Web Scraping Playbook](https://scrapeops.io/python-web-scraping-playbook/)

- [ ] Nettoyer le texte extrait (supprimer HTML résiduel, normaliser les espaces)
  - 📖 [RealPython — Strip HTML with Python](https://realpython.com/python-strip-html-tags/)

- [ ] Respecter le `robots.txt` (éthique + légal)
  - 📖 [robotstxt.org — Introduction](https://www.robotstxt.org/robotstxt.html)

- [ ] Sauvegarder les résultats bruts avant traitement (pattern "raw → processed")

---

## 🧠 Bloc 4 — Embeddings & NLP de base

> **Ressources principales :**
> - 🎬 [3Blue1Brown — Neural Networks playlist](https://www.youtube.com/playlist?list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi) — pour comprendre l'intuition mathématique
> - 📖 [platform.openai.com — Embeddings guide](https://platform.openai.com/docs/guides/embeddings)

- [ ] Comprendre ce qu'est un embedding (vecteur de représentation sémantique)
  - 🎬 [Computerphile — Word Embeddings (12 min)](https://www.youtube.com/watch?v=gQddtTdmG_8)
  - 📖 [Simon Willison — Embeddings explained](https://simonwillison.net/2023/Oct/23/embeddings/) — excellent article vulgarisé

- [ ] Différence entre embedding et tokenisation
  - 📖 [platform.openai.com — Tokenizer interactif](https://platform.openai.com/tokenizer) — très parlant visuellement

- [ ] Utiliser l'API OpenAI pour générer des embeddings :
  - 📖 [platform.openai.com — Embeddings API reference](https://platform.openai.com/docs/api-reference/embeddings)
  - [ ] `text-embedding-3-small` / `text-embedding-ada-002`
  - [ ] Comprendre les dimensions (ex : 1536 pour ada-002)

- [ ] Comprendre la similarité cosinus
  - 🎬 [StatQuest — Cosine Similarity (7 min)](https://www.youtube.com/watch?v=e9U0QAFbfLI)

- [ ] Chunking de documents :
  - 📖 [Pinecone — Chunking Strategies](https://www.pinecone.io/learn/chunking-strategies/) — article de référence
  - 🎬 [Greg Kamradt — 5 Levels of Chunking (30 min)](https://www.youtube.com/watch?v=8OJC21T2SL4)
  - [ ] Pourquoi chunker (limite de tokens des modèles)
  - [ ] Stratégies : fixed-size, par phrase, par paragraphe, recursive
  - [ ] Overlap entre chunks (éviter de couper le sens)
  - [ ] LangChain pour le chunking → 📖 [LangChain — Text Splitters](https://python.langchain.com/docs/concepts/text_splitters/)

- [ ] Métadonnées associées aux chunks (source, page, date, url…)

---

## 🗄️ Bloc 5 — Base de données vectorielle

> **Ressources principales :**
> - 📖 [docs.trychroma.com — Getting Started](https://docs.trychroma.com/docs/overview/getting-started)
> - 🎬 [pixegami — ChromaDB Tutorial (20 min)](https://www.youtube.com/watch?v=QSW2L8dkaZk)

- [ ] Comprendre le principe d'un vector store (indexation ANN)
  - 📖 [Pinecone — What is a Vector Database?](https://www.pinecone.io/learn/vector-database/)
  - 🎬 [IBM — Vector Databases explained (8 min)](https://www.youtube.com/watch?v=dN0lsF2cvm4)

- [ ] Choisir et installer **ChromaDB** :
  - 📖 [docs.trychroma.com](https://docs.trychroma.com)
  - [ ] Créer une collection
  - [ ] Insérer des documents avec embeddings et métadonnées
  - [ ] Requête par similarité (`query`)
  - [ ] Filtrer par métadonnées
  - [ ] Persistance sur disque

- [ ] (Optionnel avancé) Explorer **Qdrant**
  - 📖 [qdrant.tech/documentation](https://qdrant.tech/documentation/)

- [ ] Stratégie de deduplication (éviter d'insérer deux fois le même document)
- [ ] Gestion des mises à jour : supprimer les anciens chunks avant re-ingestion

---

## 🤖 Bloc 6 — RAG (Retrieval-Augmented Generation)

> **Ressources principales :**
> - 🎬 [freeCodeCamp — RAG from Scratch (1h)](https://www.youtube.com/watch?v=sVcwVQRHIc8)
> - 📖 [Pinecone — Retrieval Augmented Generation](https://www.pinecone.io/learn/retrieval-augmented-generation/)

- [ ] Comprendre l'architecture RAG : Retrieve → Augment → Generate
  - 🎬 [IBM — RAG Explained (8 min)](https://www.youtube.com/watch?v=T-D1OfcDW1M)

- [ ] Utiliser l'API LLM (OpenAI / Anthropic / Ollama)
  - 📖 [platform.openai.com — Chat Completions](https://platform.openai.com/docs/guides/chat)
  - 📖 [docs.anthropic.com — Messages API](https://docs.anthropic.com/en/api/messages)
  - 📖 [ollama.com](https://ollama.com) — pour tourner un LLM en local sans coût

- [ ] Construire un prompt système efficace
  - 📖 [docs.anthropic.com — Prompt Engineering](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)

- [ ] Pipeline RAG complet :
  - [ ] Recevoir la question de l'utilisateur
  - [ ] L'embedder
  - [ ] Requêter le vector store (top-k chunks)
  - [ ] Injecter les chunks dans le prompt (context window management)
  - [ ] Appeler le LLM et streamer la réponse → 📖 [OpenAI — Streaming](https://platform.openai.com/docs/api-reference/streaming)

- [ ] Gérer l'historique de conversation (messages précédents dans le contexte)
- [ ] Réponse avec citations de sources (indiquer d'où vient l'info)
- [ ] Évaluer la qualité des réponses (pertinence, hallucination)
  - 📖 [RAGAS — RAG evaluation framework](https://docs.ragas.io/en/latest/)

---

## 📁 Bloc 7 — Ingestion de documents

> **Ressource principale :**
> - 📖 [fastapi.tiangolo.com — File Uploads](https://fastapi.tiangolo.com/tutorial/request-files/)

- [ ] Lire et parser des PDFs (`pypdf` ou `pdfplumber`)
  - 📖 [pypdf.readthedocs.io](https://pypdf.readthedocs.io/en/stable/)
  - 📖 [pdfplumber — GitHub](https://github.com/jsvine/pdfplumber) — meilleur pour l'extraction de tableaux

- [ ] Lire des fichiers Word (`python-docx`)
  - 📖 [python-docx.readthedocs.io](https://python-docx.readthedocs.io/en/latest/)

- [ ] Lire des fichiers texte et Markdown — natif Python, pas de lib nécessaire

- [ ] Gérer l'upload de fichiers avec FastAPI (`UploadFile`)
  - 📖 [FastAPI — Request Files](https://fastapi.tiangolo.com/tutorial/request-files/)

- [ ] Pipeline d'ingestion complet :
  - [ ] Upload → extraction texte → chunking → embedding → stockage
  - [ ] Retourner un statut (nombre de chunks ingérés, source ajoutée)

- [ ] Nommer et identifier les sources de manière unique (hash du contenu ou nom de fichier)

---

## 🎨 Bloc 8 — Frontend (interface utilisateur)

> **Ressources principales :**
> - 🎬 [Traversy Media — React Crash Course (2h)](https://www.youtube.com/watch?v=LDB4uaJ87e0) — pour aller vite
> - 📖 [react.dev — Learn React](https://react.dev/learn) — le nouveau tuto officiel, excellent et interactif

- [ ] HTML/CSS de base :
  - 🎬 [Traversy Media — HTML Crash Course (1h)](https://www.youtube.com/watch?v=UB1O30fR-EE)
  - 🎬 [Traversy Media — CSS Crash Course (1h30)](https://www.youtube.com/watch?v=yfoY53QXEnI)
  - [ ] Flexbox → 🧪 [flexboxfroggy.com](https://flexboxfroggy.com/#fr) — apprendre en jouant
  - [ ] Grid → 🧪 [cssgridgarden.com](https://cssgridgarden.com/#fr)
  - [ ] Variables CSS → 📖 [MDN — Custom properties](https://developer.mozilla.org/fr/docs/Web/CSS/Using_CSS_custom_properties)

- [ ] JavaScript / TypeScript fondations :
  - 🎬 [Traversy Media — JS Crash Course (2h)](https://www.youtube.com/watch?v=hdI2bqOjy3c)
  - 📖 [javascript.info](https://fr.javascript.info/) — la meilleure ressource texte JS, disponible en français
  - [ ] `fetch` API → 📖 [MDN — Utiliser Fetch](https://developer.mozilla.org/fr/docs/Web/API/Fetch_API/Using_Fetch)
  - [ ] `async/await` en JS → 📖 [javascript.info — Async/await](https://fr.javascript.info/async-await)
  - [ ] Manipulation du DOM → 📖 [MDN — Introduction au DOM](https://developer.mozilla.org/fr/docs/Web/API/Document_Object_Model/Introduction)
  - [ ] Gestion d'events (click, submit, input)

- [ ] React :
  - 📖 [react.dev — Learn React](https://react.dev/learn)
  - 🧪 [react.dev — Challenges interactifs](https://react.dev/learn/thinking-in-react)
  - [ ] Composants fonctionnels
  - [ ] State (`useState`)
  - [ ] Props et communication parent/enfant
  - [ ] `useEffect` → 📖 [react.dev — useEffect](https://react.dev/reference/react/useEffect)

- [ ] Interface chatbot :
  - [ ] Affichage des messages (liste scrollable)
  - [ ] Input + bouton d'envoi
  - [ ] Streaming de réponse token par token → 📖 [MDN — ReadableStream](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream)
  - [ ] Indicateur de chargement

- [ ] Interface d'administration :
  - [ ] Formulaire pour ajouter une URL à scraper
  - [ ] Upload de fichier
  - [ ] Liste des sources indexées

- [ ] Appeler le backend FastAPI depuis React (CORS, headers)

- [ ] (Optionnel) TailwindCSS
  - 📖 [tailwindcss.com — Docs](https://tailwindcss.com/docs/installation)
  - 🎬 [Traversy Media — Tailwind Crash Course (1h30)](https://www.youtube.com/watch?v=UBOj6rqRUME)

---

## 🐳 Bloc 9 — Docker & DevOps

> **Ressources principales :**
> - 🎬 [TechWorld with Nana — Docker Tutorial for Beginners (3h)](https://www.youtube.com/watch?v=3c-iBn73dDE) — la référence absolue sur YouTube
> - 🎬 [TechWorld with Nana — Docker Compose (2h)](https://www.youtube.com/watch?v=MVIcrmeV_6c)
> - 📖 [docs.docker.com — Get Started](https://docs.docker.com/get-started/)

- [ ] Comprendre les concepts Docker :
  - [ ] Image vs Container → 📖 [docs.docker.com — Overview](https://docs.docker.com/get-started/overview/)
  - [ ] Layers et cache
  - [ ] Volumes → 📖 [docs.docker.com — Volumes](https://docs.docker.com/storage/volumes/)
  - [ ] Réseaux Docker → 📖 [docs.docker.com — Networking](https://docs.docker.com/network/)

- [ ] Écrire un `Dockerfile` pour une app Python (FastAPI) :
  - 📖 [fastapi.tiangolo.com — Docker deployment](https://fastapi.tiangolo.com/deployment/docker/)
  - [ ] Image de base (`python:3.11-slim`)
  - [ ] Copier les fichiers, installer les dépendances
  - [ ] Exposer le port
  - [ ] CMD vs ENTRYPOINT

- [ ] Écrire un `Dockerfile` pour le frontend (React + Nginx)
  - 📖 [docs.nginx.com — Beginner's Guide](https://nginx.org/en/docs/beginners_guide.html)

- [ ] `docker-compose.yml` pour orchestrer plusieurs services :
  - 📖 [docs.docker.com — Compose file reference](https://docs.docker.com/compose/compose-file/)
  - [ ] Services backend / frontend / vector DB
  - [ ] Variables d'environnement via `.env`
  - [ ] Volumes pour la persistance
  - [ ] Health checks

- [ ] `.dockerignore`
- [ ] Build multi-stage → 📖 [docs.docker.com — Multi-stage builds](https://docs.docker.com/build/building/multi-stage/)
- [ ] Lancer, stopper, rebuilder (`docker compose up/down/build`)
- [ ] Inspecter les logs (`docker logs`, `docker compose logs`)

---

## 🔐 Bloc 10 — Qualité & Sécurité

> **Ressources principales :**
> - 📖 [realpython.com — pytest](https://realpython.com/pytest-python-testing/)
> - 🎬 [ArjanCodes — Unit Testing with pytest (30 min)](https://www.youtube.com/watch?v=ULxMQ57engo)

- [ ] Ne jamais committer de secrets — `.env` + `.gitignore`
  - 📖 [gitignore.io](https://gitignore.io/) — générateur de `.gitignore`

- [ ] Valider toutes les entrées utilisateur (Pydantic côté API)

- [ ] Limiter le rate (éviter d'être spammé sur les endpoints)
  - 📖 [slowapi — Rate limiting pour FastAPI](https://github.com/laurentS/slowapi)

- [ ] Gestion propre des erreurs : erreurs HTTP claires, logger les exceptions

- [ ] Tests unitaires avec `pytest` :
  - 📖 [docs.pytest.org](https://docs.pytest.org/en/stable/)
  - 📖 [FastAPI — Testing](https://fastapi.tiangolo.com/tutorial/testing/)
  - [ ] Tester le pipeline de chunking
  - [ ] Tester les routes FastAPI avec `TestClient`

- [ ] Structurer son projet en modules (ne pas tout mettre dans `main.py`)
  - 📖 [RealPython — Python Application Layouts](https://realpython.com/python-application-layouts/)

- [ ] README clair avec instructions d'installation et de lancement
  - 📖 [makeareadme.com](https://www.makeareadme.com/) — guide + templates

---

## 📦 Stack technique recommandée

| Couche | Technologie |
|---|---|
| Backend API | FastAPI + Python 3.11+ |
| LLM | OpenAI API (ou Anthropic) |
| Embeddings | `text-embedding-3-small` (OpenAI) |
| Vector DB | ChromaDB (local) |
| Scraping | httpx + BeautifulSoup + Playwright |
| Parsing docs | pdfplumber, python-docx |
| Frontend | React + TailwindCSS |
| Container | Docker + docker-compose |

---

## 🏗️ Ordre de construction suggéré

1. **Pipeline d'ingestion seul** (script Python pur, pas d'API) — chunking + embedding + ChromaDB
2. **RAG en ligne de commande** — poser des questions depuis le terminal
3. **API FastAPI** qui expose le RAG (POST /chat)
4. **Ajout du scraping** branché sur l'ingestion
5. **Frontend minimal** connecté à l'API
6. **Dockerisation** du tout
7. **Polish** : UI, gestion d'erreurs, sources dans les réponses

---

*Ce fichier est une check-list vivante. Coche au fur et à mesure — chaque case cochée est une compétence réelle acquise.*
