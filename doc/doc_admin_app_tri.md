![picto](img/Logo_web-GeoCompiegnois.png)

# Documentation d'administration de l'application TRI SELECTIF #

# Généralité

* Statut
  - [x] à rédiger
  - [ ] en cours de rédaction
  - [ ] relecture
  - [ ] finaliser
  - [ ] révision

* Historique des versions

|Date | Auteur | Description
|:---|:---|:---|
|18/12/2018|Grégory Bodet|version initiale|


# Généralité

|Représentation| Nom de l'application |Résumé|
|:---|:---|:---|
|![picto](/doc/img/tri_selectif_bleu.png)|Défense Extérieure Contre l'Incendie (DECI)|Cette application est dédiée à la gestion et la consultation des PEI (Points d'Eau Incendie) déterminant leur niveau de DECI.|

# Accès

|Public|Métier|Accès restreint|
|:-:|:-:|:---|
||X|Accès réservé aux personnels gestionnaires des données des EPCI ayant les droits d'accès.|

# Droit par profil de connexion

* **Prestataires**

|Fonctionnalités|Lecture|Ecriture|Précisions|
|:---|:-:|:-:|:---|
|Recherches|x||Uniquement recherche par adresse, voie et PEI (sauf par contrat pour ce dernier).|
|Cartographie|x||Visibilité uniquement sur les PEI définis pouyr chaque contrat.|
|Fiche d'information PEI|x|x|Peut modifier uniquement les données accessibles.|

* **Personnes du service métier**

|Fonctionnalités|Lecture|Ecriture|Précisions|
|:---|:-:|:-:|:---|
|Toutes|x||L'ensemble des fonctionnalités (recherches, cartographie, fiches d'informations, ...) sont accessibles par tous les utilisateurs connectés.|
|Fiche d'infromation PEI|x|x|Peut modifier les données PEI.|
|Modification géométrique - Ajouter ou déplacer un PEI|x||Cette fonctionnalité est uniquement visible par les utilisateurs inclus dans les groupes ADMIN et PEI_EDIT. Attention, un PEI déjà saisie sur une commune ne peut pas être déplacé sur une autre commune.|

* **Autres profils**

Sans objet

# Les données

Sont décrites ici les Géotables et/ou Tables intégrées dans GEO pour les besoins de l'application. Les autres données servant d'habillage (pour la cartographie ou les recherches) sont listées dans les autres parties ci-après. Le tableau ci-dessous présente uniquement les changements (type de champ, formatage du résultat, ...) ou les ajouts (champs calculés, filtre, ...) non présents dans la donnée source. 

## GeoTable : `xapps_geo_v_pei_ctr`

|Attributs| Champ calculé | Formatage |Renommage|Particularité/Usage|Utilisation|Exemple|
|:---|:-:|:-:|:---|:---|:---|:---|
|affiche_info_bulle|x|x||Définit le contenu de l'info bulle affichée au survol d'un PEI|Cartographie|`CASE WHEN {id_sdis} is null or {id_sdis} ='' THEN 'N° SDIS : non renseigné' ELSE 'N° SDIS : ' {id_sdis} END`|
|cs_sdis ||x|Centre de secours de 1er appel|Liste de domaine  lt_pei_cs_sdis liée|Fiche d'information PEI||
|date_ct  |||Date du dernier contrôle technique||Fiche d'information PEI||
|date_maj ||x|Date de mise à jour|Prédéfini dd/mm/aaaa|Fiche d'information PEI||
|date_mes   ||x|Date de mise en service|Prédéfini dd/mm/aaaa|Fiche d'information PEI||
|date_sai   ||x|Date de saisie|Prédéfini dd/mm/aaaa|Fiche d'information PEI||
|debit   |||Débit||Fiche d'information PEI||
|debit_max ||Débit Max|||Fiche d'information PEI||
|debit_r_ci  ||Débit de remplissage|||Fiche d'information PEI||
|delegat   ||x|Délégataire|Liste de domaine   lt_pei_delegat liée|Fiche d'information PEI||
|diam_cana    |||Diamètre de canalisation||Fiche d'information PEI||
|diam_pei     |||Diamètre intérieur||Fiche d'information PEI||
|disponible     ||x|Disponible pour la DECI|Liste de domaine lt_pei_etat_boolean liée|Fiche d'information PEI||
|disponible_false      |x|x||Lien http du picto NON DISPONIBLE|Champ calculé `{disponible_img}` ||
|disponible_img       |x|x|Disponible|Champ IMAGE gérant l'affichage du picto selon la disponiblité DECI|Fiche d'information PEI|`CASE WHEN {disponible} = 't' THEN {disponible_true} WHEN {disponible} = 'f' THEN {disponible_false} END `|
|disponible_recherche      |x|x||Format HTML gérant l'affichage des pictos selon la disponiblité DECI des résultats après recherche|résultat recherche|`CASE WHEN {disponible} = 't' THEN '<img src="http://...." width=100 alt="" >' WHEN {disponible} = 'f' THEN '<img src="http://...." width=100 alt="">' END `|
|disponible_false      |x|x||Lien http du picto DISPONIBLE|Champ calculé `{disponible_img}` ||
|epci     |||Nom de l'EPCI||Fiche d'information PEI||
|etat_acces     ||x|Accès conforme|Liste de domaine lt_pei_etat_boolean liée|Fiche d'information PEI||
|etat_anom      ||x|Absence d'anomalie|Liste de domaine lt_pei_etat_boolean liée|Fiche d'information PEI||
|etat_conf      ||x|Conformité technique|Liste de domaine lt_pei_etat_boolean liée|Fiche d'information PEI||
|etat_pei      ||x|Etat|Liste de domaine lt_pei_etat_pei liée|Fiche d'information PEI||
|etat_sign      ||x|Signalisation conforme|Liste de domaine lt_pei_etat_boolean liée|Fiche d'information PEI||
|etiquette      |x|x||Gestion affichage étiquette sur la cartographie au niveau des pictos représentant les PEI|Cartographie |`CASE WHEN {type_pei}='NR' OR {etat_pei}='03' THEN NULL ELSE {type_pei} END`|
|gestion       ||x|Gestionnaire|Liste de domaine lt_pei_gestion liée|Fiche d'information PEI||
|id_contrat       ||x|Référence du contrat de sous traitance|Liste de domaine lt_pei_id_contrat liée|Fiche d'information PEI||
|id_pei       |||Identifiant||Fiche d'information PEI||
|id_sdis        |||n° SDIS||Fiche d'information PEI||
|lt_anom        |||Anomalie(s)||Fiche d'information PEI||
|marque        ||x||Liste de domaine lt_pei_marque liée|Fiche d'information PEI||
|nom_etab        |||Nom de l'établissement||Fiche d'information PEI||
|observ        |||Observations||Fiche d'information PEI||
|ope_ct         |||Opérateur du contrôle||Fiche d'information PEI||
|ope_sai         |||Opérateur de saisie||Fiche d'information PEI||
|photo_url         ||x|Photo|Lien avec texte de remplacement (Cliquez ici pour visualiser le PEI)|Fiche d'information PEI||
|prec         |||Précision||Fiche d'information PEI||
|press_dyn         |||Pression dynamique||Fiche d'information PEI||
|ref_terr          |||Référence sur le terrain||Fiche d'information PEI||
|resultat          |x|x|Résultat|Formatage du contenu informatif du résultat d'une recherche|Résultat de recherches|`CASE WHEN {id_sdis} IS NULL THEN CONCAT('PEI n°',{id_pei}) ELSE CONCAT('PEI n°',{id_pei},' - SDIS n°',{id_sdis}) END`|
|source_pei         |||Source||Fiche d'information PEI||
|src_date         |||Date du référentiel||Fiche d'information PEI||
|src_pei         |||Source de la donnée||Fiche d'information PEI||
|style          |x|x||Création d'un attribut style pour la réprésentation des PEI selon le statut, l'état ou la disponibilité du PEI |Cartographies|`CASE -- pei statut non renseigné WHEN {statut}='00' THEN 'Snr' -- pei statut privé WHEN {statut}='02' AND {etat_pei} IN ('00','01','02') THEN 'Spri_Enr-proj-exi' -- pei statut privé WHEN {statut}='02' AND {etat_pei}='03' THEN 'Spri_Esup' -- pei statut public et état projet ou non renseigné WHEN {statut}='01' AND {etat_pei} IN ('00','01') THEN 'Spub_Enr-proj' -- pei statut public supprimé WHEN {statut}='01' AND {etat_pei}='03' THEN 'Spub_Esup' -- pei statut public existant et dispo non renseignée WHEN {statut}='01' AND {etat_pei}='02' AND {disponible}='0' THEN 'Spub_Eexi_Dnr' -- pei statut public existant et conforme WHEN {statut}='01' AND {etat_pei}='02' AND {disponible}='t' THEN 'Spub_Eexi_Dt' -- pei statut public existant et non conforme WHEN {statut}='01' AND {etat_pei}='02' AND {disponible}='f' THEN 'Spub_Eexi_Df' END `|
|type_pei         |||Type||Fiche d'information PEI||
|type_rd         |||Type dans le règlement départemental||Fiche d'information PEI||
|verrou         ||x||Booléen Oui pour vrai et Non pour faux|Fiche d'information PEI||
|x_l93         |||Coordonnée X (L93)||Fiche d'information PEI||
|y_l93         |||Coordonnée Y (L93)||Fiche d'information PEI||


   * filtres :

|Nom|Attribut| Au chargement | Type | Condition |Valeur|Description|
|:---|:---|:-:|:---:|:---:|:---|:---|
|PEI_SECU_PRESTA|id_contrat|x|Alphanumérique|est égale à une valeur de contaxte|id_presta|Permet de filtrer l'affichage des PEI en fonction des contrats affectés au profil de connexion du ou des prestataires|
|DECI_SECU|insee|x|Alphanumérique|est égale à une valeur de contaxte|ccocom|Permet de filtrer l'affichage des PEI en fonction des communes autorisées pour chaque profil de connexion EPCI|
   
   * relations :

|Géotables ou Tables| Champs de jointure | Type |
|:---|:---|:---|
| xapps_geo_v_pei_ctr_erreur |id_pei| 0..1 (égal) |

   * particularité(s) : aucune

## GeoTable : `xapps_geo_v_pei_zonedefense`

|Attributs| Champ calculé | Formatage |Renommage|Particularité/Usage|Utilisation|Exemple|
|:---|:-:|:-:|:---|:---|:---|:---|

Sans objet

   * filtres :

|Nom|Attribut| Au chargement | Type | Condition |Valeur|Description|
|:---|:---|:-:|:---:|:---:|:---|:---|
|PEI_SECU_PRESTA|id_contrat|x|Alphanumérique|est égale à une valeur de contaxte|id_presta|Permet de filtrer l'affichage des PEI en fonction des contrats affectés au profil de connexion du ou des prestataires|
|DECI_SECU|insee|x|Alphanumérique|est égale à une valeur de contaxte|ccocom|Permet de filtrer l'affichage des PEI en fonction des communes autorisées pour chaque profil de connexion EPCI|

   * relations : aucune

   * particularité(s) : aucune
   
  
## Table : `xapps_geo_v_pei_ctr_erreur`

|Attributs| Champ calculé | Formatage |Renommage|Particularité/Usage|Utilisation|Exemple|
|:---|:-:|:-:|:---|:---|:---|:---|
|affiche_message    |x|x|null|Formate en HTML le message à afficher dans la fiche d'information en cas d'erreur selon un temps définit (évite un affichage permanent du message)|Fiche d'information PEI|`CASE WHEN extract(epoch from  now()::timestamp) - extract(epoch from {horodatage}::timestamp) <= 3 then '<table width=100%><td bgcolor="#FF000"> <font size=4 color="#ffffff"><center><b>' {erreur} '</b></center></font></td></table>' ELSE '' END`|


   * filtres : aucun
   * relations : aucune
   * particularité(s) : aucune
   


# Les fonctionnalités

Sont présentées ici uniquement les fonctionnalités spécifiques à l'application.

## Recherche globale : `Recherche dans la Base Adresse Locale`

Cette recherche permet à l'utilisateur de faire une recherche libre sur une adresse.

Cette recherche a été créée pour l'application RVA. Le détail de celle-ci est donc à visualiser dans le répertoire GitHub rva au niveau de la documentation applicative.

## Recherche globale : `Recherche d'une voie`

Cette recherche permet à l'utilisateur de faire une recherche libre sur le libellé d'une voie.

Cette recherche a été créée pour l'application RVA. Le détail de celle-ci est donc à visualiser dans le répertoire GitHub rva au niveau de la documentation applicative.
 

## Recherche (clic sur la carte) : `PEI par référence`

Cette recherche permet à l'utilisateur de cliquer sur la carte et de remonter les informations du PEI.

  * Configuration :

Source : `xapps_geo_v_pei_ctr`

|Attribut|Afficher|Rechercher|Suggestion|Attribut de géométrie|Tri des résultats|
|:---|:-:|:-:|:-:|:-:|:-:|
|Résultat|x|||||
|Type|x|||||
|Commune|x|||||
|disponible_recherche|x|||||
|geom||||x||
|id_pei|||||x|

(la détection des doublons n'est pas activée ici)

 * Filtres :

|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`OU`||

|Nom|Obligatoire|Attribut|Condition|Valeur|Champ d'affichage (1)|Champ de valeurs (1)|Champ de tri (1)|Ajout autorisé (1)|Particularités|
|:---|:-:|:---|:---|:---|:---|:---|:---|:-:|:---|
|PEI identifiant||id_pei|Prédéfinis - Filtre à valeur saisie||||||Titre : Numéro de PEI|
|PEI identifiant||id_sdis|Prédéfinis - Filtre à valeur saisie||||||Titre : Référence SDIS|

(1) si liste de domaine

 * Fiches d'information active : Fiche d'information PEI
 
## Recherche : `Toutes les recherches cadastrales`

L'ensemble des recherches cadastrales ont été formatées et intégrées par l'éditeur via son module GeoCadastre.
Seul le nom des certaines recherches a été modifié par l'ARC pour plus de compréhension des utilisateurs.

Cette recherche est détaillée dans le répertoire GitHub `docurba`.


## Recherche : `PEI public par disponibilité pour la DECI`

Cette recherche permet à l'utilisateur de faire une recherche guidée sur un PEI public en fonction de sa disponibilité pour la DECI.

  * Configuration :

Source : `xapps_geo_v_pei_ctr`

|Attribut|Afficher|Rechercher|Suggestion|Attribut de géométrie|Tri des résultats|
|:---|:-:|:-:|:-:|:-:|:-:|
|Résultat|x|||||
|Type|x|||||
|Commune|x|||||
|disponible_recherche|x|||||
|geom||||x||
|id_pei|||||x|

(la détection des doublons n'est pas activée ici)

 * Filtres :

|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`ET`||

|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`ET`|x|

|Nom|Obligatoire|Attribut|Condition|Valeur|Champ d'affichage (1)|Champ de valeurs (1)|Champ de tri (1)|Ajout autorisé (1)|Particularités|
|:---|:-:|:---|:---|:---|:---|:---|:---|:-:|:---|
|PEI disponible|x|disponible|Prédéfinis - Filtre à liste de choix||||||Titre : PEI disponible pour la DECI|
|PEI public||statut|Alphanumérique la valeur est égale à une valeur par défaut|00,01||||||

|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`ET`|x|

|Nom|Obligatoire|Attribut|Condition|Valeur|Champ d'affichage (1)|Champ de valeurs (1)|Champ de tri (1)|Ajout autorisé (1)|Particularités|
|:---|:-:|:---|:---|:---|:---|:---|:---|:-:|:---|
|PEI par EPCI||epci|Prédéfinis - Filtre à liste de choix||||||Titre : Nom de l'EPCI|
|PEI par commune||commune|Prédéfinis - Filtre à liste de choix||||||Titre : Nom de la commune|

(1) si liste de domaine

 * Fiches d'information active : Fiche d'information PEI
 
## Recherche : `PEI par date de contrôle`

Cette recherche permet à l'utilisateur de faire une recherche guidée sur un PEI en fonction d'une période de contrôle et d'un découpage administratif ou par rapport au gestionnaire ou d'un contrat.

  * Configuration :

Source : `xapps_geo_v_pei_ctr`

|Attribut|Afficher|Rechercher|Suggestion|Attribut de géométrie|Tri des résultats|
|:---|:-:|:-:|:-:|:-:|:-:|
|Résultat|x|||||
|Type|x|||||
|Commune|x|||||
|disponible_recherche|x|||||
|geom||||x||
|id_pei|||||x|

(la détection des doublons n'est pas activée ici)

 * Filtres :

|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`ET`||

|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`ET`|x|

|Nom|Obligatoire|Attribut|Condition|Valeur|Champ d'affichage (1)|Champ de valeurs (1)|Champ de tri (1)|Ajout autorisé (1)|Particularités|
|:---|:-:|:---|:---|:---|:---|:---|:---|:-:|:---|
|PEI par date de contrôle|x|date_ct|Alphanumérique la valeur est comprise entre une valeur 1 saisie||||||Titre invite 1 : Dernier controle effectué entre et Titre invite 2 : et|


|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`ET`|x|

|Nom|Obligatoire|Attribut|Condition|Valeur|Champ d'affichage (1)|Champ de valeurs (1)|Champ de tri (1)|Ajout autorisé (1)|Particularités|
|:---|:-:|:---|:---|:---|:---|:---|:---|:-:|:---|
|PEI par EPCI||epci|Prédéfinis - Filtre à liste de choix||||||Titre : Nom de l'EPCI|
|PEI par commune||commune|Prédéfinis - Filtre à liste de choix||||||Titre : Nom de la commune|

|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`ET`||

|Nom|Obligatoire|Attribut|Condition|Valeur|Champ d'affichage (1)|Champ de valeurs (1)|Champ de tri (1)|Ajout autorisé (1)|Particularités|
|:---|:-:|:---|:---|:---|:---|:---|:---|:-:|:---|
|PEI gestionnaire||gestion|Prédéfinis à liste de choix||||||Titre : Gestionnaire du PEI|

|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`ET`||

|Nom|Obligatoire|Attribut|Condition|Valeur|Champ d'affichage (1)|Champ de valeurs (1)|Champ de tri (1)|Ajout autorisé (1)|Particularités|
|:---|:-:|:---|:---|:---|:---|:---|:---|:-:|:---|
|PEI Contrat||id_contrat|Alphanumérique la valeur est égale à une valeur de liste de choix |lt_pei_id_contrat|valeur|code|valeur|x|Titre : N° de contrat et filtre non utilisable par le ou les prestataire(s)|

(1) si liste de domaine

 * Fiches d'information active : Fiche d'information PEI

## Recherche : `PEI par gestionnaire`

Cette recherche permet à l'utilisateur de faire une recherche guidée sur un PEI en fonciton du gestionnaire et du découpage administratif.

  * Configuration :

Source : `xapps_geo_v_pei_ctr`

|Attribut|Afficher|Rechercher|Suggestion|Attribut de géométrie|Tri des résultats|
|:---|:-:|:-:|:-:|:-:|:-:|
|Résultat|x|||||
|Type|x|||||
|Commune|x|||||
|disponible_recherche|x|||||
|geom||||x||
|id_pei|||||x|

(la détection des doublons n'est pas activée ici)

 * Filtres :

|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`ET`|x|

|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`ET`|x|

|Nom|Obligatoire|Attribut|Condition|Valeur|Champ d'affichage (1)|Champ de valeurs (1)|Champ de tri (1)|Ajout autorisé (1)|Particularités|
|:---|:-:|:---|:---|:---|:---|:---|:---|:-:|:---|
|PEI par EPCI|x|epci|Prédéfinis - Filtre à liste de choix||||||Titre : Nom de l'EPCI|
|PEI par commune||commune|Prédéfinis - Filtre à liste de choix||||||Titre : Nom de la commune|

|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`ET`||

|Nom|Obligatoire|Attribut|Condition|Valeur|Champ d'affichage (1)|Champ de valeurs (1)|Champ de tri (1)|Ajout autorisé (1)|Particularités|
|:---|:-:|:---|:---|:---|:---|:---|:---|:-:|:---|
|PEI gestionnaire|x|gestion|Prédéfinis à liste de choix||||||Titre : Gestionnaire du PEI|

(1) si liste de domaine

 * Fiches d'information active : Fiche d'information PEI
 
 ## Recherche : `PEI par état d'actualité`

Cette recherche permet à l'utilisateur de faire une recherche guidée sur un PEI par son état d'actualité et un découpage adminsitratif.

  * Configuration :

Source : `xapps_geo_v_pei_ctr`

|Attribut|Afficher|Rechercher|Suggestion|Attribut de géométrie|Tri des résultats|
|:---|:-:|:-:|:-:|:-:|:-:|
|Résultat|x|||||
|Type|x|||||
|Commune|x|||||
|disponible_recherche|x|||||
|geom||||x||
|id_pei|||||x|

(la détection des doublons n'est pas activée ici)

 * Filtres :

|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`ET`||


|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`ET`|x|

|Nom|Obligatoire|Attribut|Condition|Valeur|Champ d'affichage (1)|Champ de valeurs (1)|Champ de tri (1)|Ajout autorisé (1)|Particularités|
|:---|:-:|:---|:---|:---|:---|:---|:---|:-:|:---|
|PEI par état d'actualité|x|etat_pei|Prédéfinis à liste de choix||||||Titre : Etat d'actualité du PEI|

|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`ET`|x|

|Nom|Obligatoire|Attribut|Condition|Valeur|Champ d'affichage (1)|Champ de valeurs (1)|Champ de tri (1)|Ajout autorisé (1)|Particularités|
|:---|:-:|:---|:---|:---|:---|:---|:---|:-:|:---|
|PEI par EPCI||epci|Prédéfinis - Filtre à liste de choix||||||Titre : Nom de l'EPCI|
|PEI par commune||commune|Prédéfinis - Filtre à liste de choix||||||Titre : Nom de la commune|


(1) si liste de domaine

 * Fiches d'information active : Fiche d'information PEI
 
## Recherche : `PEI par caractéristiques techniques`

Cette recherche permet à l'utilisateur de faire une recherche guidée sur un PEI par ses caractéristiques techniques et un découpage adminsitratif.

  * Configuration :

Source : `xapps_geo_v_pei_ctr`

|Attribut|Afficher|Rechercher|Suggestion|Attribut de géométrie|Tri des résultats|
|:---|:-:|:-:|:-:|:-:|:-:|
|Résultat|x|||||
|Type|x|||||
|Commune|x|||||
|disponible_recherche|x|||||
|geom||||x||
|id_pei|||||x|

(la détection des doublons n'est pas activée ici)

 * Filtres :

|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`ET`||


|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`ET`||

|Nom|Obligatoire|Attribut|Condition|Valeur|Champ d'affichage (1)|Champ de valeurs (1)|Champ de tri (1)|Ajout autorisé (1)|Particularités|
|:---|:-:|:---|:---|:---|:---|:---|:---|:-:|:---|
|PEI par type||type_pei|Prédéfinis à liste de choix||||||Titre : Type de PEI|
|PEI par diam_pei||diam_pei|Prédéfinis à liste de choix||||||Titre : Diamètre intérieur|
|PEI par source||source|Prédéfinis à liste de choix||||||Titre : Source du point d'eau|
|PEI par raccord||raccord|Prédéfinis à liste de choix||||||Titre : Raccords de sortie|
|PEI par marque||marque|Prédéfinis à liste de choix||||||Titre : Marque du matériel|

|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`ET`|x|

|Nom|Obligatoire|Attribut|Condition|Valeur|Champ d'affichage (1)|Champ de valeurs (1)|Champ de tri (1)|Ajout autorisé (1)|Particularités|
|:---|:-:|:---|:---|:---|:---|:---|:---|:-:|:---|
|PEI par EPCI||epci|Prédéfinis - Filtre à liste de choix||||||Titre : Nom de l'EPCI|
|PEI par commune||commune|Prédéfinis - Filtre à liste de choix||||||Titre : Nom de la commune|

|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`ET`|x|

|Nom|Obligatoire|Attribut|Condition|Valeur|Champ d'affichage (1)|Champ de valeurs (1)|Champ de tri (1)|Ajout autorisé (1)|Particularités|
|:---|:-:|:---|:---|:---|:---|:---|:---|:-:|:---|
|PEI gestionnaire||gestion|Prédéfinis à liste de choix||||||Titre : Gestionnaire du PEI|

|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`ET`|x|

|Nom|Obligatoire|Attribut|Condition|Valeur|Champ d'affichage (1)|Champ de valeurs (1)|Champ de tri (1)|Ajout autorisé (1)|Particularités|
|:---|:-:|:---|:---|:---|:---|:---|:---|:-:|:---|
|PEI Contrat||id_contrat|Alphanumérique la valeur est égale à une valeur de liste de choix |lt_pei_id_contrat|valeur|code|valeur|x|Titre : N° de contrat et filtre non utilisable par le ou les prestataire(s)|

(1) si liste de domaine

 * Fiches d'information active : Fiche d'information PEI
 
## Recherche : `PEI par commune`

Cette recherche permet à l'utilisateur de faire une recherche guidée sur un PEI par sa commune de positionnement.

  * Configuration :

Source : `xapps_geo_v_pei_ctr`

|Attribut|Afficher|Rechercher|Suggestion|Attribut de géométrie|Tri des résultats|
|:---|:-:|:-:|:-:|:-:|:-:|
|Résultat|x|||||
|Type|x|||||
|Commune|x|||||
|disponible_recherche|x|||||
|geom||||x||
|id_pei|||||x|

(la détection des doublons n'est pas activée ici)

 * Filtres :

|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`ET`|x|


|Groupe|Jointure|Filtres liés|
|:---|:-:|:-:|
|Groupe de filtres par défaut|`ET`|x|

|Nom|Obligatoire|Attribut|Condition|Valeur|Champ d'affichage (1)|Champ de valeurs (1)|Champ de tri (1)|Ajout autorisé (1)|Particularités|
|:---|:-:|:---|:---|:---|:---|:---|:---|:-:|:---|
|PEI par EPCI||epci|Prédéfinis - Filtre à liste de choix||||||Titre : Nom de l'EPCI|
|PEI par commune||commune|Prédéfinis - Filtre à liste de choix||||||Titre : Nom de la commune|

(1) si liste de domaine

 * Fiches d'information active : Fiche d'information PEI
 
 ## Recherche : `PEI par contrat`

Cette recherche permet à l'utilisateur de faire une recherche guidée sur un PEI par son contrat de contrôle.
Ce filtre n'est pas accessible au(x) prestataire(s).

  * Configuration :

Source : `xapps_geo_v_pei_ctr`

|Attribut|Afficher|Rechercher|Suggestion|Attribut de géométrie|Tri des résultats|
|:---|:-:|:-:|:-:|:-:|:-:|
|Résultat|x|||||
|Type|x|||||
|Commune|x|||||
|disponible_recherche|x|||||
|geom||||x||
|id_pei|||||x|

(la détection des doublons n'est pas activée ici)

 * Filtres :

|Nom|Obligatoire|Attribut|Condition|Valeur|Champ d'affichage (1)|Champ de valeurs (1)|Champ de tri (1)|Ajout autorisé (1)|Particularités|
|:---|:-:|:---|:---|:---|:---|:---|:---|:-:|:---|
|PEI Contrat|x|id_contrat|Alphanumérique la valeur est égale à une valeur de liste de choix |lt_pei_id_contrat|valeur|code|valeur|x|Titre : N° de contrat et filtre non utilisable par le ou les prestataire(s)|

(1) si liste de domaine

 * Fiches d'information active : Fiche d'information PEI

## Fiche d'information : `Fiche d'information PEI`

Source : `xapps_geo_v_pei_ctr`

* Statistique : aucune
 
 * Représentation :
 
|Mode d'ouverture|Taille|Agencement des sections|
|:---|:---|:---|
|dans le gabarit|530x650|Accordéon|

|Nom de la section|Attributs|Position label|Agencement attribut|Visibilité conditionnelle|Fichie liée|Ajout de données autorisé|
|:---|:---|:---|:---|:---|:---|:---|
|Général|affiche_message|masqué|Vertical||||

|Nom de la sous-section|Attributs|Position label|Agencement attribut|Visibilité conditionnelle|Fichie liée|Ajout de données autorisé|
|:---|:---|:---|:---|:---|:---|:---|
|(vide)|n° SDIS (id_sdis),Identifiant(id_pei),Verrou (verrou),Référence sur le terrain (ref_terr),Nom de l'EPCI (epci),Insee (insee),Commune (commune),Type (type_pei),Type dans le règlement départemental (type_rd),Situation (situation),Disponible (disponible_img),Etat (etat_pei)|à gauche|Vertical||||

|Nom de la section|Attributs|Position label|Agencement attribut|Visibilité conditionnelle|Fichie liée|Ajout de données autorisé|
|:---|:---|:---|:---|:---|:---|:---|
|Caractéristiques techniques|affiche_message|masqué|Vertical||||

|Nom de la sous-section|Attributs|Position label|Agencement attribut|Visibilité conditionnelle|Fichie liée|Ajout de données autorisé|
|:---|:---|:---|:---|:---|:---|:---|
|(vide)|Diamètre intérieur (diam_int),Raccord(raccord),Marque (marque),Source (source_pei),Date de mise en service (date_mes)|à gauche|Vertical||||


|Nom de la section|Attributs|Position label|Agencement attribut|Visibilité conditionnelle|Fichie liée|Ajout de données autorisé|
|:---|:---|:---|:---|:---|:---|:---|
|Mesures et contrôle|affiche_message|masqué|Vertical||||

|Nom de la sous-section|Attributs|Position label|Agencement attribut|Visibilité conditionnelle|Fichie liée|Ajout de données autorisé|
|:---|:---|:---|:---|:---|:---|:---|
|(vide)|Référence du contrat de sous-traitance (id_contrat)|à gauche|Vertical||||

|Nom de la sous-section|Attributs|Position label|Agencement attribut|Visibilité conditionnelle|Fichie liée|Ajout de données autorisé|
|:---|:---|:---|:---|:---|:---|:---|
|Mesure||à gauche|Vertical||||

|Nom de la sous-sous-section|Attributs|Position label|Agencement attribut|Visibilité conditionnelle|Fichie liée|Ajout de données autorisé|
|:---|:---|:---|:---|:---|:---|:---|
|(vide)|Press statique (press_stat),Pression dynamique (press_dyn),Débit (debit),Débit max (debit_max)|à gauche|Vertical|type_pei=='CI' et (type_pei=='PA' && source != 'CE')|||

|Nom de la sous-sous-section|Attributs|Position label|Agencement attribut|Visibilité conditionnelle|Fichie liée|Ajout de données autorisé|
|:---|:---|:---|:---|:---|:---|:---|
|(vide)|Volmume (press_stat),Débit de remplissage (press_dyn)|à gauche|Vertical|type_pei=='CI' et (type_pei=='PA' && source != 'CE')|||


|Nom de la sous-section|Attributs|Position label|Agencement attribut|Visibilité conditionnelle|Fichie liée|Ajout de données autorisé|
|:---|:---|:---|:---|:---|:---|:---|
|Anomalies|Absence d'anomalie|à gauche|Vertical||||


|Nom de la sous-sous-section|Attributs|Position label|Agencement attribut|Visibilité conditionnelle|Fichie liée|Ajout de données autorisé|
|:---|:---|:---|:---|:---|:---|:---|
|(vide)|Anomalie(s) (lt_anom)|à gauche|Vertical|etat_anom='f'|||

|Nom de la sous-section|Attributs|Position label|Agencement attribut|Visibilité conditionnelle|Fichie liée|Ajout de données autorisé|
|:---|:---|:---|:---|:---|:---|:---|
|Conformité|Accès conforme (etat_access), Signalisation conforme (etat_sign),Conformité technique (etat_conf),Date du dernier contrôle technique (date_ct),disponible pour la DECI (disponible)|à gauche|Vertical||||

|Nom de la sous-section|Attributs|Position label|Agencement attribut|Visibilité conditionnelle|Fichie liée|Ajout de données autorisé|
|:---|:---|:---|:---|:---|:---|:---|
|Opérateur|Opérateur du contrôle (ope_ct)|à gauche|Vertical||||

|Nom de la section|Attributs|Position label|Agencement attribut|Visibilité conditionnelle|Fichie liée|Ajout de données autorisé|
|:---|:---|:---|:---|:---|:---|:---|
|Gestion|affiche_message|masqué|Vertical||||

|Nom de la sous-section|Attributs|Position label|Agencement attribut|Visibilité conditionnelle|Fichie liée|Ajout de données autorisé|
|:---|:---|:---|:---|:---|:---|:---|
|(vide)|Statut (statut), Nom de l'établissement (nom_etab),Gestionnaire (gestion),Délégataire (delegat),Centre de secours de 1er appel (cs_sdis)|à gauche|Vertical||||

|Nom de la section|Attributs|Position label|Agencement attribut|Visibilité conditionnelle|Fichie liée|Ajout de données autorisé|
|:---|:---|:---|:---|:---|:---|:---|
|Qualité|affiche_message|masqué|Vertical||||

|Nom de la sous-section|Attributs|Position label|Agencement attribut|Visibilité conditionnelle|Fichie liée|Ajout de données autorisé|
|:---|:---|:---|:---|:---|:---|:---|
|(vide)|Observations (observ), Source de la donnée (src_pei),Photo (photo_url),Coordonnées X (L93) (x_l93),Coordonnées Y (L93) (y_l93), Référentiel géographique ()|à gauche|Vertical||||

|Nom de la sous-section|Attributs|Position label|Agencement attribut|Visibilité conditionnelle|Fichie liée|Ajout de données autorisé|
|:---|:---|:---|:---|:---|:---|:---|
|(vide)|Date du référentiel (src_date), Précision (prec)|à gauche|Vertical|src_geom != '00'|||

|Nom de la sous-section|Attributs|Position label|Agencement attribut|Visibilité conditionnelle|Fichie liée|Ajout de données autorisé|
|:---|:---|:---|:---|:---|:---|:---|
|(vide)|Opérateur de saisie (ope_sai), Date de saisie (date_sai), Date de mise à jour (date_maj)|à gauche|Vertical||||

* Saisie :

Sont présent ici uniquement les attributs éditables ou disposant d'un mode de représentation spécifique.

|Attribut|Obligatoire|Valeur par défaut|Liste de domaine|Représentation|
|:---|:---|:---|:---|:---|
|n° SDIS|||||
|Type|x|NR|lt_pei_type_pei|Liste de choix|
|Diamètre intérieur||NR|lt_pei_diam_pei|Liste de choix|
|Raccord ||00|lt_pei_raccord|Liste de choix|
|Marque ||00|lt_pei_marque|Liste de choix|
|Source ||NR|lt_pei_source|Liste de choix|
|Volume |||||
|Diamètre de canalisation|||||
|Etat|x|00|lt_pei_etat_pei|Liste de choix|
|Press statique|||||
|Pression dynamique|||||
|Débit |||||
|Débit Max |||||
|Debit de remplissage |||||
|Absence d'anomalie ||0|lt_pei_etat_boolean|Liste de choix|
|Signalisation conforme ||0|lt_pei_etat_boolean|Liste de choix|
|Date de disponibilité |||||
|Date de mise en service |||||
|Date du dernier contrôle technique |||||
|Opérateur du contrôle |||||
|Date de la dernière reconnaissanve opérationnelle |||||
|Statut  ||00|lt_pei_statut|Liste de choix|
|Gestionnaire   ||00|lt_pei_gestion|Liste de choix|
|Délégataire   ||00|lt_pei_delegat|Liste de choix|
|Centre de secours de 1er appel   ||00000|lt_pei_cs_sdis|Liste de choix|
|Situation   |||||
|Observations    ||||Champ texte à plusieurs lignes|
|Photo     |||||
|Source de la donnée     |||||
|Référentiel géographique     ||00|valeur_src_geom|Liste de choix|
|Date du référentiel      ||0000|||
|Opérateur de saisie     ||%USER_LOGIN%||| 
|Anomalie(s)      |||lt_pei_anomalie|Cases à cocher multiples| 
|Type dans le règlement départemental   |||||
|Précision   |||||
|Référence sur le terrain   |||||
|Référence du contrat de sous-traitance  |||||
|Type dans le règlement départemental   ||00|lt_pei_id_contrat|Liste de choix|
|Verrou    |x|false||Caser à cocher|

**IMPORTANT** : L'édition des données jointes est désactivée.

 * Modèle d'impression : Fiche standard + carte et fiche standard

 
## Analyse :

Aucune

## Statistique :

Aucune

## Modification géométrique : `Ajouter ou déplacer un PEI`

Cette recherche permet à l'utilisateur de saisir ou modifier l'emplacement d'un PEI.
Cette fonctionnalité n'est accessible au(x) prestataire(s).

  * Configuration :
  
Source : `xapps_geo_v_pei_ctr`

 * Filtres : aucun
 * Accrochage : aucun
 * Fiches d'information active : Fiche d'informationPEI
 * Topologie : aucune 
 
 # La cartothèque

|Groupe|Sous-groupe|Visible dans la légende|Visible au démarrage|Détails visibles|Déplié par défaut|Geotable|Renommée|Issue d'une autre carte|Visible dans la légende|Visible au démarrage|Déplié par défaut|Couche sélectionnable|Couche accrochable|Catégorisation|Seuil de visibilité|Symbologie|Autres|
|:---|:---|:-:|:-:|:-:|:-:|:---|:---|:-:|:-:|:-:|:-:|:-:|:---|:---|:---|:---|:---|
|Défense incendie||x|x|x|x|xapps_geo_v_pei_ctr|Point d'Eau Incendie|||x||||style|1/120000-1/140000|pei_picto_[].svg selon la catégorie,taille 16|Interactivité avec le champ calculé affiche_info_bulle|
|||||||xapps_geo_v_pei_ctr|Point d'Eau Incendie||x|x|x|||style|0-1/20000|pei_picto_[].svg selon la catégorie,taille 35|Interactivité avec le champ calculé affiche_info_bulle|
|||||||xapps_geo_v_pei_zonedefense|Zone de défense publique||x|x|||x||0-1/25000|Fond #BBBBBB 5% d'opacité et contour #343434 4ème symbole de tiret épaisseur 1||
|||||||xapps_geo_zone_gestion|Zone de gestion||x||x|||gest||Fond #6699BB 25% opaque et coutour noir pour ARC, Fond #FFFABB 25% opaque et coutour noir pour Compiègne|Interactivité avec le champ calculé affiche_info_bulle|
|Foncier||x||x||geo_v_fon_proprio_pu_arc|Propriétés institutionnelles|x|x||x||x|||Cf carte CADASTRE||
|Limites administratives||x|x|x||geo_v_osm_commune_apc|Limites communales|x|x|x|||x|||Cf Navigateur cartographique (sauf épaisseur contour à 2||
|||||||geo_adm_quartier|Quartiers de Compiègne|x|x|x|||x||0 - 1/70000|Cf Navigateur cartographique (sauf épaisseur contour à 2||
|Cadastre|||x|||Parcelle (V3)|Parcelles V3|||||||||Cf carte CADASTRE||

# L'application

* Généralités :

|Gabarit|Thème|Modules spé|Impression|Résultats|
|:---|:---|:---|:---|:---|
|Pro|Thème GeoCompiegnois 1.0.7|Partage de lien, Introduction, Coordonnées au survol, StreetView, Export Géotables (générique geotable de l'apps, Export Fonctionnalités (générique de fonctions de l'apps), GeoCadastre (V3),Google Analytics,Page de connexion perso, javascript|A4 - Portrait et A4 - Paysage+légende||

* Particularité de certains modules :
  * Module introduction : ce menu s'ouvre automatiquement à l'ouverture de l'application grâce un code dans le module javascript. Ce module contient une introduction sur l'application, et des liens vers des fiches d'aide.
  * Module javacript : 
  `var injector = angular.element('body').injector();
var acfApplicationService = injector.get('acfApplicationService');
acfApplicationService.whenLoaded(setTimeout(function(){
$(".sidepanel-item.launcher-application").click();
}, 100));`
  * Module Google Analytics : le n° ID est disponible sur le site de Google Analytics
  * Module Export Fonctionnalité : ce module permet l'export des données issues des recherches

* Recherche globale :

|Noms|Tri|Nb de sugggestion|Texte d'invite|
|:---|:---|:---|:---|
|Recherche dans la Base Adresse Locale,Recherche d'une voie|alpha|20|Rechercher une adresse ou une une voie|

* Carte : `DECI`

Comportement au clic : (dés)active uniquement l'item cliqué
Liste des recherches : PEI par référence

* Fonds de plan :

|Nom|Au démarrage|opacité|
|:---|:---|:---|
|Cadastre||100%|
|Plan de ville||100%|
|Photographie aérienne 2013|x|70%|

* Fonctionnalités

|Groupe|Nom|
|:---|:---|
|Rechercher un PEI||
||PEI par référence|
||PEI public par disponibilité pour la DECI|
||PEI par date de contrôle|
||PEI par gestionnaire|
||PEI par état d'actualité|
||PEI par caractéristiques techniques|
||PEI par commune|
||PEI par par contrat|
|Ajouter ou déplacer un PEI||
|Recherche cadastrale||
||Parcelles par référence|
||Parcelles par adresse fiscale|
||Parcelles par nom du propriétaire|
||Parcelles multicritères|
||Parcelles par nom du propriétaire d'un local|
||Parcelles par surface|
|Recherche avancée d'une adresse||
||Recherche avancée d'une adresse|
|Recherche avancée d'une voie||
||Recherche avancée d'une voie|
