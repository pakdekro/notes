[https://github.com/Yamato-Security/hayabusa](https://github.com/Yamato-Security/hayabusa)

**Hayabusa est un outil développé par le groupe Yamato Security** au Japon pour générer rapidement des chronologies forensiques à partir des journaux d'événements Windows, facilitant ainsi la chasse aux menaces. 

Le nom "Hayabusa", qui signifie "faucon pèlerin" en japonais, a été choisi en référence à la vitesse et à l'efficacité de cet oiseau pour la chasse. 

Cet outil, programmé en Rust, utilise le multi-threading pour maximiser sa rapidité. Nous avons aussi mis en place un moyen de convertir les règles Sigma en format compatible avec Hayabusa. 

**Ces règles de détection, rédigées en YML, sont conçues pour être aisément personnalisables et extensibles.** Hayabusa peut être utilisé pour des analyses en temps réel sur des systèmes individuels, pour des analyses hors ligne en rassemblant les journaux de plusieurs systèmes, ou même en combinaison avec Velociraptor pour une recherche de menaces et une réponse aux incidents à l'échelle de l'entreprise. 

Les résultats sont rassemblés dans un fichier CSV unique, rendant l'analyse plus aisée sur des outils comme LibreOffice, Timeline Explorer, Elastic Stack, Timesketch, etc.

**Un binaire est disponible sur Windows/Linux/MacOS.**

Export en json + html :
```bash
./hayabusa json-timeline -d /chemin/vers/evtx -o /chemin/vers/output.json -H /chemin/vers/output.html
```

Export en csv + html :
```bash
./hayabusa csv-timeline -d /chemin/vers/evtx -o /chemin/vers/output.csv -H /chemin/vers/output.html
```
