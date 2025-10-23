
# 🌀 TorrentMediaHub

**TorrentMediaHub** est une stack Docker complète et sécurisée pour automatiser la recherche, le téléchargement et l’organisation de vos films via des torrents.  
Elle intègre **qBittorrent**, **Radarr**, **Prowlarr**, et **Gluetun (VPN)** pour garantir à la fois automatisation et anonymat.

---

## 🚀 Composants principaux

| Service | Description |
|----------|--------------|
| **Gluetun** | Conteneur VPN (ici avec **ProtonVPN** en **WireGuard**) assurant l’anonymat de tout le trafic torrent. |
| **qBittorrent** | Client torrent performant, utilisé pour gérer les téléchargements. |
| **Radarr** | Gestion automatisée de films : recherche, téléchargement et organisation. |
| **Prowlarr** | Gestionnaire centralisé d’indexeurs de torrents, utilisé par Radarr. |

---

## 🧩 Fonctionnement global

Tous les services (qBittorrent, Radarr, Prowlarr) passent **par le VPN Gluetun** grâce à `network_mode: "service:gluetun"`.  
Aucun trafic torrent ne sort donc en dehors du tunnel VPN.  
Les interfaces web restent accessibles localement grâce à l’ouverture contrôlée des ports dans Gluetun.

- **qBittorrent** : [http://localhost:8080](http://localhost:8080)
- **Radarr** : [http://localhost:7878](http://localhost:7878)
- **Prowlarr** : [http://localhost:9696](http://localhost:9696)

---

## ⚙️ Prérequis

Avant de commencer :

- **Docker** et **Docker Compose** installés :  
  - [Installer Docker](https://docs.docker.com/get-docker/)  
  - [Installer Docker Compose](https://docs.docker.com/compose/install/)
- Un compte **ProtonVPN** (ou autre fournisseur supporté par Gluetun)
- Des identifiants WireGuard ProtonVPN disponibles dans le tableau de bord [ProtonVPN](https://account.protonvpn.com/downloads)

---

## 📦 Installation

### 1. Cloner le dépôt

```bash
git clone https://github.com/votre-utilisateur/TorrentMediaHub.git
cd TorrentMediaHub
