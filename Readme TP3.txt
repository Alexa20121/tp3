# README pour le Projet de Gestion des Rôles et Politiques AD
 
## Introduction
 
Ce projet vise à gérer les rôles et les politiques d'accès dans Active Directory. 
Il permet de créer des groupes correspondant aux rôles métiers et aux politiques.
 
## Prérequis
 
- *PowerShell* : Version 5.1 ou supérieure
- *Modules* : ActiveDirectory` doit être installé.
- *Droits d'accès* : Disposer des droits d'administrateur sur Active Directory pour exécuter les scripts.
 
## Création des Unités d'Organisation (OU)
 
Avant d'exécuter le script, assurez-vous de créer les Unités d'Organisation nécessaires dans Active Directory. Exécutez les commandes suivantes dans PowerShell en tant qu'administrateur :
 
--> Dans powershell
# Création de l'OU principale "MEDISOLVE"
New-ADOrganizationalUnit -Name "MEDISOLVE" -Path "DC=SF,DC=LOCAL"
 
# Création de l'OU pour les politiques
New-ADOrganizationalUnit -Name "Policies" -Path "DC=SF,DC=LOCAL"
 
# Liste des départements
$departments = @("RH", "Finances", "RD", "VentesMKT", "Services","OPS","Legal","BD","TI")
 
# Création d'une OU pour chaque département
foreach ($dept in $departments) {
    New-ADOrganizationalUnit -Name $dept -Path "OU=GROUP,OU=MEDISOLVE,DC=SF,DC=LOCAL"
}
 
# Création de l'OU pour les groupes
foreach ($role in $roles) {
    $adPath = "OU=GROUP,OU=$($role.Departement),OU=MEDISOLVE,DC=SF,DC=LOCAL"
    $normalizedGroupName = $role.Role -replace '\s', '-'
    New-ADGroup -Name $normalizedGroupName -SamAccountName $normalizedGroupName -GroupScope Global -GroupCategory Security -Path $adPath
}
 
 
1. Placez le CSV rbac.csv et rbacManager.ps1 dans le meme dossier.
2. Ouvrez les dossier dans PowerShell en tant que administrateur .
3. Preciser le chemin vers le CSV
4. Exécutez le script :rbacManager.ps1
5. Vérifiez Active Directory pour confirmer les differentes creations.