# LAB 17 : Cracker OWASP UnCrackable Android Level 3

## Objectif du Lab

Ce laboratoire a pour objectif d’analyser et contourner les mécanismes de protection de l’application **OWASP UnCrackable Level 3** en utilisant des outils d’analyse statique et de reverse engineering Android.

---

# Outils utilisés

- Android Studio
- Jadx-GUI
- Apktool
- VS Code
- Keytool
- APK Signer
- ADB

---

# Téléchargement de l’APK

```bash
wget https://github.com/OWASP/owasp-mstg/raw/master/Crackmes/Android/Level_03/UnCrackable-Level3.apk
```

---

# Étape 1 : Décompilation de l’APK avec Apktool

Commande utilisée :

```bash
apktool d UnCrackable-Level3.apk -o uncrackable3
```

Cette commande permet de décompiler l’application afin d’accéder aux fichiers Smali et aux bibliothèques natives.

## Résultat

<img width="1117" height="350" alt="11" src="https://github.com/user-attachments/assets/dcba0966-4fc1-402d-b36c-a9b4a92a3064" />

---

# Étape 2 : Analyse du fichier MainActivity

Le fichier suivant a été analysé :

```text
uncrackable3/smali/sg/vantagepoint/uncrackable3/MainActivity.smali
```

Les protections détectées :

- Root Detection
- Tamper Detection
- Debugger Detection

Le bloc suivant provoquait la fermeture de l’application :

```java
showDialog("Rooting or tampering detected.");
```

## Analyse du code
<img width="1222" height="937" alt="10" src="https://github.com/user-attachments/assets/8898ddca-8f6f-4ae0-b88d-00add0b2fe26" />


---

# Étape 3 : Patch des protections

Les conditions de détection ont été modifiées dans le fichier Smali.

Modification effectuée :

```smali
if-nez v0, :cond_0
```

Remplacé par :

```smali
if-eqz v0, :cond_0
```

Cela permet de bypasser les vérifications de sécurité de l’application.

---

# Étape 4 : Recompilation de l’APK

Commande utilisée :

```bash
apktool b uncrackable3 -o patched.apk
```

Cette étape génère une nouvelle version patchée de l’application.

---

# Étape 5 : Génération de la clé de signature

Commande utilisée :

```bash
keytool -genkey -v -keystore mykey.keystore -alias mykey -keyalg RSA -keysize 2048 -validity 10000
```

Lors de cette étape, un mot de passe d’au moins 6 caractères doit être saisi.

## Génération du keystore

<img width="1470" height="327" alt="12" src="https://github.com/user-attachments/assets/48b18453-721b-4db2-9606-736d740bb420" />

---

# Étape 6 : Signature de l’APK

Commande utilisée :

```bash
apksigner sign --ks mykey.keystore patched.apk
```

---


---

---

# Conclusion

Ce laboratoire a permis de comprendre :

- Le reverse engineering Android
- L’analyse Smali
- Le fonctionnement des protections Android
- Le patching d’APK
- La recompilation et la signature d’applications Android

Ce type d’analyse est largement utilisé dans les audits de sécurité mobile et le pentesting Android.
