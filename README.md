#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Représentation d'un candidat
typedef struct {
    int id;
    char nom[100];
    char prenom[100];
    int votes;
} Candidat;

// Représentation d'une élection
typedef struct {
    int id;
    char titre[100];
    int nbrCandidats;
    Candidat candidats[20];
} Election;

// Structure pour représenter une notification
typedef struct {
    char msg[100];
} Notification;

// Tableau pour stocker les notifications
Notification notifications[10];
int nbNotifications = 0;

// Fonction pour ajouter une notification
void ajouterNotification(const char *message) {
    if (nbNotifications < 10) {
        strncpy(notifications[nbNotifications].msg, message, sizeof(notifications[nbNotifications].msg) - 1);
        notifications[nbNotifications].msg[sizeof(notifications[nbNotifications].msg) - 1] = '\0'; // Assure la terminaison de la chaîne.
        nbNotifications++;
        printf("Notification ajoutée avec succès.\n");
    } else {
        printf("Le nombre maximal de notifications est atteint.\n");
    }
}

// Fonction pour créer un compte candidat
Candidat creerCompteCandidat() {
    Candidat nouveauCompte;

    printf("Nom du candidat : ");
    scanf("%s", nouveauCompte.nom);

    printf("Prénom du candidat : ");
    scanf("%s", nouveauCompte.prenom);

    nouveauCompte.id = 0;  // L'ID peut être initialisé à 0
    nouveauCompte.votes = 0;  // Initialise le nombre de votes à 0

    return nouveauCompte;
}

// Fonction pour créer une nouvelle élection
void creerElection(Election *elections, int *nbrElections) {
    if (*nbrElections >= 20) {
        printf("Désolé, mais le nombre maximal d'élections est atteint.\n");
        return;
    }

    Election nouvelleElection;

    // Détails d'une nouvelle élection
    printf("Donnez un titre à l'élection : ");
    scanf("%s", nouvelleElection.titre);

    printf("Nombre de candidats : ");
    scanf("%d", &nouvelleElection.nbrCandidats);

    // Détails de chaque candidat
    for (int i = 0; i < nouvelleElection.nbrCandidats; ++i) {
        nouvelleElection.candidats[i] = creerCompteCandidat();
    }

    // Attribution d'un ID unique à l'élection
    nouvelleElection.id = *nbrElections + 1;

    // Ajout de la nouvelle élection au tableau
    elections[*nbrElections] = nouvelleElection;
    (*nbrElections)++;

    printf("Élection créée avec succès !\n");
}

// Fonction pour mettre à jour les informations d'un candidat
void mettreAJourCandidat(Election *elections) {
    int idElection;
    int idCandidat;

    printf("Élection à mettre à jour : ");
    scanf("%d", &idElection);

    if (idElection < 1 || idElection > *nbrElections) {
        printf("Erreur ! Élection invalide.\n");
        return;
    }

    Election *election = &elections[idElection - 1];

    printf("Candidat à mettre à jour : ");
    scanf("%d", &idCandidat);

    if (idCandidat < 1 || idCandidat > election->nbrCandidats) {
        printf("Candidat invalide.\n");
        return;
    }

    Candidat *candidat = &election->candidats[idCandidat - 1];

    printf("Liste des mises à jour du candidat %s %s :\n", candidat->nom, candidat->prenom);
    printf("1. Changer le nom\n");
    printf("2. Changer le prénom\n");
    printf("0. Quitter\n");

    int choix;
    printf("Entrez votre choix : ");
    scanf("%d", &choix);

    switch (choix) {
        case 1:
            printf("Nouveau nom : ");
            scanf("%s", candidat->nom);
            break;
        case 2:
            printf("Nouveau prénom : ");
            scanf("%s", candidat->prenom);
            break;
        case 0:
            printf("Quitter la mise à jour.\n");
            break;
        default:
            printf("Choix invalide.\n");
    }
}

// Fonction pour afficher les résultats d'une élection
void afficherResultats(Election *election) {
    printf("Résultats de l'élection %s :\n", election->titre);

    // Parcours de la liste des candidats
    for (int i = 0; i < election->nbrCandidats; ++i) {
        printf("%s %s : %d votes\n", election->candidats[i].nom, election->candidats[i].prenom, election->candidats[i].votes);
    }
}

// Fonction pour afficher la liste des votes
void afficherListeVotes(Election *election) {
    printf("Liste des votes de l'élection %s :\n", election->titre);

    // Parcours de la liste des candidats
    for (int i = 0; i < election->nbrCandidats; ++i) {
        printf("Votes pour %s %s :\n", election->candidats[i].nom, election->candidats[i].prenom);

        // Parcours de la liste des électeurs ayant voté pour ce candidat
        for (int j = 0; j < election->candidats[i].votes; ++j) {
            printf("- Électeur %d\n", j + 1);
        }

        printf("\n");
    }
}

// Fonction pour gérer une élection
void gererElection(Election *elections, int nbrElections) {
    int changerelection;
    int idElection;

    // Affichage des élections disponibles
    printf("Voici la liste des élections :\n");
    for (int i = 0; i < nbrElections; ++i) {
        printf("%d. %s\n", i + 1, elections[i].titre);
    }

    // Sélection de l'élection à gérer
    printf("Sélectionnez une élection à gérer (tapez 0 pour quitter) : ");
    scanf("%d", &idElection);

    if (idElection < 1 || idElection > nbrElections) {
        printf("Erreur ! Élection invalide.\n");
        return;
    }

    Election *election = &elections[idElection - 1];

    do {
        // Affichage du menu de gestion de l'élection
        printf("Menu de gestion de l'élection %s :\n", election->titre);
        printf("1. Afficher les candidats\n");
        printf("2. Voter\n");
        printf("3. Afficher les résultats\n");
        printf("4. Revenir à la liste des élections\n");
        printf("0. Quitter\n");

        // Sélection de l'opération à effectuer
        printf("Entrez votre choix : ");
        scanf("%d", &changerelection);

        switch (changerelection) {
            case 1:
                printf("Candidats de l'élection %s :\n", election->titre);
                for (int i = 0; i < election->nbrCandidats; ++i) {
                    printf("%d. %s %s\n", i + 1, election->candidats[i].nom, election->candidats[i].prenom);
                }
                break;
            case 2:
                printf("Erreur ! Fonctionnalité de vote non encore implémentée.\n");
                break;
            case 3:
                afficherResultats(election);
                break;
            case 4:
                printf("Revenir à la liste des élections.\n");
                break;
            case 0:
                printf("Quitter la gestion de l'élection.\n");
                break;
            default:
                printf("Erreur ! Choix invalide.\n");
        }

    } while (changerelection != 0 && changerelection != 4);
}

// Fonction pour gérer les tâches supplémentaires
void tacheSupplementaire() {
    int choix;
    do {
        printf("Menu :\n");
        printf("1. Ajouter une notification\n");
        printf("2. Afficher les notifications\n");
        printf("0. Quitter\n");

        printf("Entrez votre choix : ");
        scanf("%d", &choix);

        switch (choix) {
            case 1: {
                printf("Entrez le message de la notification : ");
                char message[100];
                scanf("%s", message);
                ajouterNotification(message);
                break;
            }
            case 2:
                printf("Afficher les notifications :\n");
                for (int i = 0; i < nbNotifications; ++i) {
                    printf("%d. %s\n", i + 1, notifications[i].msg);
                }
                break;
            case 0:
                printf("Au revoir !\n");
                break;
            default:
                printf("Erreur ! Choix invalide. Veuillez réessayer.\n");
        }

    } while (choix != 0);
}

int main() {
    Election elections[20];
    int nbrElections = 0;
    int choix;

    do {
        printf("Menu :\n");
        printf("1. Créer un nouveau compte\n");
        printf("2. Créer une nouvelle élection\n");
        printf("3. Mise à jour des informations des candidats\n");
        printf("4. Afficher et gérer les élections\n");
        printf("5. Afficher le résultat\n");
        printf("6. Afficher la liste des votes\n");
        printf("7. Tâche supplémentaire\n");
        printf("0. Quitter\n");

        printf("Entrez votre choix : ");
        scanf("%d", &choix);

        switch (choix) {
            case 1:
                creerCompteCandidat(); // Appeler la fonction correcte
                break;
            case 2:
                creerElection(elections, &nbrElections);
                break;
            case 3:
                mettreAJourCandidat(elections);
                break;
            case 4:
                gererElection(elections, nbrElections); // Appeler la fonction correcte
                break;
            case 5:
                afficherResultats(elections);
                break;
            case 6:
                afficherListeVotes(elections);
                break;
            case 7:
                tacheSupplementaire();
                break;
            case 0:
                printf("Au revoir !\n");
                break;
            default:
                printf("Erreur ! Choix invalide. Veuillez réessayer.\n");
        }

    } while (choix != 0);

    return 0;
}
