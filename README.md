# 🌐 Formation Azure - Guide d'installation & premières commandes

## 🔗 Accès au Portail Azure

- **URL :** [https://portal.azure.com/#home](https://portal.azure.com/#home)  
- **Utilisateur :** `training-user-15@soatformation.onmicrosoft.com`

---

## 🛠️ Installation du client Azure pour PowerShell

### 1. Lancer PowerShell en tant qu'administrateur

Autoriser l'exécution de scripts :

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### 2. Installer et mettre à jour le module Azure PowerShell

```powershell
Install-Module -Name Az -Repository PSGallery -Force
Update-Module -Name Az -Force
```

> 💡 Cela active les commandes Azure PowerShell (`Connect-AzAccount`, etc.), **différentes** des commandes AZ CLI utilisées sur Linux ou CMD.

---

## 🔧 Installation de l’interface AZ CLI

- **Téléchargement :** [https://aka.ms/installazurecliwindowsx64](https://aka.ms/installazurecliwindowsx64)

> Cela permet d’utiliser des commandes comme `az login`, `az provider list`, etc.

---

## 🔐 Connexion à Azure

### Avec PowerShell

```powershell
Connect-AzAccount -UseDeviceAuthentication
```

### Avec AZ CLI

```bash
az login --use-device-code
```

---

## 🔍 Vérifier la connexion actuelle

```bash
az account show
```

Affiche l’état de connexion et les informations du compte.

---

## 📚 Commandes & ressources utiles

- **Commandes AZ CLI :**  
  [https://learn.microsoft.com/en-us/cli/azure/reference-index?view=azure-cli-latest](https://learn.microsoft.com/en-us/cli/azure/reference-index?view=azure-cli-latest)

- **Commandes PowerShell (exemple) :**

```powershell
Get-Help Get-AzVM -Full
```

---

## 💻 Utilisation de Microsoft Azure

Deux méthodes principales :

- **Console Web**
- **Ligne de commande** (PowerShell ou AZ CLI)

---

## ⚙️ Création de ressources Azure

### Exemple : création d’un groupe de ressources

```bash
az group create --name training-Beauquier-rg2 --location westeurope
```

---

## 🧮 Utilisation de variables dans PowerShell

Définir des variables pour les réutiliser plus facilement :

```powershell
$RESOURCE_GROUP = "training-Beauquier-rg"
$AVAILABILITY_SET_NAME = "MyAvailabilitySet"
$LOCATION = "westeurope"
$AZUREUSER = "azureuser"
```

Afficher toutes les variables :

```powershell
Get-Variable
```

---

## 🚀 Création d'une machine virtuelle

### En ligne de commande :

```bash
az vm create \
  --resource-group $RESOURCE_GROUP \
  --name MyVM \
  --image Ubuntu2204 \
  --admin-username $AZUREUSER \
  --generate-ssh-keys
```

---

## 🔐 Connexion SSH à la VM
Différentes méthode : 
  - Par client ssh en ligne de commande
  - Par Client Web Azure
  - Par un Putty par exemple (ssh)
    
### Étapes client ssh en ligne de commande : 
Quand on utilise --generate-ssh-keys , Azure stock la clé public et privé dans c:/users/nathan/.ssh

  1. ajout du client ssh sur Powershell :
  ```bash
    Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
  ```
  2. Connexion en ligne de commande :
  On peut donc ce connecter directement a la vm par le biais de cette commande par exemple :
  ```bash
    ssh -i ~/.ssh/id_rsa.pem azureuser@IPPUBLICVM
  ```

### Étapes client Web Azure : 
  1. **Aller dans votre ressource groupe**  

  2. **Selectionner votre VM**

  3. **Cliquer sur Connect, puis SSH using Azure CLI**

### Étapes client Putty :
  1. Lancer putty
  2. Charger la clé dans **Connexion -> SSH -> Auth -> Credentials**
  3. Dans Session, renseigner L'IP Public et le port SSH puis démarrer la connection
---
> 🧠 Pensez à bien configurer votre groupe de sécurité réseau pour autoriser la connexion SSH (port 22).


## 📦 Templates Azure (ARM Templates)

Les templates Azure (ou ARM templates) sont des fichiers JSON (ou Bicep) qui décrivent de manière **déclarative** l'infrastructure que vous souhaitez déployer sur Azure.

---

### 🛁 Structure d’un template ARM (JSON)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "defaultValue": "MyVM",
      "metadata": {
        "description": "Nom de la machine virtuelle"
      }
    }
  },
  "variables": {
    "location": "[resourceGroup().location]"
  },
  "resources": [
    {
      "type": "Microsoft.Compute/virtualMachines",
      "apiVersion": "2021-11-01",
      "name": "[parameters('vmName')]",
      "location": "[variables('location')]",
      "properties": {
        // Configuration de la VM
      }
    }
  ],
  "outputs": {
    "vmName": {
      "type": "string",
      "value": "[parameters('vmName')]"
    }
  }
}
```

---

### ⚙️ Composants principaux d’un template

| Élément      | Description                                               |
| ------------ | --------------------------------------------------------- |
| `parameters` | Entrées personnalisables (ex : nom de VM, taille, etc.)   |
| `variables`  | Valeurs dérivées utilisées plusieurs fois                 |
| `resources`  | Liste des ressources à déployer                           |
| `outputs`    | Valeurs affichées après le déploiement (ex : IP publique) |

---

### 🚀 Déploiement d’un template

**Via Azure CLI :**

```bash
  az deployment group create --resource-group $RESOURCE_GROUP --template-file ./template.json --parameters ./parameters.json
```

**Via le Portail Azure :**

1. Créez un groupe de ressources.
2. Allez dans "Déployer un modèle personnalisé".
3. Collez ou chargez votre template.

---

### 📁 Exemples de templates

Vous pouvez consulter la bibliothèque officielle ici :
- **Templates de déploiement Azure :**  
👉 [https://github.com/renauddahou/azure-quickstart-templates](https://github.com/renauddahou/azure-quickstart-templates)

---

