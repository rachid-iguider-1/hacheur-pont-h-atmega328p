# hacheur-pont-h-atmega328p
Pilotage d'un hacheur en pont en H avec ATmega328P (Bare-Metal) et supervision LCD I2C
# Pilotage de Hacheur en Pont en H — ATmega328P (Bare-Metal)

Ce projet implémente le contrôle d'un étage de puissance pour moteur CC via un hacheur en pont en H. Le code est développé en **C/C++ Bare-Metal** sur microcontrôleur ATmega328P (Arduino Uno) avec supervision en temps réel sur écran LCD I2C.

---

## 🛠️ Spécifications Techniques

* **Microcontrôleur :** ATmega328P (16 MHz)
* **Génération PWM :** Timer 1 (16 bits) en mode *Fast PWM* (Mode 14) à **10 kHz**
* **Sécurité Bootstrap :** Bridage matériel du rapport cyclique à **90 % max** (temps de recharge des condensateurs de driver type IR2104)
* **Protections :** Verrouillage logiciel anti-court-circuit transversal (*Shoot-Through*)
* **Interface Utilisateur :**
  * Potentiomètre sur canal ADC0 (lecture 10 bits avec zone morte < 0.24V)
  * Boutons-poussoirs sur registres `PIND` (anti-rebond logiciel et machine à états)
  * Affichage LCD 16x2 I2C (cadencement du rafraîchissement à 200 ms)

---

## 💻 Architecture du Code

Le programme est structuré autour de registres matériels AVR manipulés directement :

* **`init_PWM()` :** Configuration de `TCCR1A`, `TCCR1B` et `ICR1` pour fixer $F_{PWM} = 10\text{ kHz}$ sur `OC1A` (PB1) et `OC1B` (PB2).
* **`init_ADC()` :** Configuration de l'ADC avec prescaler de 128 ($F_{ADC} = 125\text{ kHz}$).
* **`drive_motor()` :** Gestion sécurisée de la consigne et basculement des bras du pont en H.
* **`loop()` :** Boucle de contrôle cadence à 20 ms (50 Hz).

---

## 🛠️ Matériel Requis & Schéma de Câblage

| Composant | Broche ATmega328P / Arduino | Rôle |
| :--- | :--- | :--- |
| **PWM Bras A** | PB1 (Broche 9) | Signal PWM Marche Avant |
| **PWM Bras B** | PB2 (Broche 10) | Signal PWM Marche Arrière |
| **Bouton 1** | PD2 (Broche 2) | Commutation Marche Avant / Arrêt |
| **Bouton 2** | PD3 (Broche 3) | Commutation Marche Arrière / Arrêt |
| **LED Direction 1** | PD4 (Broche 4) | Témoin Marche Avant (Verte) |
| **LED Direction 2** | PD5 (Broche 5) | Témoin Marche Arrière (Bleue) |
| **Potentiomètre** | PC0 / A0 | Consigne de vitesse (0 à 100 %) |
| **LCD I2C** | SDA (A4) / SCL (A5) | Interface de supervision (Adresse `0x27`) |

---

## 🚀 Simulation & Test

Projet testé sous **Proteus VSM** avec co-simulation du pont en H et réponse dynamique du moteur CC sous charge.
