# Contrôleur automatique de ventilateur de plafond

[![Ouvrir votre instance Home Assistant et afficher la boîte de dialogue d'import du blueprint.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fnicolinuxfr%2Fauto-ceiling-fan-blueprint%2Fgh-pages%2Ffr%2Fceiling_fan.yaml)

**URL directe (copier-coller) :**
```
https://raw.githubusercontent.com/nicolinuxfr/auto-ceiling-fan-blueprint/gh-pages/fr/ceiling_fan.yaml
```

Ce blueprint contrôle automatiquement un ventilateur de plafond en fonction de la différence de température (delta) entre le plafond et l'ambiance. Il aide à homogénéiser la température d'une pièce en faisant tourner le ventilateur à la bonne vitesse et dans le bon sens.

## Comment ça fonctionne

1. Le blueprint calcule un delta de température entre les capteurs plafond et ambiance.
2. Il détermine le sens de rotation du ventilateur :
   - **Sans entité de mode PAC** : le sens est basé sur la saison courante, détectée automatiquement depuis la localisation de votre instance Home Assistant (hémisphère nord : novembre–mars = inversé ; hémisphère sud : mai–septembre = inversé).
   - **Avec entité de mode PAC** : le sens est contrôlé par une entité `input_select`.
3. Il ajuste la vitesse du ventilateur selon des seuils configurables :
   - Delta sous le **seuil d'arrêt** (défaut 0.8°C) : ventilateur arrêté.
   - Delta dans la **zone d'hystérésis** (seuil arrêt → seuil démarrage) : **vitesse basse** (16%).
   - Delta au-dessus du **seuil de démarrage** (défaut 2.0°C) : **vitesse moyenne** (33%).
   - Delta au-dessus du **seuil de haute vitesse** (défaut 5.0°C) : **haute vitesse** (67%).
4. **Mode silencieux** optionnel : quand actif (ex. TV allumée), le seuil de démarrage est relevé pour éviter le bruit.

## Sources de température

Sélectionnez un ou plusieurs capteurs de température et/ou groupes de capteurs. L'automatisation utilise `expand()` pour résoudre automatiquement les membres des groupes, puis calcule le min et le max de toutes les valeurs disponibles.

- **Un groupe de capteurs** : l'automatisation développe ses membres et extrait toutes les valeurs.
- **Plusieurs capteurs individuels** : l'automatisation utilise directement toutes leurs valeurs.
- **Un mélange des deux** : toutes les valeurs sont regroupées.

Au moins 2 valeurs de température distinctes sont nécessaires. Si une seule valeur est disponible, l'automatisation ne se déclenche pas.

## Options de configuration

| Input | Description | Défaut |
|---|---|---|
| Ventilateur | Le ventilateur de plafond à contrôler | — |
| Capteurs de température | Capteurs de température et/ou groupes de capteurs | — |
| Entité mode PAC | input_select optionnel (chauffage/clim/éteint) | — |
| État chauffage | Valeur de l'état pour le mode chauffage | `Chauffage` |
| État climatisation | Valeur de l'état pour le mode climatisation | `Climatisation` |
| État éteint | Valeur de l'état pour le mode éteint | `Éteint` |
| Seuil d'arrêt | Delta en-dessous duquel le ventilateur s'arrête | 0.8°C |
| Seuil de démarrage normal | Delta au-dessus duquel le ventilateur démarre | 2.0°C |
| Seuil de démarrage silencieux | Delta au-dessus duquel il démarre (mode silencieux) | 3.5°C |
| Seuil de haute vitesse | Delta au-dessus duquel il tourne à haute vitesse | 5.0°C |
| Vitesse basse | Vitesse dans la zone d'hystérésis | 16% |
| Vitesse moyenne | Vitesse en fonctionnement normal | 33% |
| Haute vitesse | Vitesse quand le delta est élevé | 67% |
| Entité mode silencieux | input_boolean ou binary_sensor (ex. TV allumée) | — |

## Limitations connues

- Les blueprints ne supportent pas la validation d'un nombre minimum de sélections : l'interface exige au moins 1 capteur, mais l'automatisation a besoin d'au moins 2 valeurs de température pour calculer un delta. Si les valeurs sont insuffisantes, l'automatisation ne se déclenche tout simplement pas.
- La modification du sens de rotation peut ne pas être supportée par toutes les intégrations de ventilateurs quand le ventilateur est arrêté.
