# installation-wekan-debian11
# Instructions d'installation détaillées pour Wekan sur Debian 11
# Mise à jour du système :
apt update
apt upgrade -y

# Installation des dépendances :
apt install -y curl gnupg2 git

# Installation de Node.js :
curl -fsSL https://deb.nodesource.com/setup_14.x | sudo -E bash -
apt install -y nodejs

# Installation de MongoDB :

echo "deb http://repo.mongodb.org/apt/debian buster/mongodb-org/4.4 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
curl -fsSL https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -
apt update
apt install -y mongodb-org

# Démarrage et activation de MongoDB :
systemctl start mongod
systemctl enable mongod

# Installation de Wekan :

git clone https://github.com/wekan/wekan.git
cd wekan
npm install

# Configuration de Wekan :
cp ./settings.json.example ./settings.json
nano ./settings.json
## Dans le fichier settings.json, configurez les paramètres selon vos besoins.

#Démarrage de Wekan :
npm start

# Configuration du service Wekan (optionnel) :
nano /etc/systemd/system/wekan.service
#Ajoutez le contenu suivant dans le fichier de service :
[Unit]
Description=Wekan kanban board
After=network.target mongod.service

[Service]
ExecStart=/usr/bin/npm start --prefix /chemin/vers/wekan
WorkingDirectory=/chemin/vers/wekan
Restart=always
User=nom_utilisateur
Group=nom_groupe

[Install]
WantedBy=multi-user.target

### Assurez-vous de remplacer /chemin/vers/wekan par le chemin absolu vers le répertoire Wekan, et nom_utilisateur et nom_groupe par l'utilisateur et le groupe sous lequel vous souhaitez exécuter Wekan.

### Activer et démarrer le service Wekan :

systemctl daemon-reload
systemctl enable wekan
systemctl start wekan



