# TransportBLV — Registre de Transport

Application mobile-first pour l'enregistrement et le dispatching des camions (charbon, sable, gypse, terre rouge) vers les clients miniers.
Une seule page HTML autonome : `index.html`. Interface en **français** (par défaut), **néerlandais** et **anglais**.

## Modules

1. **Identification**: chauffeur, téléphone, société de transport, immatriculation (camions récents en un tap).
2. **Suivi opérationnel**: client, produit, tonnage au départ / déchargé, lieux et heures de départ, d'arrivée prévue et réelle, kilométrage, photos des bons (chargement et livraison), voyages planifiés.
3. **Gestion financière**: prix d'achat à la tonne, fret transporteur par tonne, péages routier et minier, coût des retards, prix de vente (par client ou prix du marché).
4. **Analyse & dispatching**: coût au km, taux de livraison à temps par transporteur, pertes en route, marge, avancement des contrats par client avec rythme requis (t/jour, camions/jour), alertes, simulateur de prix du marché.

Plus :
- **Mode chauffeur**: écran simplifié (départ / arrivée / tonnage / photo), sans prix ni marges.
- **Hors ligne**: les voyages et photos sont gardés sur le téléphone et envoyés automatiquement au retour du réseau.
- **Rapport mensuel**: par client et transporteur, export **PDF** et CSV (Excel).

## Données & hébergement

L'application est publiée comme artefact sur claude.ai et utilise ses capacités :

| Capacité | Usage |
|---|---|
| `db` | voyages (`voyages/*`), clients (`config/clients`), prix / tarifs / alertes (`config/prix`), photos de secours (`photos/*`) |
| `assets` (`upload: "interact"`) | photos des bons |
| `downloads` | rapport PDF |
| `user` | droits d'écriture |

Ouvert en dehors de claude.ai (par ex. en local), le fichier fonctionne en **mode local** : les données restent dans le `localStorage` du navigateur, et le PDF n'est pas disponible.

Bibliothèques chargées à la demande (cdnjs) : jsPDF 2.5.1 et jspdf-autotable 3.8.2.

## Calculs

- Coût total = tonnage départ × prix d'achat + tonnage départ × fret/t + péages + coût des retards
- Coût / km = coût total ÷ km
- Revenu = tonnage déchargé × prix de vente (voyage → client → marché)
- À temps = arrivée réelle ≤ arrivée prévue
- Perte = tonnage départ − tonnage déchargé
- Rythme requis client = reste à livrer ÷ jours restants avant l'échéance ; rythme actuel = moyenne livrée sur 14 jours

## Développement

Pas de build : modifier `index.html` puis republier l'artefact.
