GESTION DE BIBLIOTHÈQUE, RÉALISÉ PAR BARMANE IMANE, TROMBATI MALAK ET ETTAOUSSI NOUHAILA :

voici le code suivant :

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct {
    int ID, disponibilite;
    char titre[10], auteur[10], description[15], nom[10], categorie[10];
    float prix;
} Livre;

void remplissage(Livre *l, int index) {
    printf("Donnez l'ID : ");
    scanf("%d", &(l + index)->ID);
    printf("Donnez la disponibilite : ");
    scanf("%d", &(l + index)->disponibilite);
    printf("Donnez le titre : ");
    scanf("%s", (l + index)->titre);
    printf("Donnez l'auteur : ");
    scanf("%s", (l + index)->auteur);
    printf("Donnez la description : ");
    scanf("%s", (l + index)->description);
    printf("Donnez le nom : ");
    scanf("%s", (l + index)->nom);
    printf("Donnez la categorie : ");
    scanf("%s", (l + index)->categorie);
}

void affichage(Livre *l, int index) {
    printf("ID : %d\n", (l + index)->ID);
    printf("Disponibilite : %d\n", (l + index)->disponibilite);
    printf("Titre : %s\n", (l + index)->titre);
    printf("Auteur : %s\n", (l + index)->auteur);
    printf("Description : %s\n", (l + index)->description);
    printf("Nom : %s\n", (l + index)->nom);
    printf("Categorie : %s\n", (l + index)->categorie);
}

Livre* Ajout(Livre **l, int *n) {
    int choix;
    printf("Saisissez 1 pour l'ajout au début, 2 pour l'ajout à la fin, et 3 pour l'ajout dans une position\n");
    do {
        printf("Donnez votre choix : ");
        scanf("%d", &choix);
    } while (choix != 1 && choix != 2 && choix != 3);

    int k;
    Livre *temp = NULL;

    switch (choix) {
        case 1: // Ajout au début
            printf("Donnez le nombre de livres à ajouter : ");
            scanf("%d", &k);
            temp = (Livre*)realloc(*l, (*n + k) * sizeof(Livre));
            if (temp) {
                *l = temp;

                
                for (int i = *n - 1; i >= 0; i--) {
                    *(*l + i + k) = *(*l + i); // Décalage vers la droite
                }

                
                for (int i = 0; i < k; i++) {
                    remplissage(*l, i); 
                }

                *n += k; 
            } else {
                printf("Erreur de réallocation de mémoire.\n");
            }
            break;

        case 2: // Ajout à la fin
            printf("Donnez le nombre de livres à ajouter : ");
            scanf("%d", &k);
            temp = (Livre*)realloc(*l, (*n + k) * sizeof(Livre));
            if (temp) {
                *l = temp;
                for (int i = *n; i < *n + k; i++) {
                    remplissage(*l, i);
                }

                *n += k; 
            } else {
                printf("Erreur de réallocation de mémoire.\n");
            }
            break;

        case 3: // Ajout dans une position
            int pos;
            printf("Donnez la position : ");
            scanf("%d", &pos);
            if (pos < 0 || pos >= *n) {
                printf("Position invalide.\n");
                break;
            }
            temp = (Livre*)realloc(*l, (*n + 1) * sizeof(Livre));
            if (temp) {
                *l = temp;

                
                for (int i = *n; i >= pos; i--) {
                    *(*l + i + 1) = *(*l + i); // Décalage vers la droite
                }

                remplissage(*l, pos); 
                *n += 1; 
            } else {
                printf("Erreur de réallocation de mémoire.\n");
            }
            break;
    }

    return *l;
}

Livre* suppression(Livre **l, int *n) {
    
    int pos;
    printf("Saisissez la position à supprimer : ");
    scanf("%d", &pos);

    
    if (pos < 0 || pos >= *n) {
        printf("Position invalide.\n");
        return *l; 
    }

    for (int i = pos; i < *n - 1; i++) {
        *(*l + i) = *(*l + i + 1); // Décaler les éléments vers la gauche
    }

    
    Livre temp = (Livre)realloc(*l, (*n - 1) * sizeof(Livre));
    if (temp) {
        *l = temp;
        *n -= 1;
    } else {
        printf("Erreur de réallocation de mémoire.\n"); 
    }

    return *l; 
}

void modification(Livre *l, int n) {
    int id1, choix1;
    printf("Donnez l'ID du livre que vous souhaitez modifier : ");
    scanf("%d", &id1);
    
    do {
        printf("Pour modifier le nom(1), le titre(2), description(3), catégorie(4), auteur(5) : ");
        scanf("%d", &choix1);
    } while (choix1 < 1 || choix1 > 5);
    
    for (int i = 0; i < n; i++) {
        if ((l + i)->ID == id1) {
            switch (choix1) {
                case 1:
                    printf("Donnez le nouveau nom : ");
                    scanf("%s", (l + i)->nom);
                    break;
                case 2:
                    printf("Donnez le nouveau titre : ");
                    scanf("%s", (l + i)->titre);
                    break;
                case 3:
                    printf("Donnez la nouvelle description : ");
                    scanf("%s", (l + i)->description);
                    break;
                case 4:
                    printf("Donnez la nouvelle catégorie : ");
                    scanf("%s", (l + i)->categorie);
                    break;
                case 5:
                    printf("Donnez le nouvel auteur : ");
                    scanf("%s", (l + i)->auteur);
                    break;
                default:
                    printf("Choix invalide.\n");
            }
            affichage(l, i);
            break;
        }
    }
}

void sauvegarder_csv(Livre *livres, int nombre) {
    FILE *fichier = fopen("livres.txt", "w");
    if (fichier == NULL) {
        printf("Erreur lors de l'ouverture du fichier.\n");
        return;
    }
    fprintf(fichier, "ID,Disponibilite,Auteur,Description,Nom,Categorie\n");
    for (int i = 0; i < nombre; i++) {
        fprintf(fichier, "%d,%d,%s,%s,%s,%s\n", livres[i].ID, livres[i].disponibilite, livres[i].auteur, livres[i].description, livres[i].nom, livres[i].categorie);
    }
    fclose(fichier);
    printf("Les livres ont ete sauvegardes dans le fichier 'livres.csv'.\n");
}

void recherche(Livre *livres, int nombre) {
    char i;
    char j[30];
    int k;

    printf("Chercher le livre par : 't' pour le titre ou 'a' pour l'auteur : ");
    getchar(); // Pour consommer le saut de ligne restant
    scanf("%c", &i);

    if (i == 't') {
        printf("Donnez le titre : ");
        scanf("%s", j);
        int found = 0;
        for (k = 0; k < nombre; k++) {
            if (strcmp((livres + k)->titre, j) == 0) {
                affichage(livres, k);
                found = 1;
                break;
            }
        }
        if (!found) {
            printf("Le livre avec le titre '%s' n'existe pas.\n", j);
        }
    } else if (i == 'a') {
        printf("Donnez l'auteur : ");
        scanf("%s", j);
        int found = 0;
        for (k = 0; k < nombre; k++) {
            if (strcmp((livres + k)->auteur, j) == 0) {
                affichage(livres, k);
                found = 1;
                break;
            }
        }
        if (found == 0) {
            printf("Le livre avec l'auteur '%s' n'existe pas.\n", j);
        }
    } else {
        printf("Le livre avec l'auteur '%s' existe .\n", j);
    }
}

int main() {
    char nomUtilisateur[10];
    printf("Donnez votre nom : ");
    scanf("%s", nomUtilisateur);

    int nbr;
    printf("Donnez le nombre de livres : ");
    scanf("%d", &nbr);

    Livre livres = (Livre)malloc(nbr * sizeof(Livre));
    if (!livres) {
        printf("Erreur de mémoire.\n");
        return 1;
    }

    for (int i = 0; i < nbr; i++) {
        printf("Pour le livre %d\n", i + 1);
        remplissage(livres, i);
    }

    int continuer = 1;
    while (continuer) {
        printf("Donnez votre choix: \n");
        printf("1 pour l'affichage\n");
        printf("2 pour l'ajout\n");
        printf("3 pour la suppression\n");
        printf("4 pour la modification\n");
        printf("5 pour le sauvegardage dans le fichier\n");
        printf("6 pour la recherche\n");
        printf("7 pour quitter\n");

        int choix;
        scanf("%d", &choix);

        switch (choix) {
            case 1:
                for (int i = 0; i < nbr; i++) {
                    printf("Pour le livre %d\n", i + 1);
                    affichage(livres, i);
                }
                break;
            case 2:
                livres = Ajout(&livres, &nbr);
                break;
            case 3:
                livres = suppression(&livres, &nbr);
                break;
            case 4:
                modification(livres, nbr);
                break;
            case 5:
                sauvegarder_csv(livres, nbr);
                break;
            case 6:
                recherche(livres, nbr);
                break;
            case 7:
                continuer = 0;
                break;
            default:
                printf("Choix invalide.\n");
        }
    }

    free(livres);
    return 0;
}

voici l'explication :

remplissage:
Cette fonction permet de remplir les détails d'un livre.
Elle prend un pointeur vers une structure Livre et un index en argument.
Elle demande à l'utilisateur de saisir les détails de chaque attribut d'un livre (ID, disponibilité, titre, auteur, description, nom, catégorie) et les stocke dans la structure Livre à 
l'index spécifié.

affichage:
Cette fonction affiche les détails d'un livre.
Elle prend un pointeur vers une structure Livre et un index en argument.
Elle affiche les valeurs des attributs d'un livre à l'index spécifié.

Ajout:
Cette fonction permet d'ajouter un ou plusieurs livres à la collection.
Elle prend un double pointeur vers une structure Livre et un pointeur vers un entier n (représentant le nombre de livres) en argument.
Elle demande à l'utilisateur où ajouter les nouveaux livres (au début, à la fin ou à une position spécifique), puis alloue dynamiquement de la mémoire pour stocker les nouveaux livres.
Elle décale les livres existants si nécessaire et remplit les détails des nouveaux livres.

suppression:
Cette fonction permet de supprimer un livre de la collection.
Elle prend un double pointeur vers une structure Livre et un pointeur vers un entier n (représentant le nombre de livres) en argument.
Elle demande à l'utilisateur de saisir la position du livre à supprimer, puis supprime le livre en décalant les livres suivants.

modification:
Cette fonction permet de modifier les détails d'un livre existant.
Elle prend un pointeur vers une structure Livre et un entier n (représentant le nombre de livres) en argument.
Elle demande à l'utilisateur de saisir l'ID du livre à modifier ainsi que l'attribut à modifier, puis modifie la valeur de cet attribut.

sauvegarder_csv:
Cette fonction permet de sauvegarder les détails des livres dans un fichier CSV.
Elle prend un pointeur vers une structure Livre et un entier nombre (représentant le nombre de livres) en argument.
Elle ouvre un fichier texte en mode écriture, écrit les détails des livres dans le fichier au format CSV, puis ferme le fichier.

recherche:
Cette fonction permet de rechercher un livre par titre ou auteur.
Elle prend un pointeur vers une structure Livre et un entier nombre (représentant le nombre de livres) en argument.
Elle demande à l'utilisateur de choisir le critère de recherche (titre ou auteur), puis de saisir le titre ou l'auteur à rechercher.
Elle parcourt la collection de livres et affiche les détails du livre trouvé correspondant au critère de recherche.
Elle parcourt la collection de livres et affiche les détails du livre trouvé correspondant au critère de recherche.

