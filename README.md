# 🏭 ASIX COMUNITY — Automatització de projectes amb IA

> ⚠️ **FASE BETA — Projecte públic per a data testing**
> Aquest projecte està en desenvolupament actiu. Els resultats generats per la IA **no són definitius ni precisos al 100%**. Llegeix la secció [Advertències](#️-advertències-importants) abans de continuar.

---

## 📌 Què és n8n-Start-Project?

n8n-Start-Project és un sistema d'automatització basat en **n8n** que, a partir d'un formulari i el PDF d'un enunciat d'un projecte en equip, genera automàticament:

- 📁 **Carpeta de projecte** (Google Drive)
- 📄 **Plantilla de documentació generica** (Google Docs)
- 📄 **Acords de projecte** (Google Docs)
- 🎯 **Objectius** (Google Docs)
- 📊 **Pla de treball** (Google Sheets)

Tot el procés utilitza IA (via **OpenRouter**) per analitzar l'enunciat i omplir les plantilles corresponents. Un segon workflow de suport s'encarrega de notificar errors per **Discord** i **Gmail**.

El projecte està pensat principalment per a cicles formatius d'ASIX i DAW/DAM. L'objectiu és que els usuaris voluntaris facin data testing i ajudin a millorar els prompts del sistema per fer-lo cada vegada més precís i flexible. L'objectiu d'aquesta automatització se centra, sobretot, en aquells estudiants o coordinadors que volen estalviar temps obtenint una versió en brut del projecte; d'aquesta manera, només hauran de revisar el resultat de la IA i polir els arxius generats a la nova carpeta. Si la intenció inicial era generar el projecte i lliurar-lo directament al Moodle, aquesta no és la millor opció, ja que la IA pot tenir algunes al·lucinacions o el text pot resultar més o menys informal, segons el gust de l'usuari. Per tant, la recomanació és actuar amb responsabilitat i seny: utilitzeu-lo per tenir un esborrany que us estalviï temps i, a partir d'aquí, reviseu i sintonitzeu el contingut generat.

---

## ⚠️ Advertències importants

- 🔴 El model de IA **no està en la seva versió final**. Els acords, objectius i pla de treball generats poden contenir errors, omissions o imprecisions.
- 🔴 **No utilitzis els documents generats directament** sense revisar-los prèviament.
- 🟢 El teu feedback (bones i males generacions) ajuda directament a millorar el `prompt_user` i el `prompt_system` per a tothom.

---

## 📁 Estructura del repositori

```
/
├── README.md                        ← Aquest fitxer
├── workflows/
│   ├── ASIX_Start_Project_v1.json   ← Workflow principal
│   └── ASIX_Log_error_v1.json       ← Workflow de gestió d'errors
└── resources/
│   ├── plantilla_acords.docx   ← plantilla acords
│   ├── plantilla_objectius.docx   ← plantilla objetius
│   └── plantilla_pla_treball.xlsx   ← plantilla pla de treball
└── docs/
│   ├── DriveSetup.md   ← documentació drive
│   └── Formulari.md   ← documentació Formulari

```
---

## 🗺️ Arquitectura general

```
Formulari (Google Forms / n8n Form)
        │
        ▼
[n8n] ASIX Start Project v1
        │
        ├── Crea carpeta al Drive
        ├── Copia plantilles (Acords, Objectius, Pla Treball)
        ├── Descarrega el PDF de l'enunciat
        ├── Extreu el text del PDF
        ├── Crida a la IA via OpenRouter
        ├── Parseja la resposta JSON
        └── Omple els documents amb batchUpdate (Docs API + Sheets API)
                │
                └── Notificació Discord (Log)
                        │
                        └── [n8n] ASIX Log Error v1 (en cas d'error)
```

<img width="1510" height="532" alt="image" src="https://github.com/user-attachments/assets/2a01e156-4907-4c03-902c-e1fa1f3c54ef" />

---

## 🛠️ Requisits previs

Abans d'importar els workflows necessites tenir preparat:

| Requisit | Descripció |
|---|---|
| **n8n** en local o Docker | Motor d'automatització |
| **API Key d'OpenRouter** | Per fer les crides a la IA |
| **Projecte a Google Cloud** | Per obtenir les credencials OAuth2 |
| **Bot de Discord** | Per rebre notificacions d'error |
| **Compte de Gmail** | Per rebre notificacions d'error per correu |

---

## 🚀 Instal·lació ràpida

### 1. Aixecar n8n amb Docker

Si no tens n8n instal·lat, pots arrencar-lo fàcilment amb Docker:

```bash
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

Un cop arrencat, accedeix a [http://localhost:5678](http://localhost:5678) i crea el teu compte d'administrador.

> 💡 Si vols que les dades persisteixin entre reinicis, el paràmetre `-v ~/.n8n:/home/node/.n8n` ja ho fa. Pots canviar `~/.n8n` per la ruta que vulguis.

<img width="815" height="98" alt="image" src="https://github.com/user-attachments/assets/0d46ec9f-8110-4080-a7ac-40db89b65122" />

---

### 2. Obtenir la API Key d'OpenRouter

1. Ves a [https://openrouter.ai](https://openrouter.ai) i crea un compte.
2. Accedeix a **Keys** i genera una nova API Key.
3. A n8n, crea una credencial de tipus **Header Auth** amb:
   - **Name:** `Authorization`
   - **Value:** `Bearer <la-teva-api-key>`

<img width="1862" height="169" alt="image" src="https://github.com/user-attachments/assets/4dff6f47-a180-47b8-807d-9ecee1ea3f3d" /> 

---

### 3. Configurar Google Cloud (OAuth2)

Necessites un projecte a Google Cloud per obtenir el **Client ID** i el **Client Secret** que s'usen a les credencials de Drive, Docs, Sheets i Gmail.

1. Ves a [https://console.cloud.google.com](https://console.cloud.google.com) i crea un projecte nou.
2. Activa les següents **APIs** al teu projecte:
   - ✅ Google Drive API
   - ✅ Google Docs API
   - ✅ Google Sheets API
   - ✅ Gmail API
3. Ves a **APIs i serveis → Credencials → Crear credencials → ID de client OAuth**.
4. Selecciona **Aplicació web** com a tipus.
5. Afegeix com a **URI de redirecció autoritzada**:
   
   ```
   http://localhost:5678/rest/oauth2-credential/callback
   ```
   
7. Copia el **Client ID** i el **Client Secret** generats.
8. A n8n, crea les credencials corresponents (Google Drive, Google Docs, Google Sheets, Gmail) introduint el Client ID i Secret.

> ⚠️ La primera vegada que executis el workflow, n8n et demanarà autoritzar l'accés al teu compte de Google per a cada credencial.

---

### 4. Credencial genèrica per al node "Omplir doc Pla Treball"

El node que omple el Pla de Treball utilitza una **credencial genèrica OAuth2** (tipus `oAuth2Api`) en lloc d'una credencial específica de Sheets. Això és necessari perquè fa servir la Sheets API directament via HTTP Request.

Per crear-la a n8n:
1. Ves a **Credencials → Nova credencial → OAuth2 API**.
2. Configura-la amb:
   - **Authorization URL:** `https://accounts.google.com/o/oauth2/auth`
   - **Access Token URL:** `https://oauth2.googleapis.com/token`
   - **Client ID:** (el mateix que has obtingut a Google Cloud)
   - **Client Secret:** (el mateix)
   - **Scope:** `https://www.googleapis.com/auth/spreadsheets`
   - **Auth URI Query Parameters:** `access_type=offline&prompt=consent`
3. Desa i autoritza.

<img width="1085" height="681" alt="Sin nombre" src="https://github.com/user-attachments/assets/55611345-14ea-40c4-92cb-726a7d1bb7c8" />

---

### 5. Configurar el Bot de Discord

1. Ves al [Discord Developer Portal](https://discord.com/developers/applications) i crea una nova aplicació.
2. Ves a **Bot** i genera un token.
3. Afegeix el bot al teu servidor amb permisos de lectura i enviament de missatges.
4. A n8n, crea una credencial de tipus **Discord Bot** amb el token.
5. Al workflow **ASIX Log Error**, configura el node Discord amb el teu **Guild ID** i **Channel ID** del canal on vols rebre les alertes. 

<img width="1852" height="479" alt="image" src="https://github.com/user-attachments/assets/cde45e29-b1c4-43e6-8b45-7f34a0352739" />


---

### 6. Importar els workflows

1. A n8n, ves a **Workflows → Import from file**.
2. Importa primer `ASIX_Log_error_v1.json`.
3. Importa després `ASIX_Start_Project_v1.json`.
4. Revisa i assigna totes les credencials als nodes corresponents.
5. Activa ambdós workflows.

<img width="318" height="355" alt="image" src="https://github.com/user-attachments/assets/8a39c447-4264-49cf-b4ad-c016c67ea861" />


---

## 🤝 Com contribuir al projecte (data testing)

El projecte millora gràcies als usuaris que el proven. Si ets alumne o company i vols ajudar:

1. **Prova el workflow** amb enunciats reals de projectes.
2. **Revisa els documents generats** i anota on s'equivoca la IA.
3. **Reporta els resultats**: bons i dolents. Cada cas ajuda a afinar el `prompt_system` i el `prompt_user`.
4. Si tens coneixements de prompting, proposa millores directament.

> Tot feedback és benvingut, tant si el resultat és excel·lent com si és un desastre total.
> correu de contacte: ionutantonioardelean@gmail.com 
