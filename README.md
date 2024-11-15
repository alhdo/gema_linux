# Mission Interstellaire : Formation des Cadets en Programmation Bash
## Centre de Formation de la Fédération Galactique - Année Stellaire 2424

## /// TRANSMISSION PRIORITAIRE ///
**DE:** Académie de Programmation de la Fédération Galactique  
**À:** Cadets de Première Année  
**OBJET:** Formation aux Systèmes de Commande des Vaisseaux


## [CONTEXTE DE MISSION]
L'année est 2424. La Fédération Galactique forme sa nouvelle génération de programmeurs spatiaux. Les systèmes de nos vaisseaux fonctionnent sous BashOS, un dérivé spatial de l'ancien système Unix. En tant que Cadet, vous devez maîtriser la programmation de ces systèmes.

## [NIVEAU 1] : Initialisation du Terminal de Commande
**Contexte:** Votre premier jour à bord du vaisseau-école "USS Terminal"

**Mission:**
1. Créez votre identifiant de terminal (`hello_space.sh`)
2. Configurez les autorisations d'accès (permissions d'exécution)
3. Lancez votre première communication :
   ```
   "Terminal [VOTRE_NOM] initialisé. Prêt pour les ordres."
   ```
**Mission:**

```bash
#!/bin/bash

# Définition des variables
CADET_NAME="$1"
TERMINAL_ID="$(hostname)"

# Vérification du nom
if [ -z "$CADET_NAME" ]; then
    echo "Erreur: Nom du cadet requis"
    exit 1
fi

# Message d'initialisation
echo "Terminal [$CADET_NAME] initialisé. Prêt pour les ordres."
echo "ID Terminal: $TERMINAL_ID"

# Test de validation
if [ $? -eq 0 ]; then
    echo "Initialisation réussie"
fi
```

**Test:**
```bash
chmod +x hello_space.sh
./hello_space.sh "Cadet Evan"
```

**Objectifs d'apprentissage:**
- Création de script
- Shebang
- Permissions
- Première exécution

## [NIVEAU 2] : Configuration du Journal de Bord
**Contexte:** Chaque officier doit maintenir un journal de bord personnel

**Mission:**
1. Créez votre journal de bord (`space_log.sh`)
2. Configurez les variables d'identification :
   ```bash
   OFFICER_NAME
   RANK
   STARSHIP
   MISSION_DATE
   ```
3. Affichez l'en-tête de votre journal :
   ```
   === JOURNAL DE BORD ===
   Officier : [OFFICER_NAME]
   Rang : [RANK]
   Vaisseau : [STARSHIP]
   Date Stellaire : [MISSION_DATE]
   ```

**Validation:** Le capitaine doit pouvoir lire clairement vos informations

**Example structure**
```bash
#!/bin/bash

# Configuration
OFFICER_NAME="Cadet Johnson"
RANK="Première Classe"
STARSHIP="USS Terminal"
MISSION_DATE=$(date +"%Y.%m.%d")

# Fonction de validation
validate_fields() {
    local fields=("$OFFICER_NAME" "$RANK" "$STARSHIP")
    for field in "${fields[@]}"; do
        if [ -z "$field" ]; then
            echo "Erreur: Champs manquants"
            exit 1
        fi
    done
}

# Affichage formaté
display_header() {
    cat << EOF
=== JOURNAL DE BORD ===
Officier : $OFFICER_NAME
Rang    : $RANK
Vaisseau: $STARSHIP
Date    : $MISSION_DATE
====================
EOF
}

# Exécution
validate_fields
display_header
```

## [NIVEAU 3] : Système de Communication Interstellaire
**Contexte:** Les communications avec d'autres vaisseaux sont essentielles

**Mission:**
1. Créez un système de communication (`space_comm.sh`)
2. Le programme doit :
   - Demander le nom du vaisseau destinataire
   - Demander la priorité du message (1-5)
   - Enregistrer le message
   - Sauvegarder dans `transmissions_log.txt`

**Format de transmission:**
```
=== TRANSMISSION SPATIALE ===
De: [Votre vaisseau]
À: [Vaisseau destinataire]
Priorité: [Niveau]
Message: [Contenu]
=== FIN DE TRANSMISSION ===
```

**Exemple avancé:**
```bash
#!/bin/bash

# Configuration
LOG_FILE="transmissions_log.txt"
PRIORITY_LEVELS=(1 2 3 4 5)

# Validation de la priorité
validate_priority() {
    local priority=$1
    for level in "${PRIORITY_LEVELS[@]}"; do
        if [ "$priority" -eq "$level" ]; then
            return 0
        fi
    done
    return 1
}

# Enregistrement du message
log_transmission() {
    local dest=$1
    local priority=$2
    local message=$3
    
    {
        echo "=== TRANSMISSION SPATIALE ==="
        echo "Date: $(date)"
        echo "De: $STARSHIP"
        echo "À: $dest"
        echo "Priorité: $priority"
        echo "Message: $message"
        echo "=== FIN DE TRANSMISSION ==="
        echo
    } >> "$LOG_FILE"
}

# Interface utilisateur
echo "Système de Communication Interstellaire"
read -p "Vaisseau destinataire: " dest_ship
read -p "Priorité (1-5): " priority
read -p "Message: " message

# Validation et envoi
if ! validate_priority "$priority"; then
    echo "Erreur: Niveau de priorité invalide"
    exit 1
fi

log_transmission "$dest_ship" "$priority" "$message"
```
## [NIVEAU 4] : Scanner de Diagnostics
**Contexte:** Les systèmes du vaisseau doivent être régulièrement vérifiés

**Mission:**
1. Créez un scanner (`system_scan.sh`)
2. Recueillez les informations suivantes :
   - Nom du technicien
   - Secteur du vaisseau
   - État des systèmes (OK/Défaillant)
3. Enregistrez les résultats dans `ship_diagnostics.log`

**Exemple complet**
```bash
#!/bin/bash

# Configuration
LOG_FILE="ship_diagnostics.log"
SECTORS=("Pont" "Ingénierie" "Armement" "Sciences" "Communication")
SYSTEMS=("Électrique" "Hydraulique" "Informatique" "Environnemental")

# Couleurs pour le terminal
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m' # No Color

# Fonction de validation des entrées
validate_input() {
    local input=$1
    local type=$2
    local valid=false

    case $type in
        "sector")
            for sector in "${SECTORS[@]}"; do
                if [ "$input" == "$sector" ]; then
                    valid=true
                    break
                fi
            done
            ;;
        "status")
            if [ "$input" == "OK" ] || [ "$input" == "Défaillant" ]; then
                valid=true
            fi
            ;;
    esac

    if [ "$valid" == false ]; then
        echo "Erreur: Entrée invalide - $input pour $type"
        exit 1
    fi
}

# Fonction pour générer un ID unique de scan
generate_scan_id() {
    echo "SCAN-$(date +%Y%m%d-%H%M%S)"
}

# Fonction pour vérifier l'état d'un système
check_system() {
    local system=$1
    local sector=$2
    
    # Simulation de vérification système
    local random_status=$((RANDOM % 10))
    if [ $random_status -gt 8 ]; then
        echo "Défaillant"
    else
        echo "OK"
    fi
}

# Fonction pour enregistrer les résultats
log_result() {
    local scan_id=$1
    local tech_name=$2
    local sector=$3
    local system=$4
    local status=$5
    local timestamp=$(date '+%Y-%m-%d %H:%M:%S')

    echo "$timestamp | $scan_id | $tech_name | $sector | $system | $status" >> "$LOG_FILE"
}

# Fonction d'affichage du rapport
display_report() {
    local scan_id=$1
    local tech_name=$2
    
    echo -e "\n=== RAPPORT DE DIAGNOSTIC ==="
    echo "ID Scan: $scan_id"
    echo "Technicien: $tech_name"
    echo "Date: $(date '+%Y-%m-%d %H:%M:%S')"
    echo "========================="
}

# Programme principal
main() {
    # Vérification du fichier de log
    if [ ! -f "$LOG_FILE" ]; then
        echo "# JOURNAL DE DIAGNOSTICS - VAISSEAU" > "$LOG_FILE"
        echo "# Format: Date | ID Scan | Technicien | Secteur | Système | État" >> "$LOG_FILE"
    fi

    # Collecte des informations
    echo "=== SCANNER DE DIAGNOSTICS ==="
    read -p "Nom du technicien: " tech_name

    if [ -z "$tech_name" ]; then
        echo -e "${RED}Erreur: Nom du technicien requis${NC}"
        exit 1
    fi

    # Génération de l'ID de scan
    scan_id=$(generate_scan_id)

    # Affichage des secteurs disponibles
    echo -e "\nSecteurs disponibles:"
    for i in "${!SECTORS[@]}"; do
        echo "$((i+1)). ${SECTORS[i]}"
    done

    # Sélection du secteur
    read -p "Sélectionnez un secteur (1-${#SECTORS[@]}): " sector_choice
    sector="${SECTORS[$((sector_choice-1))]}"
    validate_input "$sector" "sector"

    # Début du scan
    echo -e "\n${YELLOW}Démarrage du scan du secteur: $sector${NC}"
    display_report "$scan_id" "$tech_name"

    # Scan de chaque système
    for system in "${SYSTEMS[@]}"; do
        echo -e "\nAnalyse du système $system..."
        status=$(check_system "$system" "$sector")
        
        if [ "$status" == "OK" ]; then
            echo -e "${GREEN}État: OK${NC}"
        else
            echo -e "${RED}État: Défaillant${NC}"
        fi

        log_result "$scan_id" "$tech_name" "$sector" "$system" "$status"
    done

    # Rapport final
    echo -e "\n${GREEN}Scan terminé avec succès${NC}"
    echo "Résultats enregistrés dans $LOG_FILE"
}

# Point d'entrée avec gestion d'erreurs
{
    main
} || {
    echo -e "${RED}Une erreur est survenue durant le scan${NC}"
    exit 1
}
```

**Fonctionnalités clés:**
- Génération d'ID unique pour chaque scan
- Validation des entrées utilisateur
- Simulation réaliste des états système
- Journal détaillé des diagnostics
- Gestion des couleurs pour meilleure lisibilité
- Gestion des erreurs complète

**Points de contrôle:**
- [ ]  Le script demande correctement les informations utilisateur
- [ ]  Les résultats sont enregistrés dans le fichier log
- [ ]  Les codes couleur s'affichent correctement
- [ ]  Les erreurs sont gérées proprement
- [ ]  Le format du rapport est conforme aux standards
## [NIVEAU 5] : MISSION FINALE - Centre de Contrôle
**Contexte:** La Fédération a besoin d'un système de contrôle centralisé

**Mission:** Créez le centre de contrôle (`control_center.sh`) qui :
1. Affiche un message de bienvenue
2. Demande l'identification de l'officier
3. Propose les options suivantes :
   - Journal de bord
   - Communications
   - Diagnostics
   - Quitter
4. Enregistre toutes les actions dans `control_center.log`

**Exemple complet**

```bash
#!/bin/bash

# Configuration globale
LOG_FILE="control_center.log"
SPACE_LOG="space_log.sh"
SPACE_COMM="space_comm.sh"
SYSTEM_SCAN="system_scan.sh"
EMERGENCY_PROTOCOL="emergency_protocol.sh"

# Couleurs
RED='\033[0;31m'
GREEN='\033[0;32m'
BLUE='\033[0;34m'
YELLOW='\033[1;33m'
NC='\033[0m'

# ASCII Art pour le centre de contrôle
display_header() {
    echo "======================================="
    echo "          CENTRE DE CONTRÔLE           "
    echo "         FÉDÉRATION GALACTIQUE         "
    echo "======================================="
    echo "     _____                             "
    echo "    |     |     CONTRÔLE SPATIAL      "
    echo "    |     |_____     ________         "
    echo "    |           |   |        |        "
    echo "    |     ______|   |________|        "
    echo "    |     |                           "
    echo "    |_____|                           "
    echo "======================================="
}

# Fonction de logging
log_action() {
    local timestamp=$(date '+%Y-%m-%d %H:%M:%S')
    local user=$1
    local action=$2
    echo "$timestamp | $user | $action" >> "$LOG_FILE"
}

# Validation de l'officier
validate_officer() {
    local name=$1
    local rank=$2
    
    if [[ -z "$name" ]] || [[ -z "$rank" ]]; then
        echo -e "${RED}Erreur: Nom et rang requis${NC}"
        return 1
    fi

    # Vérification du format du nom (lettres et espaces uniquement)
    if ! [[ $name =~ ^[A-Za-z[:space:]]+$ ]]; then
        echo -e "${RED}Erreur: Format de nom invalide${NC}"
        return 1
    fi

    return 0
}

# Menu Journal de Bord
journal_menu() {
    local officer_name=$1
    local officer_rank=$2

    if [[ -f "$SPACE_LOG" ]]; then
        echo -e "${YELLOW}Ouverture du journal de bord...${NC}"
        ./"$SPACE_LOG" "$officer_name" "$officer_rank"
        log_action "$officer_name" "Accès journal de bord"
    else
        echo -e "${RED}Erreur: Journal de bord non trouvé${NC}"
    fi
}

# Menu Communications
communications_menu() {
    local officer_name=$1

    if [[ -f "$SPACE_COMM" ]]; then
        echo -e "${YELLOW}Initialisation du système de communication...${NC}"
        ./"$SPACE_COMM"
        log_action "$officer_name" "Utilisation système communication"
    else
        echo -e "${RED}Erreur: Système de communication non trouvé${NC}"
    fi
}

# Menu Diagnostics
diagnostics_menu() {
    local officer_name=$1

    if [[ -f "$SYSTEM_SCAN" ]]; then
        echo -e "${YELLOW}Lancement du scanner de diagnostics...${NC}"
        ./"$SYSTEM_SCAN" "$officer_name"
        log_action "$officer_name" "Exécution diagnostics système"
    else
        echo -e "${RED}Erreur: Scanner de diagnostics non trouvé${NC}"
    fi
}

# Menu Urgences
emergency_menu() {
    local officer_name=$1

    if [[ -f "$EMERGENCY_PROTOCOL" ]]; then
        echo -e "${RED}!!! ACTIVATION PROTOCOLES D'URGENCE !!!${NC}"
        ./"$EMERGENCY_PROTOCOL"
        log_action "$officer_name" "Activation protocoles d'urgence"
    else
        echo -e "${RED}Erreur: Protocoles d'urgence non trouvés${NC}"
    fi
}

# Vérification de l'environnement
check_environment() {
    # Vérification du fichier de log
    if [[ ! -f "$LOG_FILE" ]]; then
        echo "# JOURNAL DU CENTRE DE CONTRÔLE" > "$LOG_FILE"
        echo "# Format: Date | Officier | Action" >> "$LOG_FILE"
    fi

    # Vérification des permissions
    for script in "$SPACE_LOG" "$SPACE_COMM" "$SYSTEM_SCAN" "$EMERGENCY_PROTOCOL"; do
        if [[ -f "$script" && ! -x "$script" ]]; then
            chmod +x "$script"
        fi
    done
}

# Menu principal
display_menu() {
    echo -e "\n=== CENTRE DE CONTRÔLE ==="
    echo "1. Journal de Bord"
    echo "2. Communications"
    echo "3. Diagnostics"
    echo "4. Protocoles d'Urgence"
    echo "5. Quitter"
    echo "======================="
}

# Programme principal
main() {
    clear
    display_header
    check_environment

    # Identification de l'officier
    echo -e "\n${BLUE}=== IDENTIFICATION REQUISE ===${NC}"
    read -p "Nom de l'officier: " officer_name
    read -p "Rang: " officer_rank

    if ! validate_officer "$officer_name" "$officer_rank"; then
        exit 1
    fi

    echo -e "${GREEN}Identification réussie. Bienvenue, $officer_rank $officer_name${NC}"
    log_action "$officer_name" "Connexion au centre de contrôle"

    while true; do
        display_menu
        read -p "Sélectionnez une option (1-5): " choice

        case $choice in
            1)
                journal_menu "$officer_name" "$officer_rank"
                ;;
            2)
                communications_menu "$officer_name"
                ;;
            3)
                diagnostics_menu "$officer_name"
                ;;
            4)
                emergency_menu "$officer_name"
                ;;
            5)
                echo -e "${YELLOW}Déconnexion du centre de contrôle...${NC}"
                log_action "$officer_name" "Déconnexion"
                exit 0
                ;;
            *)
                echo -e "${RED}Option invalide${NC}"
                ;;
        esac

        read -p "Appuyez sur Entrée pour continuer..."
    done
}

# Gestion des erreurs
trap 'echo -e "${RED}Programme interrompu${NC}"; exit 1' INT TERM

# Exécution du programme
main || {
    echo -e "${RED}Une erreur est survenue dans le centre de contrôle${NC}"
    exit 1
}
```
## [SYSTÈME DE RÉUSSITE]
- ⭐ Cadet (50 points) : Niveaux 1-2 complétés
- ⭐⭐ Lieutenant (75 points) : Niveaux 1-3 complétés
- ⭐⭐⭐ Commandant (90 points) : Niveaux 1-4 complétés
- ⭐⭐⭐⭐ Capitaine (100 points) : Tous les niveaux complétés

## [BONUS DE MISSION]
- Style de code propre (+5 points)
- Messages d'erreur personnalisés (+5 points)
- ASCII art dans les interfaces (+5 points)

## [PROTOCOLE DE SOUMISSION]
1. Tous les scripts dans un dossier `mission_spatiale`
2. Un fichier `mission_report.txt` contenant :
   - Vos commandes
   - Vos choix tactiques
   - Les obstacles rencontrés
3. Captures d'écran des systèmes en fonctionnement

## [DIRECTIVES DE LA FÉDÉRATION]
- Tout script doit commencer par le shebang réglementaire
- Les variables doivent suivre le protocole de nommage standard
- La documentation est obligatoire pour la maintenance future
- Chaque système doit gérer ses entrées/sorties de manière sécurisée

## [CONSEILS DU COMMANDEMENT]
```
"Un bon programmeur spatial documente toujours son code.
Les erreurs dans l'espace peuvent être fatales.
La clarté du code est aussi importante que sa fonctionnalité."
```

## /// FIN DE TRANSMISSION ///
*"Ad Astra Per Bash"*

[NOTE]: Ce TD est une simulation d'entraînement. Aucun vaisseau spatial n'a été endommagé durant sa conception.
