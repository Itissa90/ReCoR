ReCoR — Caisse de Retraites du Togo
3.Dossiers : ouvrez un dossier, suivez sa timeline et faites avancer son statut.
4.Notifications : transmettez un avis, enregistrez sa réception ou une relance.
5.Comptes : consultez un compte assuré, puis cliquez sur une période pour « Expliquer l'écart ».
6.États et éditions : prévisualisez un état et exportez-le en CSV, Excel ou version imprimable.
7.Traçabilité : vérifiez l'historique horodaté des actions.

Réinitialiser les données de démonstration

Fermez le serveur, supprimez data/recor.db, puis relancez Demarrer-ReCoR.bat : la base est recréée avec le jeu de démonstration d'origine.

Cette opération efface toutes les saisies effectuées dans l'application.

Déploiement sur le serveur CRT

1.Copiez le dossier complet de l'application sur le serveur.
2.Vérifiez que Node.js 24 (ou le runtime portable fourni) est disponible.
3.Démarrez avec runtime\node\node.exe app\server.js ou node app/server.js.
4.Placez un reverse proxy HTTPS devant le port 3000 et restreignez l'accès au réseau CRT.
5.Sauvegardez régulièrement data/recor.db et le dossier data/pieces.
6.Avant la mise en production : changez les mots de passe de démonstration et adaptez les paramètres dans Administration.

Le port peut être changé sans modifier le code : set PORT=3001 avant le démarrage.

Mise sur GitHub

Le dépôt ne doit pas contenir la base de données, les exports, ni le moteur Node portable (fichiers volumineux ou propres à chaque poste). Le fichier .gitignore les exclut déjà.

1.Créez un dépôt vide sur GitHub (sans README ni licence).
2.Dans le dossier du projet, exécutez :

code.bat
git init
git add .
git commit -m "ReCoR v1.0 - plateforme CRT"
git branch -M main
git remote add origin https://github.com/VOTRE_COMPTE/recor-crt.git
git push -u origin main

3.Sur une autre machine, clonez le dépôt puis installez Node.js 24 (https://nodejs.org) avant de lancer npm start ou node app/server.js. Le dossier runtime/ et le jeu de démonstration sont recréés automatiquement au premier démarrage.

Un fichier ZIP déposé via l'interface web de GitHub reste une pièce jointe binaire : il n'est pas exécutable et ne permet pas de naviguer dans le code. Utilisez Git pour publier les sources.

Notes techniques

•Zéro dépendance npm ; moteur SQLite intégré à Node.js.
•Sessions HttpOnly de 8 heures, jetons CSRF, hachage des mots de passe, limitation des tentatives de connexion, en-têtes de sécurité.
•Exports CSV protégés contre l'injection de formules ; journal de traçabilité à 8 champs.
•Les PDF sont produits via l'impression du navigateur (Imprimer → « Enregistrer au format PDF »).
•Les données affichées sont fictives et destinées à la démonstration.

Dépannage

•« Le port 3000 est déjà utilisé » : arrêtez l'autre instance, ou lancez avec set PORT=3001 puis ouvrez http://127.0.0.1:3001.
•La page ne s'ouvre pas : vérifiez que la fenêtre du serveur est toujours ouverte et qu'aucun antivirus ne bloque Node.js.
•Base incohérente après des essais : appliquez la procédure de réinitialisation ci-dessus.
