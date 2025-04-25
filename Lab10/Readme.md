# Laboratoire 10: Configuration Geoserver et mise en place de services VTS et WFS

## Étape 1 : Configuration et lancement d’une instance de Geoserver

**1**. Ouvrir `GitHub` et Lancer un Codespace à partir de notre fork du dépot github du cours (sur la branche main le codespace).

---
![alt text](<create new fork.png>)
---
![alt text](fork.png)
---

![alt text](<create new code space.png>)
---

![alt text](<new code space.png>)
---

## Étape 2 : Configuration de l’environnement

**1**.Dans le dossier `Atlas`, on Copie-colle le fichier  `.env.example` situé dans le dossier Atlas (dans le même dossier) puis on renomme le fichier en `.env` (supprimez le .example).
 Ensuite, on va modifier les variables d’environnement avec nos informations personnelles :
 ```bash
DB_USER=CODEPERMANENT
DB_PASSWORD=VOTREMOTDEPASSE
DB_HOST=geo7630h25.cvwywmuc8u6v.us-east-1.rds.amazonaws.com
DB_NAME=geo7630
```
---
![alt text](fichier.env.png)
---

**2**.Dans le dossier Atlas,  on fait un clic droit sur le fichier `docker-compose.yml` et on sélectionne `Compose Up`. 
Dans notre cas, on va installer tout dabord `Docker`
---
![alt text](<installation docker compose.png>)
---




