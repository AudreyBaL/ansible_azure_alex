Cours INFR010 - Ansible avec Alex Faivre pour CAPGIPI6
Audrey Balagué, grandement aidée par : Jonathan Genthon, Fayard Elise, Clémentine Dauchot et Madjid Chabane

L'exercice consiste dans le fait de créer un playbook ansible permettant de créer une VM sur Azure avec :
Un virtual Network
Un resource group
Un subnet
Une VM
Un IP publique associée à la VM (pour pinguer depuis l'extérieur)
un Network Security Group (firewall) permettant de faire du SSH sur votre VM Linux ou du RDP sur Windows (attention plus difficile)

Avant de commencer à créer le playbook il est nécessaire de s'assurer que nous avons bien un compte Azure actif avant les droits pour créer des ressources.
Sur notre machine avec ansible nous devons avoir :
- L'ensemble des modules ansible et python à jour
- curl installé
- gnupg installé
- installer uv

Nous pouvons lancer les commandes suivantes pour préparer notre environnement : 

uv python install
uv venv ansible_ipi
ansible_ipi/bin/activate
Activer l'environnement virtuel :
source ansible_ipi/bin/activate
Mettre à jour pip et installer Ansible :
uv tool run pip install --upgrade pip
uv tool run pip install ansible
Installer la collection Azure pour Ansible :
ansible-galaxy collection install azure.azcollection --force
uv tool run pip install -r ~/.ansible/collections/ansible_collections/azure/azcollection/requirements.txt
uv tool run pip install ansible[azure]
Configurer les alias pour les commandes Ansible :
alias pip="uv tool run pip"
alias ansible="uv tool run ansible"
alias ansible-playbook="uv tool run ansible-playbook"
alias ansible-vault="uv tool run ansible-vault"
Voici le Bash d'installation:

Avant d'éxecuter le playbook, nous devons créer un fichier "vaulté" pour stocker le mot de passe de l'user qui sera utilisé pour se connecter à la VM :

ansible-vault create vault_password.yml

Commande pour lancer le playbook : 

ansible-playbook --ask-vault-pass exercice_ansible_azure.yml

On peut alors vérifier que le playbook s'est bien déroulé et que dans Azure on retrouve bien toutes les ressources crées :

![image](https://github.com/user-attachments/assets/a2aab83e-f98b-482a-8b1d-8bc14109617c)

On peut également tenter de se connecter au serveur crée via son adresse ip publique, l'utilisateur renseigné et le mot de passe contenu dans le fichier vault.
