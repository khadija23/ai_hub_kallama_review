# Réannotation des Transcriptions Non Vérifiées

## Description du Projet

Ce projet a été réalisé en collaboration entre **AI Hub Sénégal** et **CLAD** dans le cadre de la réannotation de transcriptions audio du corpus Kallama. L'objectif est de traiter et de réannoter les portions du corpus qui n'ont pas encore été validées par des experts linguistes.

## Contexte

### Corpus Existant
Le dossier `raw/` contient **l'intégralité du corpus collecté** via Kallama pour trois langues sénégalaises :
- **Pulaar** (code ISO : fuc)
- **Sereer** (code ISO : srr)
- **Wolof** (code ISO : wol)

### Objectif de la Réannotation
Notre objectif est de **réannoter les parties non vérifiées (unchecked)** du corpus - c'est-à-dire les transcriptions qui n'ont pas encore été validées par des experts linguistes. Cette validation et correction sont effectuées par les linguistes experts de **CLAD**.

## Source des Données Audio

**Note importante :** L'ensemble des fichiers audio correspondant aux transcriptions sont disponibles sur **OpenSLR** (Open Speech and Language Resources).

### Téléchargement des Datasets Audio

Les datasets audio peuvent être téléchargés depuis OpenSLR aux URLs suivantes :

```bash
# Téléchargement du dataset Wolof
wget https://www.openslr.org/resources/151/speech_dataset_wol.tar.gz

# Téléchargement du dataset Pulaar
wget https://www.openslr.org/resources/151/speech_dataset_fuc.tar.gz

# Téléchargement du dataset Sereer
wget https://www.openslr.org/resources/151/speech_dataset_srr.tar.gz
```

### Ressource OpenSLR
- **Référence** : OpenSLR Resource #151
- **Lien** : https://www.openslr.org/151/

## Workflow de Réannotation

Le processus de réannotation suit les étapes suivantes :

```
Corpus raw (unchecked) → Extraction → Lots de 5h → Google Colab → GCP → Label Studio → Validation CLAD
```

### 1. Extraction des Données Non Vérifiées
À partir du corpus complet dans `raw/`, nous avons extrait les transcriptions non vérifiées pour les trois langues.

### 2. Organisation en Lots
Les données extraites ont été divisées en lots de **5 heures** comprenant :
- Les fichiers audio correspondants (téléchargés depuis OpenSLR)
- Les pré-annotations existantes (transcriptions brutes)

### 3. Pipeline de Traitement

1. **Prétraitement** : Les lots sont d'abord traités dans **Google Colab** pour préparer les données et générer/améliorer les pré-annotations
2. **Stockage** : Les fichiers sont ensuite téléchargés vers **Google Cloud Platform (GCP)** pour l'hébergement
3. **Annotation** : Connexion à l'interface **Label Studio** pour la réannotation manuelle
4. **Validation** : Les experts linguistes de **CLAD** valident et corrigent les transcriptions

### 4. Résultat Final
Les transcriptions validées par CLAD sont ensuite placées dans le dossier `checked/` avec le statut de **transcriptions vérifiées**.

## Utilisation Future des Données

### Objectifs de Modélisation

Les transcriptions réannotées et validées seront utilisées en combinaison avec d'autres données existantes au sein de la communauté pour :

1. **Développer un modèle Wolof** : Un modèle ASR (Automatic Speech Recognition) spécialisé pour la langue wolof
2. **Développer un modèle multilingue** : Un modèle ASR capable de gérer simultanément les trois langues :
   - Pulaar (fuc)
   - Sereer (srr)
   - Wolof (wol)

### Stratégie de Développement

Les données réannotées de haute qualité permettront :
- Un entraînement plus robuste des modèles
- Une meilleure généralisation sur des données réelles
- Une amélioration des performances de reconnaissance vocale pour les langues sénégalaises
- La création de ressources linguistiques pérennes pour la communauté

## Structure des Dossiers

```
transcriptions/
├── checked/                      # Transcriptions validées par les experts CLAD
│   ├── transcriptions-fuc/       # Transcriptions en Pulaar vérifiées
│   │   ├── stm/                  # Format NIST
│   │   └── trs/                  # Format Transcriber
│   ├── transcriptions-srr/       # Transcriptions en Sereer vérifiées
│   │   ├── stm/
│   │   └── trs/
│   └── transcriptions-wol/       # Transcriptions en Wolof vérifiées
│       ├── stm/
│       └── trs/
└── raw/                          # Corpus complet collecté (incluant parties non vérifiées)
    ├── transcriptions-fuc/       # Transcriptions en Pulaar
    │   ├── stm/
    │   └── trs/
    ├── transcriptions-srr/       # Transcriptions en Sereer
    │   ├── stm/
    │   └── trs/
    └── transcriptions-wol/       # Transcriptions en Wolof
        ├── stm/
        └── trs/
```

## Formats de Fichiers

Les transcriptions sont fournies dans deux formats :

- **TRS** : Format Transcriber (format XML pour l'outil Transcriber)
- **STM** : Format NIST (Segment Time Mark format)

Pour plus d'informations sur ces formats, consultez la section **3.12 What are the reference transcript data formats?** de la documentation du projet.

## Règles de Transcription

### 1. Marquage des Disfluences

Les disfluences (hésitations, pauses remplies, etc.) sont marquées par un token commençant par `%`.

**Exemples de labels :**
- `%e` : hésitation
- `%hum` : son d'hésitation

**Exemples dans le corpus :**
- `wol_43611.trs` : %e prévision :fra météo :fra yi xibaar yi si jaww ji
- `fuc_42111.trs` : %hum %hum céréal

### 2. Marquage des Emprunts Linguistiques

Les emprunts à d'autres langues sont marqués par un tag linguistique commençant par `:` suivi du code de langue.

**Format recommandé :** `mot_emprunté :code_langue`

**Exemples :**
- `accompagnement :fra` (emprunt au français)
- `marketing :en` (emprunt à l'anglais)

**Exemples dans le corpus :**
- `wol_43611.trs` : météo :fra yi xibaar yi si jaww ji
- `fuc_41812.trs` : ndokka mo microphone :fra
- `srr_3610.trs` : Maaga i mbaaga consulter :fra météo :fra bo andoona ye

### 3. Notes Importantes sur les Conventions

#### Codes de Langues
Bien que le code **ISO 639-2** (3 lettres) soit recommandé, le code **ISO 639-1** (2 lettres) est également présent dans certaines transcriptions.

**Exemples :**
- `accompagnement :fr` (au lieu de :fra)
- `wol_4613.trs` : marketing :en
- `wol_43112.trs` : bisimilah :ar donc :fra looy yok

#### Espacement
La convention recommandée est d'ajouter un espace avant le tag linguistique mais pas entre les deux-points et le code de langue : `mot :langue`

Cependant, certaines transcriptions contiennent un espace supplémentaire : `mot : langue`

**Exemples dans le corpus :**
- `fuc_4911.trs` : tottaaɗo machine : fra nani sara mum
- `wol_4611.trs` : comme :fra business : en
- `srr_4210.trs` : fumier : fra feene

## Statut des Transcriptions

### Dossier `raw/`
Contient **l'intégralité du corpus collecté** via Kallama, incluant :
- Les transcriptions déjà vérifiées (partiellement)
- Les transcriptions **non vérifiées (unchecked)** qui font l'objet de ce projet de réannotation

### Dossier `checked/`
Contient les transcriptions qui ont été **validées et corrigées** par les experts linguistes de **CLAD**. Ce dossier s'enrichit au fur et à mesure de l'avancement du projet de réannotation.

## Rôles et Responsabilités

### AI Hub Sénégal
- Gestion technique du projet
- Mise en place du pipeline de traitement
- Configuration des outils (Colab, GCP, Label Studio)
- Développement des modèles ASR

### CLAD
- Validation linguistique par des experts natifs
- Correction et standardisation des transcriptions
- Assurance qualité des annotations

## Technologies Utilisées

- **OpenSLR** : Source des données audio (Resource #151)
- **Google Colab** : Environnement de prétraitement et de préparation des lots
- **Google Cloud Platform (GCP)** : Stockage et hébergement des données audio et des transcriptions
- **Label Studio** : Interface d'annotation et de réannotation des transcriptions
- **Kallama** : Source du corpus audio original

## Métriques du Projet

- **Taille des lots** : 5 heures d'audio par lot
- **Langues couvertes** : 3 (Pulaar, Sereer, Wolof)
- **Formats de sortie** : TRS et STM
- **Source audio** : OpenSLR Resource #151

## Contact et Support

Pour toute question concernant le processus de réannotation ou les transcriptions, veuillez contacter :
- **AI Hub Sénégal** : Support technique et questions sur le pipeline
- **CLAD** : Questions linguistiques et validation des transcriptions

## Références

- **OpenSLR Resource #151** : https://www.openslr.org/151/
- **Kallama Project** : Corpus de données audio pour les langues sénégalaises


