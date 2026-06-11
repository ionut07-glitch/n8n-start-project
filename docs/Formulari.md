# 📋 Formulari — Asix Start Project

El workflow s'activa quan es rep una resposta del formulari de Google Forms. A continuació es detallen tots els camps que conté, el seu format esperat i si són obligatoris o no.

> 💡 **Com obtenir el formulari:** No és possible importar un Google Form directament. Demana accés com a editor al formulari original, fes-ne una còpia des del teu Drive i vincula-la al teu workflow n8n.

---

## Camps del formulari

| # | Nom del camp | Tipus | Obligatori | Format esperat / Exemple |
|---|---|---|---|---|
| 1 | **Enunciat del projecte (PDF)** | Fitxer (Drive) | ✅ Sí | PDF pujat al Drive. El formulari mostra un selector de fitxers de Google Drive |
| 2 | **Hores totals de classe per al projecte** | Resposta breu | No | Número enter. Ex: `40` |
| 3 | **Noms dels membres (separats per comes)** | Resposta llarga | No | Ex: `Joan García, Maria López, Pere Mas` |
| 4 | **Assignació de rols del grup** | Resposta llarga | No | `Coordinador: Nom, Corrector: Nom, Unificador: Nom, Recordador: Nom, Secretari: Nom, Normalitzador: Nom, Multimèdia: Nom` |
| 5 | **Adreces de contacte dels membres** | Resposta llarga | No | `Nom - correu@exemple.com - telèfon (opcional)` |
| 6 | **Nom i codi del mòdul** | Resposta breu | No | Ex: `369 - Implantació de Sistemes Operatius` |
| 7 | **Número de grup** | Resposta breu | No | Ex: `3` |
| 8 | **Informació addicional i peticions per a la IA** | Resposta llarga | No | Text lliure. Indicacions extra que es passaran directament al prompt de la IA |
| 9 | **Nom carpeta del projecte** | Resposta breu | ✅ Sí | Ex: `G3_MP07_ProjecteXarxa`. Serà el nom de la carpeta creada al Drive |

---

## Notes importants

- El camp **Enunciat del projecte (PDF)** utilitza un selector de fitxers de Google Drive (no una pujada directa). L'alumne ha de tenir el PDF ja pujat al Drive i seleccionar-lo des del formulari. Assegura't que la carpeta de Drive associada al formulari tingui els permisos correctes.

- El camp **Nom carpeta del projecte** és crític: és el valor que utilitza el workflow per crear la carpeta al Drive i per nomenar les còpies de les plantilles (`nom_acords`, `nom_objectius`, `nom_pla_treball`). Recomana als alumnes usar un format consistent, per exemple `G{numero}_{codi_modul}_{nom_curt}`.

- El camp **Informació addicional** s'injecta directament al prompt de la IA. Els alumnes poden usar-lo per indicar coses com: *"El projecte és en parelles"*, *"Tenim 3 setmanes en comptes de 5"*, o qualsevol restricció específica de l'enunciat que vulguin que la IA tingui en compte.

- Els camps no obligatoris **sí que afecten la qualitat de la generació**. Com més informació s'ompli, millor serà el resultat dels documents generats per la IA.
