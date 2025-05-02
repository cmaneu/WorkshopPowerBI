# Introduction & Architecture Générale

L'objectif de ce pas à pas est de proposer un environnement de travail au sein de Microsoft Fabric avec les leviers de sécurité d'accès les plus restrictifs. Nous allons découvrir comment fournir l'accès aux ressources uniquement depuis le réseau interne d'une entreprise ou d'une organisation, pour être capable de développer un Notebook Spark au sein de Microsoft Fabric, qui aurait besoin d'atteindre des données contenues dans un Data Lake au sein d'Azure. L'échange entre l'utilisateur et la plateforme se fait ne manière privée, et l'échange entre Microsoft Azure et Microsoft Fabric se fait lui aussi de manière privée.

Nous allons pas à pas déployer un environnement qui respecte les contraintes suivantes : 
  - Utilisation du Private Link : Les échanges avec Power BI et/ou Microsoft Fabric sont fait via le réseau Azure, et non via Internet. Cela permet de contrôler le point d'entrée des utilisateur, ainsi que les intéreactions avec les données qui transitent vers et en dehors de Microsoft Fabric. 
  - Désactivation de l'accès depuis Internet public : Non seulement l'accès se fait via les services interne Azure, mais en plus l'accès au portail ne peut plus se faire via l'IP publique du service.
  - Désactivation des accès public aux ressources dans Azure : En désactivant les accès au Key Vault (container des clés d'identification) ou à ADLS Gen2 (container des données) via un réseau public, on interdit aussi les accès depuis n'importe quel endroit
  - Utilisation de private endpoints pour les services Azure : On définit un point d'entrée approuvé pour accéder aux ressources Azure, ce point d'entrée est donc strictement réservé à un service ou un usage.
  - Consommations des services Azure via Microsoft Fabric de manière privée : Une fois l'accès au portail restreint, les échanges entre le service et les ressources maîtrisé, les utilisateurs et développeurs évolueront dans un environnement cloisoné.  

**Notes & Limitations :**
  - Managed Private Network : Lorsqu'un Administrateur active le private Link, le lancement d'un notebook Spark générera automatiquement l'usage de Managed Virtual Network pour instancier les ressources de calcul Spark. Ce cluster n'est pas accessible depuis le réseau public. L'instanciation des ressources au sein de ce Managed Virtual Network est plus long que lorsqu'on l'utilise de manière publique, le démarrage d'une session passe de 30 secondes environ à 4 minutes.
     
  - Starter Pools : Il est possible de lancer un Notebook Spark sans se soucier de la configuration des ressources de calculs utilisées. Dans ce cas, ce pool de ressource appelé _Starter Pool_ sera instancié et possède une taille standard mais non optimale pour exécuter les calculs. Lors de la privatisation des ressources, ce Starter Pool n'est plus disponible, et il est donc nécessaire d'en créer un avec une taille définie directement au sein du Workspace par exemple pour être capable d'exécuter notre Notebook.  

Une fois ces différents pré-requis validés, nous allons voir comment nous pouvons utiliser Microsoft Fabric pour transformer nos données et les faire évoluer dans notre environnement. 

**Architecture générale :** 

<img src="https://github.com/user-attachments/assets/ab850c9f-8417-478f-bf26-a90a1baad66a">

L'usage d'une VM dans Azure n'est pas nécessaire. 

# Pré requis

Pour ce pas à pas, nous aurons besoin : 
  - **Les rôles** : 
    - Au sein d'Azure, nous avons besoin des droits de création dans une souscription de test ou d'un groupe de ressources 
    - Au sein de Microsoft Fabric, des droits d'administrateurs sur l'environnement. 
  - **Les ressources** : 
    - D'un environnement **Azure**, avec la possibilité de créer :
      - Une capacité Microsoft Fabric 
      - Un Virtual Network ainsi qu'un Bastion
      - Une Virtual Machine 
      - Un private Link Services
      - Un Key Vault
      - Un Azure Data Lake Storage Gen 2
    - D'un environnement **Microsoft Fabric** avec le rôle Administrateur :
      - Pour activer/desactiver les features au sein du portail
      - La possibilité de créer un workspace, qui va contenir un Notebook

# Pas à pas 

**Activation des features dans le portail Power BI :**

Dans le portail d'administration Power BI, nous allons activer la feature permettant d'utiliser les Private Links : Accéder au portail d'administartion, rechercher les paramètres réseaux avancés, et activer la feature : 

<img src="https://github.com/user-attachments/assets/638cbb9f-7586-41c0-a6fb-2809fddb171d" width="300">
<img src="https://github.com/user-attachments/assets/4fcba74a-4e9e-4aff-b505-18f8c48cb04f" width="400">

Nous allons aussi récolter l'ID de notre tenant Microsoft Fabric : 

Dans Azure : 
<img src="https://github.com/user-attachments/assets/3a2f0f70-98b0-4bee-889b-f590af92e954" width="300">
<img src="https://github.com/user-attachments/assets/ef9ced03-cb39-47ce-9876-d7cadd99b1b5" width="300">

Once created click 
<img src="https://github.com/user-attachments/assets/796576ae-db3a-4b57-b5df-62234418f900" width="300">


In RG, create item 
<img src="https://github.com/user-attachments/assets/9c076b8e-e2d5-4eb5-b71f-43469ba176d0" width="300">

Create Template 
<img src="https://github.com/user-attachments/assets/3dd14a61-365e-4f5b-85d9-c15de5bec98b" width="300">

Change variables 
<img src="https://github.com/user-attachments/assets/4d72d13c-183e-436c-8328-205af0a96310" width="300">

Get About Pbi 
<img src="https://github.com/user-attachments/assets/eda6e8f3-7528-4452-acbd-5e5cd6cccd8c" width="300">
<img src="https://github.com/user-attachments/assets/02293db2-0d6b-4358-afbc-13b49ba1d705" width="300">

<img src="https://github.com/user-attachments/assets/550dd9a7-6823-45a1-bb5a-98af24b5096d" width="300">

<img src="https://github.com/user-attachments/assets/a4777ba1-5507-4316-b382-6f416a26fb23" width="300">

# Réseau
Create VNET 
<img src="https://github.com/user-attachments/assets/ff41ecea-a3f3-4c69-8b9e-cea5fc427ae7" width="300">

<img src="https://github.com/user-attachments/assets/7b3e25d9-89bd-4bea-a7ca-04aad91e08f8" width="300">
<img src="https://github.com/user-attachments/assets/7eceaaac-b567-4f79-975b-d98d61d020e7" width="300">

<img src="https://github.com/user-attachments/assets/25281224-cce8-4799-8b29-697db6e77749" width="300">
<img src="https://github.com/user-attachments/assets/1eff4f78-6dd5-4a9c-bd1e-b5197f71f736" width="300">


Create Spoke pour VMs
<img src="https://github.com/user-attachments/assets/bed1c7bc-719a-4ed0-a6b9-d2b3b53183b9" width="300">

Def Subnets 
<img src="https://github.com/user-attachments/assets/389996f5-b41d-4fea-80da-86c8cac5d0c3" width="300">

Define Peerings
<img src="https://github.com/user-attachments/assets/1d9cd631-e602-4a27-ba18-50559cc68a4d" width="300">
<img src="https://github.com/user-attachments/assets/e5b34aef-d0fc-45de-b02d-abc60c9ce7b6" width="300">

<img src="https://github.com/user-attachments/assets/66c6897f-7121-456f-a3e2-3f6a73e3247c" width="300">
<img src="https://github.com/user-attachments/assets/705f13b0-d913-4215-9768-a3832d31a3fc" width="300">

Repeat steps Spoke for private link endpoints

<img src="https://github.com/user-attachments/assets/acd32b71-d81c-4c6f-a44c-9ebdc6238a8c" width="300">
<img src="https://github.com/user-attachments/assets/e2cc4ccb-d9a6-4d72-ba70-e5f0265cc41f" width="300">
<img src="https://github.com/user-attachments/assets/4608f9eb-5622-4a21-b24c-b6baac2fedda" width="300">
<img src="https://github.com/user-attachments/assets/2641d2bf-00c2-4ee1-8ade-1716dffe2dc2" width="300">
<img src="https://github.com/user-attachments/assets/4c1c97e2-a4dd-4c9a-be3a-f2e30cb2a674" width="300">

DNS Links
<img src="https://github.com/user-attachments/assets/acbfcccc-3010-497e-8b4f-aaa551939c04" width="300">

Create VM 
<img src="https://github.com/user-attachments/assets/8a8c9266-470a-4f25-9642-b81d0d4298c8" width="300">
<img src="https://github.com/user-attachments/assets/0b119e98-0aed-4b85-9371-d0319e55900d" width="300">
<img src="https://github.com/user-attachments/assets/797a0098-62cc-4d23-9398-db9e08182c6f" width="300">
<img src="https://github.com/user-attachments/assets/463a8ea6-29ad-4765-8096-40b801e62846" width="300">
<img src="https://github.com/user-attachments/assets/0b8793eb-2357-4063-a371-f58a53bb69e2" width="300">

<img src="https://github.com/user-attachments/assets/588aca89-8702-4d63-9435-9df110468129" width="300">


# All in One : 

Create VNET 
<img src="https://github.com/user-attachments/assets/2355d410-83c6-4711-9c69-5134efa75b6a" width="300">
With Bastion
<img src="https://github.com/user-attachments/assets/e7170bdb-dd44-40ed-b6fe-3738a641e106" width="300">
Define Subnets
<img src="https://github.com/user-attachments/assets/48b68d16-2f6b-4607-908d-bf8c6e09c2f8" width="300">
Create

Create VM 
<img src="https://github.com/user-attachments/assets/d04620bb-1cce-4764-8185-85198e3439f1" width="300">
<img src="https://github.com/user-attachments/assets/1a7a8bbe-6c90-47eb-9f5d-00fd91b7a506" width="300">
<img src="https://github.com/user-attachments/assets/954cd52f-28b7-457e-9005-839e20162fba" width="300">
<img src="https://github.com/user-attachments/assets/8a30b3af-460f-4ac1-bb69-63d73cf4c345" width="300">


Create PE 
<img src="https://github.com/user-attachments/assets/9ed19018-0238-40d5-9ce9-e89f62d761fe" width="300">
<img src="https://github.com/user-attachments/assets/e5b272d2-3118-4fea-9be5-655170cb8edc" width="300">
<img src="https://github.com/user-attachments/assets/63ee729e-056e-47b3-8043-3bcac9c7780d" width="300">

Connect via Bastion 

Ns lookup : 
<img src="https://github.com/user-attachments/assets/528c56c5-54a0-43e6-8998-eb5dad5cefb3" width="300">

Test desac
<img src="https://github.com/user-attachments/assets/8e65693c-b43a-4f8b-9186-d691f3f76f73" width="300">
<img src="https://github.com/user-attachments/assets/8c3ae0d1-2626-4f4f-be85-97356105645d" width="300">
<img src="https://github.com/user-attachments/assets/aef0c1ea-e111-4771-b57a-9420f2bac3ec" width="300">

Depuis le poste : 
<img src="https://github.com/user-attachments/assets/aad07028-6c18-41ad-80cd-cfbe7529ab2a" width="300">

Create SA : 
<img src="https://github.com/user-attachments/assets/967bc46a-c23c-48f0-a285-430ec8f39bc8" width="300">
<img src="https://github.com/user-attachments/assets/63a69146-5ef2-434c-aae4-9ae169d8364f" width="300">
<img src="https://github.com/user-attachments/assets/a13ae6b9-47ee-4cb2-868f-22052cbbecbf" width="300">
<img src="https://github.com/user-attachments/assets/7abcac83-4384-4aa9-8764-102ba547e8cf" width="300">

Create wks :
<img src="https://github.com/user-attachments/assets/c24cd867-8b60-432b-a6d6-98e0f1979bf9" width="300">

```/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Storage/storageAccounts/{storage-account-name}```

```/subscriptions/ecaa47e5-79d1-47e4-ac93-80b1484c3dbf/resourceGroups/rg-private-fabric-all-in/providers/Microsoft.Storage/storageAccounts/safabricprivate```
<img src="https://github.com/user-attachments/assets/afdf748a-13a8-4bf9-8567-6e3173109ce4" width="300">
<img src="https://github.com/user-attachments/assets/4d1f3787-4515-49b0-ab56-367d308bc397" width="300">
<img src="https://github.com/user-attachments/assets/ced69224-96ba-422a-a683-e99af82ed63a" width="300">
<img src="https://github.com/user-attachments/assets/68649b63-1378-42cb-93d3-47d51d13604b" width="300">

<img src="https://github.com/user-attachments/assets/c6557c00-72a7-4118-887b-118fc68f1034" width="300">
<img src="https://github.com/user-attachments/assets/c1359d7e-b59a-4308-9df3-38780dcb11d9" width="300">
<img src="https://github.com/user-attachments/assets/b765fe16-f328-4934-9a71-de26299bd597" width="300">

<img src="https://github.com/user-attachments/assets/3c4b5b94-e748-4231-8996-836927eaec84" width="300">
<img src="https://github.com/user-attachments/assets/97cca32a-8cb0-40de-bf04-d41621b245db" width="300">


# Roles 
<img src="https://github.com/user-attachments/assets/2165a8ac-411d-4e5d-bd6c-d58c0023824c" width="300">
<img src="https://github.com/user-attachments/assets/2aa9286a-03ad-4e19-a647-d8319a586dfb" width="300">

# Key Vault
<img src="https://github.com/user-attachments/assets/d6136e47-4c03-4fa3-a011-9fadad19517d" width="300">
<img src="https://github.com/user-attachments/assets/efa8ce05-e461-4602-a57d-62597a8baa72" width="300">
<img src="https://github.com/user-attachments/assets/ada19118-87ae-45dc-a7db-e6ffac0ce9dc" width="300">
<img src="https://github.com/user-attachments/assets/893f2e18-2156-4fd3-a90a-b30f7a54a546" width="300">

```/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}```
```/subscriptions/ecaa47e5-79d1-47e4-ac93-80b1484c3dbf/resourceGroups/rg-private-fabric-all-in/providers/Microsoft.KeyVault/vaults/kv-private-gennaker```

<img src="https://github.com/user-attachments/assets/81b4726b-42e7-45cf-b26c-ee9ff2cfe0fd" width="300">
<img src="https://github.com/user-attachments/assets/0c252fd4-f053-490c-87c2-9a496f807e1f" width="300">
<img src="https://github.com/user-attachments/assets/cf7cc421-399c-40f3-80b7-398a07f8c848" width="300">
<img src="https://github.com/user-attachments/assets/166f30b7-879c-413c-b71c-7c869b7da732" width="300">
<img src="https://github.com/user-attachments/assets/a2682b80-86b0-45fc-8fd6-ee1c3648467f" width="300">
<img src="https://github.com/user-attachments/assets/44141c0c-f1e7-488b-acb5-26806ee7fb69" width="300">
<img src="https://github.com/user-attachments/assets/d9fa770d-a26c-4dfb-8305-58a0d60f6777" width="300">
<img src="https://github.com/user-attachments/assets/30ea44ae-4430-4a93-85dd-c173af572cd2" width="300">
<img src="https://github.com/user-attachments/assets/04da8163-6f38-4182-bbeb-39e3a3f5ca41" width="300">

Create key 
<img src="https://github.com/user-attachments/assets/a509871c-1e50-4a61-bd40-50d3f7cd1584" width="300">

<img src="https://github.com/user-attachments/assets/f3063673-09e1-4511-9a58-78be4d3c1385" width="300">
<img src="https://github.com/user-attachments/assets/595294b8-4783-41d1-aee8-e4faad48622d" width="300">
