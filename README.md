# UrbanCode Delivery Demo

This repository is for demo JPetStore on IBM's UrbanCode tool using Nexus repository and Jenkins to push artifacts to UrbanCode's CodeStation repository.

- Jenkins Delivery Pipeline
- UbanCode's use of Components and Applications
- UrbanCodes snapshots
- 


# Steps

1. Run deliver_pipeline docker compose.
   - first: docker-compose.ymal
   - second: docker-compose-urbancode.yaml
1. Login into UrbanCode:
   - User: `admin`
   - Pass: `admin`
1. Install plugins for Tomcat and DBUpgrader (See references: [1], [2])


```
cd delivery_pipeline
docker-compose up -d
docker-compose -f docker-compose-urbancode up
```
- You're starting the second container in foreground due to bug being present 
- Press Enter twice to accept defaults

```
docker-compose -f docker-compose-urbancode up -d
```
- Start container in detached mode.




# Steps for Importing Components & Applications

JPetStore processes have been defined in this repository.

```
cd resources
docker cp import-jpetstore.keystore delivery_pipeline_udcs_1:/opt/ibm-ucd/server/appdata/conf
docker exec -it delivery_pipeline_udcs_1 bash
cd /opt/ibm-ucd/server/appdata/conf
```


Check alias of private key.
```
keytool -list -keystore import-jpetstore.keystore -storepass tempPassword -storetype jceks
```

Output: 
```
Keystore type: jceks
Keystore provider: IBMJCE

Your keystore contains 1 entry

aes128keyvqm6, Apr 25, 2018, SecretKeyEntry,
```

Import key
```
keytool -importkeystore \
-srckeystore import.keystore \
-srcstorepass tempPassword \
-srcstoretype jceks \
-alias aes128keyvqm6 \
-destkeystore encryption.keystore \
-deststorepass changeit \
-deststoretype jceks
```
- Type password 'changeit' 
- restart docker container

```
docker restart delivery_pipeline_udcs_1
```


``` 
keytool -list -keystore encryption.keystore -storepass changeit -storetype jceks
```


# Environments Setup

- `DEV`
- `QA` 
- `PRE-PROD`
- `PROD`


## References
1. Plugin - (Tomcat)[https://developer.ibm.com/urbancode/plugin/tomcat-ibmucd/]
1. Plugin - (DBUpgrader)[https://developer.ibm.com/urbancode/plugin/ibm-urbancode-deploy-dbupgrader-ibmucd/]
