# TorrentMediaHub

**TorrentMediaHub** est une stack Docker complète permettant de créer un hub de téléchargement automatisé de torrents pour gérer et organiser des films et des séries. Ce projet inclut **qBittorrent**, **Radarr**, **Jackett**, et **Prowlarr**, configurés ensemble pour automatiser la recherche, le téléchargement et l'organisation des médias via des torrents.  
➡️ **Nouvelle version** : ajout de **Gluetun** pour sécuriser toute la stack derrière un VPN et recommandation d'utilisation avec **Tailscale** pour un accès privé.

## Composants principaux

- **qBittorrent** : Client torrent léger et performant utilisé pour télécharger les fichiers.
- **Radarr** : Outil de gestion automatisée de films. Il recherche, télécharge et organise les films.
- **Jackett** : Permet de connecter plusieurs indexeurs de torrents à Radarr et Prowlarr.
- **Prowlarr** : Un gestionnaire d'indexeurs pour Radarr et Sonarr, améliorant la recherche et le téléchargement.
- **Gluetun** : Fournit une connexion VPN (WireGuard/OpenVPN). Toute la stack passe par ce tunnel pour plus de sécurité.

## Prérequis

Avant de commencer, vous devez avoir Docker et Docker Compose installés sur votre machine. Vous pouvez suivre la documentation officielle de Docker pour l'installation :

- [Installer Docker](https://docs.docker.com/get-docker/)
- [Installer Docker Compose](https://docs.docker.com/compose/install/)

Pour un accès distant sécurisé sans ouvrir de ports sur votre box, il est fortement recommandé d'utiliser [Tailscale](https://tailscale.com/) sur votre serveur.

Vous devez également avoir un abonnement VPN compatible (par exemple ProtonVPN) et récupérer la configuration WireGuard ou OpenVPN pour l'utiliser avec Gluetun.

## Installation

1. **Cloner ce dépôt** :

```bash
git clone https://github.com/votre-utilisateur/TorrentMediaHub.git
cd TorrentMediaHub
```

2. **Modifier le fichier `docker-compose.yml`** :

   - Modifiez les volumes pour les répertoires de configuration et de téléchargement afin qu'ils correspondent à vos répertoires locaux.
   - Ajoutez votre configuration VPN dans le service **Gluetun** (WireGuard ou OpenVPN).
   - Vous pouvez ajuster les ports si nécessaire.
   - Modifiez les variables `PUID` et `PGID` pour correspondre à votre utilisateur et groupe. Vous pouvez les obtenir avec la commande suivante :

   ```bash
   id -u <votre_utilisateur>  # pour PUID
   id -g <votre_utilisateur>  # pour PGID
   ```

3. **Démarrer Docker Compose** :

```bash
docker-compose up -d
```

Cela téléchargera les images nécessaires et démarrera les services en arrière-plan.  
⚠️ **Note :** Assurez-vous que Gluetun établit bien la connexion VPN avant d'accéder aux autres services.

## Accès aux interfaces Web

Une fois les services lancés, vous pouvez accéder aux interfaces web suivantes :

- **qBittorrent** : [http://localhost:8080](http://localhost:8080)
- **Radarr** : [http://localhost:7878](http://localhost:7878)
- **Jackett** : [http://localhost:9117](http://localhost:9117)
- **Prowlarr** : [http://localhost:9696](http://localhost:9696)

⚡ Si vous utilisez **Tailscale**, remplacez `localhost` par l'IP Tailscale de votre serveur pour un accès sécurisé depuis vos autres appareils.

## Configuration

1. **qBittorrent** :
   - Configurez qBittorrent en utilisant son interface Web à `http://localhost:8080`.
   - Assurez-vous que les paramètres de téléchargement pointent vers le bon répertoire.

2. **Radarr** :
   - Accédez à Radarr à `http://localhost:7878` pour configurer vos films préférés et les chemins de stockage.
   - Connectez Radarr à Prowlarr pour la gestion des indexeurs.

3. **Jackett** :
   - Allez sur `http://localhost:9117` et configurez vos trackers (indexeurs) de torrents.
   - Une fois Jackett configuré, connectez-le à Prowlarr pour enrichir les résultats de recherche.

4. **Prowlarr** :
   - Accédez à `http://localhost:9696` pour ajouter vos indexeurs de torrents.
   - Associez Prowlarr à Radarr pour permettre à Radarr d'utiliser ces indexeurs.

## Utilisation

1. **Ajouter un film** :
   - Dans **Radarr**, ajoutez un film à votre bibliothèque.
   - Radarr, avec l'aide de Prowlarr et Jackett, recherchera automatiquement les torrents et les enverra à qBittorrent pour les télécharger.
   
2. **Automatisation** :
   - Une fois configuré, le processus de recherche, de téléchargement et d'organisation des films devient entièrement automatisé.

3. **Gestion des torrents** :
   - Vous pouvez gérer vos torrents directement depuis l'interface Web de qBittorrent ou via les autres outils connectés.

## Sécurité

- Toute la stack est encapsulée derrière **Gluetun**, garantissant que le trafic sortant passe toujours par un VPN.
- Avec **Tailscale**, vous pouvez accéder à vos services en toute sécurité depuis n’importe où sans ouvrir de ports sur votre box.
- Si vous exposez vos services à Internet, il est recommandé d'utiliser un proxy inverse comme **nginx** ou **Traefik** pour renforcer la sécurité.
