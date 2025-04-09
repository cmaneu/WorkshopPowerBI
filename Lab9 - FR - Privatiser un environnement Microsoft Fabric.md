# Introduction & Architecture Générale

1.
2. 
3.
4.

# Pré requis

# Pas à pas 

Enable Private Links
![Capture d'écran 2025-03-12 111730](https://github.com/user-attachments/assets/4fcba74a-4e9e-4aff-b505-18f8c48cb04f)

Create RG
![image](https://github.com/user-attachments/assets/3a2f0f70-98b0-4bee-889b-f590af92e954)
![Capture d'écran 2025-03-12 111848](https://github.com/user-attachments/assets/ef9ced03-cb39-47ce-9876-d7cadd99b1b5)

Once created click 
![Capture d'écran 2025-03-12 111936](https://github.com/user-attachments/assets/796576ae-db3a-4b57-b5df-62234418f900)


In RG, create item 
![Capture d'écran 2025-03-12 112225](https://github.com/user-attachments/assets/9c076b8e-e2d5-4eb5-b71f-43469ba176d0)

Create Template 
![Capture d'écran 2025-03-12 112308](https://github.com/user-attachments/assets/3dd14a61-365e-4f5b-85d9-c15de5bec98b)

Change variables 
![Capture d'écran 2025-03-12 112459](https://github.com/user-attachments/assets/4d72d13c-183e-436c-8328-205af0a96310)

Get About Pbi 
![Capture d'écran 2025-03-12 112540](https://github.com/user-attachments/assets/eda6e8f3-7528-4452-acbd-5e5cd6cccd8c)
![Capture d'écran 2025-03-12 112558](https://github.com/user-attachments/assets/02293db2-0d6b-4358-afbc-13b49ba1d705)

![Capture d'écran 2025-03-12 112714](https://github.com/user-attachments/assets/550dd9a7-6823-45a1-bb5a-98af24b5096d)

![Capture d'écran 2025-03-12 113704](https://github.com/user-attachments/assets/a4777ba1-5507-4316-b382-6f416a26fb23)

# Réseau
Create VNET 
![Capture d'écran 2025-03-12 123900](https://github.com/user-attachments/assets/ff41ecea-a3f3-4c69-8b9e-cea5fc427ae7)

![Capture d'écran 2025-03-12 123932](https://github.com/user-attachments/assets/7b3e25d9-89bd-4bea-a7ca-04aad91e08f8)
![Capture d'écran 2025-03-12 124049](https://github.com/user-attachments/assets/7eceaaac-b567-4f79-975b-d98d61d020e7)

![Capture d'écran 2025-03-12 124124](https://github.com/user-attachments/assets/25281224-cce8-4799-8b29-697db6e77749)
![Capture d'écran 2025-03-12 124417](https://github.com/user-attachments/assets/1eff4f78-6dd5-4a9c-bd1e-b5197f71f736)


Create Spoke pour VMs
![Capture d'écran 2025-03-12 142824](https://github.com/user-attachments/assets/bed1c7bc-719a-4ed0-a6b9-d2b3b53183b9)

Def Subnets 
![Capture d'écran 2025-03-12 142914](https://github.com/user-attachments/assets/389996f5-b41d-4fea-80da-86c8cac5d0c3)

Define Peerings
![Capture d'écran 2025-03-12 143316](https://github.com/user-attachments/assets/1d9cd631-e602-4a27-ba18-50559cc68a4d)
![image](https://github.com/user-attachments/assets/e5b34aef-d0fc-45de-b02d-abc60c9ce7b6)

![image](https://github.com/user-attachments/assets/66c6897f-7121-456f-a3e2-3f6a73e3247c)
![image](https://github.com/user-attachments/assets/705f13b0-d913-4215-9768-a3832d31a3fc)

Repeat steps Spoke for private link endpoints

![image](https://github.com/user-attachments/assets/acd32b71-d81c-4c6f-a44c-9ebdc6238a8c)
![image](https://github.com/user-attachments/assets/e2cc4ccb-d9a6-4d72-ba70-e5f0265cc41f)
![image](https://github.com/user-attachments/assets/4608f9eb-5622-4a21-b24c-b6baac2fedda)
![image](https://github.com/user-attachments/assets/2641d2bf-00c2-4ee1-8ade-1716dffe2dc2)
![image](https://github.com/user-attachments/assets/4c1c97e2-a4dd-4c9a-be3a-f2e30cb2a674)

DNS Links
![image](https://github.com/user-attachments/assets/acbfcccc-3010-497e-8b4f-aaa551939c04)

Create VM 
![image](https://github.com/user-attachments/assets/8a8c9266-470a-4f25-9642-b81d0d4298c8)
![image](https://github.com/user-attachments/assets/0b119e98-0aed-4b85-9371-d0319e55900d)
![image](https://github.com/user-attachments/assets/797a0098-62cc-4d23-9398-db9e08182c6f)
![image](https://github.com/user-attachments/assets/463a8ea6-29ad-4765-8096-40b801e62846)
![image](https://github.com/user-attachments/assets/0b8793eb-2357-4063-a371-f58a53bb69e2)

![image](https://github.com/user-attachments/assets/588aca89-8702-4d63-9435-9df110468129)


# All in One : 

Create VNET 
![image](https://github.com/user-attachments/assets/2355d410-83c6-4711-9c69-5134efa75b6a)
With Bastion
![image](https://github.com/user-attachments/assets/e7170bdb-dd44-40ed-b6fe-3738a641e106)
Define Subnets
![image](https://github.com/user-attachments/assets/48b68d16-2f6b-4607-908d-bf8c6e09c2f8)
Create

Create VM 
![image](https://github.com/user-attachments/assets/d04620bb-1cce-4764-8185-85198e3439f1)
![image](https://github.com/user-attachments/assets/1a7a8bbe-6c90-47eb-9f5d-00fd91b7a506)
![image](https://github.com/user-attachments/assets/954cd52f-28b7-457e-9005-839e20162fba)
![image](https://github.com/user-attachments/assets/8a30b3af-460f-4ac1-bb69-63d73cf4c345)


Create PE 
![image](https://github.com/user-attachments/assets/9ed19018-0238-40d5-9ce9-e89f62d761fe)
![image](https://github.com/user-attachments/assets/e5b272d2-3118-4fea-9be5-655170cb8edc)
![image](https://github.com/user-attachments/assets/63ee729e-056e-47b3-8043-3bcac9c7780d)

Connect via Bastion 

Ns lookup : 
![image](https://github.com/user-attachments/assets/528c56c5-54a0-43e6-8998-eb5dad5cefb3)

Test desac
![image](https://github.com/user-attachments/assets/8e65693c-b43a-4f8b-9186-d691f3f76f73)
![image](https://github.com/user-attachments/assets/8c3ae0d1-2626-4f4f-be85-97356105645d)
![image](https://github.com/user-attachments/assets/aef0c1ea-e111-4771-b57a-9420f2bac3ec)

Depuis le poste : 
![image](https://github.com/user-attachments/assets/aad07028-6c18-41ad-80cd-cfbe7529ab2a)

Create SA : 
![image](https://github.com/user-attachments/assets/967bc46a-c23c-48f0-a285-430ec8f39bc8)
![image](https://github.com/user-attachments/assets/63a69146-5ef2-434c-aae4-9ae169d8364f)
![image](https://github.com/user-attachments/assets/a13ae6b9-47ee-4cb2-868f-22052cbbecbf)
![image](https://github.com/user-attachments/assets/7abcac83-4384-4aa9-8764-102ba547e8cf)

Create wks :
![image](https://github.com/user-attachments/assets/c24cd867-8b60-432b-a6d6-98e0f1979bf9)
```/subscriptions/ecaa47e5-79d1-47e4-ac93-80b1484c3dbf/resourceGroups/rg-private-fabric-all-in/providers/Microsoft.Storage/storageAccounts/safabricprivate```
![image](https://github.com/user-attachments/assets/afdf748a-13a8-4bf9-8567-6e3173109ce4)
![image](https://github.com/user-attachments/assets/4d1f3787-4515-49b0-ab56-367d308bc397)
![image](https://github.com/user-attachments/assets/ced69224-96ba-422a-a683-e99af82ed63a)
![image](https://github.com/user-attachments/assets/68649b63-1378-42cb-93d3-47d51d13604b)

![image](https://github.com/user-attachments/assets/c6557c00-72a7-4118-887b-118fc68f1034)
![image](https://github.com/user-attachments/assets/c1359d7e-b59a-4308-9df3-38780dcb11d9)
![image](https://github.com/user-attachments/assets/b765fe16-f328-4934-9a71-de26299bd597)

![image](https://github.com/user-attachments/assets/3c4b5b94-e748-4231-8996-836927eaec84)
![image](https://github.com/user-attachments/assets/97cca32a-8cb0-40de-bf04-d41621b245db)


Roles 
![image](https://github.com/user-attachments/assets/2165a8ac-411d-4e5d-bd6c-d58c0023824c)
![image](https://github.com/user-attachments/assets/2aa9286a-03ad-4e19-a647-d8319a586dfb)
