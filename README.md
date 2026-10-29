# IsyWeb

<p align="center">
  <a href="https://buymeacoffee.com/khirale">
    <img
      src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png"
      alt="Buy Me a Coffee"
      height="45"
    >
  </a>
</p>


**Suivez votre installation Isypower depuis votre ordinateur, dans votre navigateur.**

IsyWeb est un petit programme à lancer sur votre PC (Windows ou Linux). Il ouvre une page web locale qui affiche votre installation solaire : flux d'énergie en direct, batteries, production par panneau, bilans et économies en euros. Vous utilisez les **mêmes identifiants** que dans l'application mobile Isypower.

Pas d'installation compliquée, pas de compte à créer, pas de serveur à louer : un seul fichier à lancer.

> Projet communautaire, **non affilié à Materfrance** ni à Isypower. IsyWeb utilise le même cloud que l'application mobile, avec votre propre compte. Il est fourni tel quel, sans garantie.

---

## Ce que vous pouvez faire

- **Accueil** : flux d'énergie animé (production, maison, réseau, batteries, borne de recharge), économies du jour, prévision solaire sur 3 jours, courbes de la journée.
- **Production** : par micro-onduleur, par batterie (entrées solaires) et par panneau, avec les pinces CT 2 et CT 3 du compteur intelligent comme sources à part. Contrôle de santé des panneaux sur 7 jours.
- **Batteries** : cluster maître / esclave, charge de chaque batterie, modules, réglages.
- **Bilan** : jour, mois, année et total, avec graphiques et économies en euros. Consommation et achats sont détaillés par tarif (heures pleines / creuses, Tempo…).
- **Tarifs** : Base, Heures pleines / creuses (plusieurs plages dans la journée), Tempo, OctoSaisons (Octopus) ou personnalisé. Périodes été / hiver, et changements de prix à une date donnée.
- **Appareils** : état de chaque appareil, renommage (le nom reste local à IsyWeb), réglages.
- **Alertes** : batterie faible, appareil hors ligne, panneau à vérifier, jour Tempo rouge… avec notifications Windows ou Linux.
- **Export CSV** (ouvrable dans Excel), **mode affichage mural** plein écran.
- **Accès depuis un autre appareil de la maison** (téléphone, tablette), protégé par un code à 6 chiffres. Désactivé par défaut.

---

## Téléchargement

Rendez-vous dans la section **Releases** du dépôt et téléchargez le fichier correspondant à votre machine :

| Système | Fichier |
|---|---|
| Windows 10 / 11 | `IsyWeb-X.Y.Z-Windows.zip` |
| Linux, PC ou serveur Intel / AMD (le plus courant) | `isyweb-X.Y.Z-linux-x86_64.tar.gz` |
| Linux, Raspberry Pi 3 / 4 / 5 (système 64 bits) | `isyweb-X.Y.Z-linux-arm64.tar.gz` |
| Linux, Raspberry Pi (système 32 bits) | `isyweb-X.Y.Z-linux-armv7.tar.gz` |

Pas sûr de votre architecture Linux ? Tapez `uname -m` dans un terminal : `x86_64` → x86_64, `aarch64` → arm64, `armv7l` → armv7.

---

## Démarrage rapide

### Windows

1. Décompressez l'archive, puis double-cliquez sur **`IsyWeb.exe`**.
2. Si Windows affiche « Windows a protégé votre ordinateur » : cliquez sur **Informations complémentaires**, puis **Exécuter quand même**. Ce message apparaît pour tout petit programme non signé, une seule fois.
3. Votre navigateur s'ouvre sur la page. L'icône IsyWeb apparaît près de l'horloge (parfois derrière la flèche ^) : clic pour ouvrir la page, clic droit pour « Démarrer avec Windows », activer l'accès réseau ou quitter.
4. Connectez-vous avec l'e-mail et le mot de passe de votre compte Isypower. Cochez « Rester connecté » pour ne plus les retaper.

### Linux avec écran

```bash
tar xzf isyweb-X.Y.Z-linux-x86_64.tar.gz
cd isyweb-X.Y.Z-linux-x86_64
./install.sh
```

Pas besoin de `sudo`. Le navigateur s'ouvre sur la page, et IsyWeb apparaît ensuite dans le menu des applications.

### Linux sans écran (Raspberry Pi, mini-PC, serveur, machine virtuelle)

```bash
./install.sh --service
```

IsyWeb tourne alors en service et démarre avec la machine. Activez l'accès réseau (voir plus bas) pour l'utiliser depuis un autre appareil. Le `LISEZ-MOI.txt` de l'archive Linux détaille chaque étape, pas à pas.

---

## Accès depuis un téléphone ou un autre PC

Désactivé par défaut. Pour l'activer : **Réglages › Accès depuis le réseau local** (ou clic droit sur l'icône IsyWeb).

1. IsyWeb affiche une adresse (par exemple `http://192.168.1.20:8765`), un code à 6 chiffres et un QR code.
2. Sur l'autre appareil, connecté au même Wi-Fi, scannez le QR code ou saisissez l'adresse puis le code.
3. L'appareil reste autorisé 30 jours. « Déconnecter tous les appareils » les révoque tous et change le code.

Le PC (ou la machine) doit rester allumé avec IsyWeb lancé. Sous Windows, si le pare-feu le demande, cochez **Réseaux privés** puis **Autoriser l'accès**.

> Les échanges ne sont pas chiffrés sur le réseau local : n'activez cette option que chez vous, jamais sur un Wi-Fi public. Ne jamais exposer IsyWeb sur Internet.

---

## À savoir

- **Les données se rafraîchissent depuis la page ouverte.** Toutes les 3 minutes tant qu'un onglet IsyWeb est ouvert. Le programme n'interroge pas le cloud de lui-même : sans page ouverte, il n'y a ni mise à jour ni alerte.
- **Les calculs viennent de grandeurs mesurées.** Production = somme de tous les panneaux et des pinces CT. Économies = Σ (production − injection − charge batterie + décharge batterie) × prix de la demi-heure. La « consommation » du cloud est un calcul dérivé qui peut être faux (batterie chargée sur le réseau) : IsyWeb la recalcule lui-même (production + achat + décharge − charge − injection), et la valeur du cloud reste visible au survol.
- **Historique.** Le cloud ne fournit le détail jour par jour que pour les mois récents. Les anciens mois sont valorisés au prix moyen du tarif, et un bandeau le signale. Pour Année et Total, le premier calcul télécharge les jours lentement, puis le cache du navigateur est conservé.
- **Limitation des appels au cloud.** Pour ménager le service, l'« interrogation immédiate » du bouton Actualiser est limitée à 2 fois par 20 minutes, et les lectures identiques sont mémorisées 20 secondes. L'actualisation automatique n'est pas concernée.
- **Pinces CT.** Si une pince du compteur est câblée sur une autre ligne que prévu, décochez « Compter dans la production et la consommation » sur sa carte (onglet Production) : sa valeur reste affichée mais n'entre plus dans les calculs instantanés.

---

## Confidentialité

- Sans l'option réseau local, le programme n'écoute que sur votre ordinateur (127.0.0.1).
- Vos identifiants ne sont envoyés qu'au cloud Isypower, comme le fait l'application mobile. « Rester connecté » les garde dans le navigateur ; « Se déconnecter » les efface. Ils ne sont **jamais** écrits dans les réglages d'IsyWeb.
- Les réglages (tarifs, alertes, noms des appareils) sont enregistrés dans `%APPDATA%\IsyWeb\settings.json` (Windows) ou `~/.config/IsyWeb/` (Linux).
- La prévision solaire utilise la météo gratuite [Open-Meteo](https://open-meteo.com/) : seule la position de la centrale lui est transmise.
- Avec un tarif Tempo, les couleurs des jours viennent de [api-couleur-tempo.fr](https://www.api-couleur-tempo.fr/) (aucune donnée personnelle).

---

## Dépannage

| Problème | Piste |
|---|---|
| Windows bloque le lancement | « Informations complémentaires » › « Exécuter quand même ». Si l'antivirus supprime le fichier, l'autoriser dans ses exceptions. |
| La page ne s'ouvre pas | Ouvrez `http://127.0.0.1:8765/`. Si rien ne répond, IsyWeb n'est pas lancé, ou le port 8765 est déjà pris par un autre logiciel. |
| « E-mail ou mot de passe incorrect » | Vérifiez les mêmes identifiants dans l'application mobile, retapez le mot de passe à la main (sans espace en fin de saisie), essayez en navigation privée. |
| « Le cloud Isypower ne répond pas » | Vérifiez la connexion Internet, un pare-feu ou un antivirus qui inspecte le trafic HTTPS. |
| Rien ne s'affiche sur un autre appareil | Même Wi-Fi, accès réseau activé, pare-feu autorisé (réseau en profil « Privé » sous Windows). |
| Pas de notification | Réglages › Notifications : activer, puis « Envoyer une notification de test ». |


---

## Ligne de commande

```text
IsyWeb --status              état de l'instance en cours
IsyWeb --lan on|off          activer / désactiver l'accès réseau local
IsyWeb --lan-revoke          déconnecter tous les appareils et changer le code
IsyWeb --autostart on|off    démarrage avec la session
IsyWeb --no-browser          ne pas ouvrir le navigateur au démarrage
IsyWeb --quit                arrêter l'instance en cours
IsyWeb --version
```

---

## Compiler depuis les sources

Go 1.24 ou plus récent, bibliothèque standard uniquement (aucune dépendance).

```bash
# Linux (x86_64)
CGO_ENABLED=0 go build -trimpath -ldflags "-s -w -X main.version=X.Y.Z" -o isyweb .

# Windows (sans console, icône dans la zone de notification)
CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build -trimpath -ldflags "-s -w -H windowsgui -X main.version=X.Y.Z" -o IsyWeb.exe .

# Raspberry Pi
CGO_ENABLED=0 GOOS=linux GOARCH=arm64 go build -trimpath -ldflags "-s -w -X main.version=X.Y.Z" -o isyweb .
CGO_ENABLED=0 GOOS=linux GOARCH=arm GOARM=7 go build -trimpath -ldflags "-s -w -X main.version=X.Y.Z" -o isyweb .
```

Le serveur est en Go ; l'interface est une seule page `index.html` embarquée dans le binaire. Un faux cloud de test (`mock_cloud_test.py`) permet de développer sans compte : lancez-le, puis démarrez IsyWeb avec `ISYWEB_UPSTREAM=http://127.0.0.1:9000/photo_voltaic`.

| Fichier | Rôle |
|---|---|
| `main.go` | serveur local, relais vers le cloud, sécurité |
| `throttle.go` | limitation des appels au cloud |
| `lan.go`, `pair.html` | accès réseau local (code, QR, cookies) |
| `settings.go` | réglages persistants |
| `cli.go` | options en ligne de commande |
| `ui_windows.go`, `ui_linux.go`, `ui_other.go` | icône, notifications, démarrage automatique |
| `index.html` | toute l'interface |
| `linux/install.sh` | installation Linux (utilisateur ou service) |

---

## Contribuer

Les retours sont les bienvenus via les *Issues* : indiquez la version (en bas de la page), le système et, si possible, une capture d'écran. **Ne publiez jamais** vos identifiants, jetons ou numéros de série.
