## 📂 Configuració de Google Drive — Plantilles

El workflow **no crea els documents des de zero**: el que fa és **copiar plantilles** que ja tens al teu Drive i després omplir-les amb les dades generades per la IA. Per tant, és imprescindible tenir les plantilles correctament preparades abans de la primera execució.

### Estructura de carpetes recomanada

Has de tenir una carpeta al teu Drive (per exemple `ASIX_Factory_Plantilles`) on guardes totes les plantilles. Dins trobaràs:

```
ASIX_Plantilles/
├── plantilla_acords        ← Google Docs
├── plantilla_objectius     ← Google Docs
└── plantilla_pla_treball   ← Google Sheets
```

> Els IDs d'aquestes plantilles és el que has de configurar als nodes `Copiar plantilla Acords`, `Copiar plantilla Objectius` i `Copiar plantilla Pla Treball` del workflow.

---

### ⚠️ Format natiu de Google — Molt important

Les plantilles **han d'estar en format natiu de Google Drive**, és a dir:

- ✅ **Google Docs** (no `.docx`)
- ✅ **Google Sheets** (no `.xlsx`)

Això és perquè n8n utilitza l'API de Google per copiar els fitxers. Si les plantilles són fitxers de Microsoft Office pujats al Drive, la còpia no funcionarà correctament o el `batchUpdate` posterior fallarà.

Si tens una plantilla en `.docx` o `.xlsx`, obre-la al Drive i fes **Fitxer → Desa com a Google Docs/Sheets** per convertir-la.

---

### 🔖 Com funcionen els placeholders

Les plantilles utilitzen **placeholders** (marcadors de posició) que la IA substitueix amb el contingut real. Un placeholder és simplement un text fix dins del document que el workflow busca i reemplaça.

Per exemple, a `plantilla_acords` pots tenir:

```
Nom del projecte: [NOM_PROJECTE]
Membres del grup: [MEMBRES]
Data d'inici:     [DATA_INICI]
```

Quan el workflow executa el node `Omplir doc Acords`, fa una crida `batchUpdate` a l'API de Google Docs que busca cada placeholder i el substitueix pel valor corresponent generat per la IA.

**Regles importants dels placeholders:**

- Han de ser **únics** dins del document. Si el mateix placeholder apareix dues vegades, l'API substituirà totes les ocurrències, cosa que pot ser útil (per exemple, el nom del projecte a la capçalera i al peu) o problemàtica si no és intencionat.
- Respecta exactament la **capitalització i els caràcters especials**. `{{NOM_PROJECTE}}` i `{{nom_projecte}}` són placeholders diferents.
- **No facis servir formats especials** (negreta, color, mida gran) directament sobre el text del placeholder. Aplica el format a un paràgraf o caràcter adjacent, perquè quan l'API substitueix el text pot perdre el format del placeholder original.
- Per al **Pla de Treball** (Google Sheets), els placeholders no funcionen igual: el workflow escriu directament a cel·les concretes per posició (fila i columna), no per cerca de text.
