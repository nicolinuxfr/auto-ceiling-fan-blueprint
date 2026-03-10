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
   - **Sans entité de mode PAC** : le sens est basé sur la saison courante (hémisphère nord — novembre–mars = inversé pour le chauffage, avril–octobre = normal pour la clim).
   - **Avec entité de mode PAC** : le sens est contrôlé par une entité `input_select`.
3. Il ajuste la vitesse du ventilateur selon des seuils configurables :
   - Delta sous le **seuil d'arrêt** (défaut 0.8°C) : ventilateur arrêté.
   - Delta dans la **zone d'hystérésis** (seuil arrêt → seuil démarrage) : **vitesse basse** (16%).
   - Delta au-dessus du **seuil de démarrage** (défaut 2.0°C) : **vitesse moyenne** (33%).
   - Delta au-dessus du **seuil de haute vitesse** (défaut 5.0°C) : **haute vitesse** (67%).
4. **Mode silencieux** optionnel : quand actif (ex. TV allumée), le seuil de démarrage est relevé pour éviter le bruit.

## Sources de température (choisir une)

- **Groupe min_max** (recommandé) : un helper Home Assistant `min_max` qui regroupe les deux capteurs. Le delta est `max_value − min_value`.
- **Deux capteurs séparés** : fournir directement un capteur plafond et un capteur ambiance.

Si les deux sont configurés, le groupe a la priorité.

## Options de configuration

| Input | Description | Défaut |
|---|---|---|
| Ventilateur | Le ventilateur de plafond à contrôler | — |
| Groupe de température | Un groupe de capteurs min_max | — |
| Capteur plafond | Température près du plafond | — |
| Capteur ambiance | Température à hauteur d'ambiance | — |
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

- La détection de direction par saison suppose un emplacement en **hémisphère nord**. Les utilisateurs de l'hémisphère sud doivent configurer une entité `input_select` de mode PAC.
- Les blueprints ne supportent pas les inputs optionnels mutuellement exclusifs : les deux options (groupe et deux capteurs) sont affichées, mais une seule doit être remplie (le groupe a la priorité).
- La modification du sens de rotation peut ne pas être supportée par toutes les intégrations de ventilateurs quand le ventilateur est arrêté.
