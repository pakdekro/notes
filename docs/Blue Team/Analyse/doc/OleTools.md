https://github.com/decalage2/oletools

**OLETools** est une suite d'outils Python permettant d'analyser les fichiers OLE (Object Linking and Embedding), les fichiers au format MS Office (Word, Excel, etc.), les fichiers RTF, et d'identifier des éléments suspects ou malveillants. Il est souvent utilisé en analyse de malware et en recherche de menaces (threat hunting). Chaque outil a une fonction spécifique pour l'extraction de métadonnées, de macros, de liens et d'autres artefacts.

### Installation

`pip install oletools`

### Outils Principaux

1. **olevba**
    
    - **Description** : Analyse les fichiers OLE (Word, Excel) pour extraire et analyser les macros VBA.
    - **Usage** :
        `olevba <fichier>`
    - **Options principales** :
        - `-c` : Affiche le code complet des macros.
        - `-a` : Extrait et analyse automatiquement les macros.
        - `-r` : Recherche des chaînes intéressantes comme les URL, chemins de fichiers, et autres.
        - `-d` : Mode débogage avec plus de détails.
    - **Exemple** :
        `olevba -c maldoc.doc`
2. **oleid**
    
    - **Description** : Identifie les indicateurs de présence de contenu suspect dans les fichiers OLE.
    - **Usage** :
        `oleid <fichier>`
    - **Exemple** :
        `oleid maldoc.doc`
    - **Indicateurs analysés** :
        - Présence de macros.
        - Formules Excel cachées.
        - Lien OLE/ActiveX.
        - Propriétés de document anormales.
3. **rtfobj**
    
    - **Description** : Extrait et analyse les objets OLE incorporés dans les fichiers RTF.
    - **Usage** :
        `rtfobj <fichier.rtf>`
    - **Exemple** :
        `rtfobj maldoc.rtf`
    - **Options principales** :
        - `-s` : Recherche de chaînes de caractères spécifiques.
        - `-O <directory>` : Spécifie le répertoire de sortie pour les objets extraits.
4. **olemeta**
    
    - **Description** : Extrait les métadonnées des fichiers OLE (par ex., auteur, date de création, modification).
    - **Usage** :
        `olemeta <fichier>`
    - **Exemple** :
        `olemeta document.doc`
5. **mraptor**
    
    - **Description** : Détecte rapidement les macros suspectes dans les fichiers Office.
    - **Usage** :
        `mraptor <fichier>`
    - **Options principales** :
        - `-d` : Active le mode de débogage.
        - `-s` : Désactive la vérification des chemins de fichiers.
    - **Exemple** :
        `mraptor rapport.ls`
6. **oledump**
    
    - **Description** : Analyse les fichiers OLE pour extraire des flux d'objets incorporés et permet de manipuler les données.
    - **Usage** :
        `oledump.py <fichier>`
    - **Options principales** :
        - `-s <stream_number>` : Sélectionne le flux à afficher ou à analyser.
        - `-p` : Analyse en mode Plugin pour des signatures spécifiques.
    - **Exemple** :
        `oledump.py -s 1 malware.doc`
7. **pyxswf**
    
    - **Description** : Extrait et analyse les objets Flash (SWF) présents dans les fichiers OLE.
    - **Usage** :
        `pyxswf <fichier>`
    - **Exemple** :
        `pyxswf presentation.ppt

### Cas d'utilisation

- **Vérification rapide de macros suspectes** :
    `olevba -a suspect_file.xls`
- **Extraction de métadonnées** :
    `olemeta secret_file.doc`
- **Analyse d'un document RTF** :
    `rtfobj suspicious_document.rtf`

### Commandes Utiles de Débogage

- **Afficher toutes les macros présentes dans un fichier** :
    `olevba -c malware.doc`
- **Identifier les indicateurs suspects dans un fichier OLE** :
    `oleid targeted_file.doc`
