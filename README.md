# NutriPulse

Estimer le niveau de risque de malnutrition des enfants de 0 à 59 mois à partir de simples réponses de questionnaire, afin de décider quels ménages visiter en premier. Données : enquête MICS6 Togo 2017.


## Contenu du dossier

```
NutriPulse/
|-- README.md
|-- requirements.txt
|-- data/
|   |-- raw/      ch.sav, hh.sav, hl.sav       (fichiers bruts, jamais modifiés)
|   |-- clean/    nutripulse_clean.parquet     (fichier propre, produit par le notebook)
|                 parametres_nettoyage.json    (plafond et listes de variables)
|-- notebook/
|   |-- nettoyage_MICS.ipynb                   (le nettoyage, documenté pas à pas)
    |-- analyse.ipynb                       
|-- docs/
    |-- NutriPulse_Choix_de_nettoyage.pdf      (les choix expliqués)
    |-- NutriPulse_Architecture.pdf            (la démarche générale du projet)
```

## Relancer le nettoyage

1. Installer les bibliothèques : `pip install -r requirements.txt`
2. Ouvrir `notebook/nettoyage.ipynb` et tout exécuter.

Le fichier `data/clean/nutripulse_clean.parquet` est alors régénéré à l'identique. Si une règle de nettoyage change, on corrige le notebook et on le relance : on ne retouche jamais le fichier propre à la main.

## Le fichier propre

Une ligne par enfant de 0 à 59 mois ayant des mesures jugées fiables : **4 900 enfants**, **28 colonnes**.

- **Identifiants** : `grappe`, `menage`, `id_menage`, `ligne_enfant`
- **Cibles** : `stunt` (retard de croissance), `waste` (émaciation), valeurs 0 ou 1
- **Pour décrire (jamais en entrée)** : `stunt_niveau`, `waste_niveau` (0 normal, 1 modéré, 2 sévère), `haz`, `whz`
- **Variables communes aux deux modèles (6)** : `age_mois`, `region`, `edu_mere`, `electricite`, `cuisiniere`, `toilettes`
- **Propres au retard de croissance (4)** : `sexe_f`, `acte_naissance`, `eau_amelioree`, `animaux`
- **Propres à l'émaciation (4)** : `age_gardien`, `taille_menage`, `lavage_mains`, `telephone`
- **Poids d'enquête** : `poids_enfant` (uniquement pour les chiffres nationaux)
- **Audit d'équité (jamais en entrée)** : `milieu`, `quintile_richesse`, `ethnie`

## Ce que le nettoyage ne fait volontairement pas

Pas de remplacement des cases vides, pas de transformation des catégories en nombres, pas de changement d'échelle. Ces opérations apprennent à partir des données : elles seront faites plus tard, dans la modélisation, uniquement sur la partie d'entraînement, pour éviter toute fuite d'information.

## Suite du projet

2. Analyse exploratoire (`analyse.ipynb`)
3. Modélisation (`modelisation.ipynb`)
