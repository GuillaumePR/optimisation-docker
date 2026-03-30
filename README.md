1. Quel est le poids de `tp-docker:classic` ?
tp-docker    classic   32bc2d07ec6a   2 minutes ago   1.12GB
2. Qu’est-ce qui “gonfle” potentiellement l’image ici ?
Image de base volumineuse (node:20) qui n'est pas slim ou alpine
Absence de .dockerignore

Pourquoi node_modules dans le .dockerignore est une bonne idée ?
Evite d'inclure des fichiers inutiles dans l'image, et réduit le temps de build

Nouveau poids :
tp-docker    multistage   896df6ef4bb1   39 seconds ago   202MB

Quelles étapes créent les plus gros layers ?
'RUN npm ci' : très lourd avec les node_modules
'COPY . . ' massif sans .dockerignore

Comment le fait de copier package*.json avant le reste aide le cache Docker ?
Pas de réinstallation de dépendances et donc aussi plus rapide 



Mini compte-rendu :
L’image classic pèse environ 1 GB, tandis que l’image multistage descend à environ 200 MB, soit une réduction considérable du pois d'image.

Le multi-stage permet de réduire le poids car en éliminant les dépendances de développement et les outils de build dans l’image finale. On évite de copier des fichiers inutiles en ne gardant que les choses nécessaires.

En production, une amélioration supplémentaire serait d’utiliser une image plus minimale comme node:alpine, ou d’exécuter l’application avec un utilisateur non-root que l'on créee avant pour renforcer la sécurité.
