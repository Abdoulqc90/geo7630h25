## Identification de la problématique:
# accessibilité des aires de stationnement pendant les périodes de déneigement de la ville de Montréal pour la saison 2024-2025.

# 1** Contexte général
Stationner sa voiture à Montréal peut représenter un véritable casse-tête tant pour les usagers de la route que pour les autorités de la ville, encore plus durant la saison de l’hiver. Selon le livre blanc sur le stationnement à Montréal, les aires de stationnement occupent plus de 22,4 km² sur l’île de Montréal, soit un peu plus que l’arrondissement de Côte des Neiges Notre-Dame de Grâce (21,4 km²). Malheureusement la méconnaissance de la répartition spatiale de ces zones pendant l’hiver et des conditions de stationnement entraînent des désagréments chez les usagers. Les autorités municipales quant à elles voient leur programme de déneigement perturbé par le mauvais stationnement.
Au regard de ce qui précède, il se pose la question de:
Comment garantir l’accessibilité des aires de stationnement à Montréal tout en assurant une gestion efficace du déneigement pendant l’hiver ?
La réponse à cette question passe nécessairement par le développement d’une application pour une meilleure prise en compte de tous ces éléments.
# 2**Les données et analyses prévues concernant la problématique des stationnements de Montréal durant les périodes de déneigement.
Afin de procéder à la création d’une application pouvant répondre à la problématique ci-dessus, nous nous appuierons sur les données ouvertes fournies par le site web de la ville de Montréal pour la saison 2024-2025. Il s’agit des données de type CSV et GEOJSON. Les grandes lignes de ces analyses consisteront à :
•	Cartographier les zones de stationnements dans la ville de Montréal;
•	Identifier le nombre de stationnement gratuits et payants disponibles dans cette zone;
•	Intégrer les signalisations de restrictions de stationnements pendant le déneigement.
Toutes ces opérations se feront à l’aide de l’outil FME Workbench 2024.2.2. Les fonctionnalités reader, attribute keeper, attribute filter, pointonareaoverlayer et attributecreator seront éventuellement utilisées pour l’intégration et le traitement des données.

# 3** Visualisation
La visualisation des données traitées se fera à l’aide de l’outil QGIS. La carte ainsi obtenue se présentera sous forme d’application web ou mobile telle qu’indiquée ci-dessous:
![alt text](<Capture d’écran 2025-01-26 151817-1.png>)
![alt text](<Capture d’écran 2025-01-26 151536.png>)
![alt text](<Capture d’écran 2025-01-26 152832.png>)


# 4** Sources
https://carrefour.vivreenville.org/publication/problematique-du-stationnement
https://www.journaldemontreal.com/2024/01/22/les-defis-du-deneigement-a-montreal
https://ici.radio-canada.ca/nouvelle/1093533/deneigeurs-remorquage-milliers-vehicules-hors-heures-montreal
https://donnees.montreal.ca/dataset/stationnements-deneigement
2023-03-02_CRE-Montreal_Livre-blanc-VF_numerique.pdf
https://play.google.com/store/apps/details?id=com.heritagesoftware.infoneige&pli=1
https://services.montreal.ca/deneigement/carte