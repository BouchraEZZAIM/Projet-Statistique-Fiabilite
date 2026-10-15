# 🔧 Analyse de la Fiabilité d'un Parc de Serveurs de Calcul

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

Projet complet d'analyse statistique de la fiabilité d'un parc de serveurs de calcul, couvrant la modélisation théorique, l'estimation paramétrique, les intervalles de confiance, l'optimisation de maintenance, et l'application à des données réelles (NASA C-MAPSS).

---

## 📖 Table des matières

- [Aperçu](#-aperçu)
- [Structure du projet](#-structure-du-projet)
- [Contenu détaillé du notebook](#-contenu-détaillé-du-notebook)
- [Résultats clés](#-résultats-clés)
- [Installation](#-installation)
- [Utilisation](#-utilisation)
- [Technologies utilisées](#-technologies-utilisées)
- [Méthodologie](#-méthodologie)
- [Auteur](#-auteur)

---

## 🎯 Aperçu

Ce projet analyse la **fiabilité** et la **maintenance optimale** d'un parc de **1 000 serveurs de calcul** modélisés par une loi de **Weibull(β=2.5, η=5)** (durées en milliers d'heures).

Le projet couvre :

- 📊 **Modélisation théorique** de la fiabilité (R(t), f(t), λ(t))
- 📈 **Estimation paramétrique** par régression linéaire (papier de Weibull) et par **Maximum de Vraisemblance (MLE)**
- 🎯 **Intervalles de confiance** à 95 % (méthode delta théorique et bootstrap paramétrique)
- ⚙️ **Optimisation de l'architecture** série vs parallèle
- 💰 **Optimisation du seuil de remplacement préventif** (compromis coût / disponibilité)
- ⏱️ **Analyse de survie** avec l'estimateur de **Kaplan-Meier** (données censurées)
- 🤖 **Modélisation ML** sur données réelles **NASA C-MAPSS** (Random Forest, KMeans, LSTM)

---

## 📁 Structure du projet

```
Projet-Statistique-Fiabilite/
│
├── Projet_statistique.ipynb     # Notebook principal (analyse complète)
├── requirements.txt              # Dépendances Python
└── README.md                     # Ce fichier
```

---

## 📚 Contenu détaillé du notebook

### 1️⃣ Acquisition et Visualisation des Données
- Génération d'un échantillon Weibull (N = 1000)
- Statistiques descriptives (moyenne, écart-type, min/max)
- **Visualisation en 3 panneaux** :
  - Histogramme des durées de vie + densité théorique
  - Courbe de fiabilité R(t) (empirique vs théorique)
  - Taux de défaillance λ(t) (croissant car β > 1)

### 2️⃣ Analyse de Sensibilité — Régimes de β
- **Grille d'histogrammes** pour β ∈ {0.5, 0.8, 1.0, 1.2, 1.5, 2.0, 2.5, 3.0}
- Illustration des trois régimes :
  - **β < 1** : Taux de défaillance décroissant (mortalité infantile)
  - **β = 1** : Taux constant (loi exponentielle)
  - **β > 1** : Taux croissant (usure)

### 3️⃣ Régression Linéaire — Papier de Weibull
- Transformation : Y = ln(-ln(R̂(t))) vs X = ln(t)
- Estimation des paramètres par MCO
- **Résultats obtenus** : β̂ ≈ 2.514, η̂ ≈ 4.919 (R² = 0.994)

### 4️⃣ Intervalles de Confiance sur β̂ et η̂ (95 %)
- **Méthode théorique** : Loi de Student + méthode delta pour η̂
- **Méthode bootstrap paramétrique** (B = 2000 réplications)
- **Ellipse de confiance jointe** pour le couple (β, η)
- Comparaison des deux approches

### 5️⃣ Optimisation de l'Architecture — Série vs Parallèle
- Simulation de 10 000 paires (T₁, T₂) exponentielles
- Comparaison des distributions, taux de défaillance, espérances
- **Conclusion** : l'architecture parallèle augmente significativement la durée de vie

### 6️⃣ Durée de Survie et Optimisation de la Maintenance
- **Probabilité de survie conditionnelle** : P(T > t₀ + Δt | T > t₀)
- **Espérance de vie résiduelle** E[T_t₀]
- **Optimisation du seuil τ*** de remplacement préventif (Cp = 1, Cf = 10)
- Sensibilité au ratio Cf/Cp ∈ {3, 5, 10, 20, 50}

### 7️⃣ Disponibilité A(τ) et Comparaison Coût vs Disponibilité
- Courbe de disponibilité A(τ) avec TR = 4h, TRP = 0.5h
- **Optimum** de disponibilité par rapport au coût
- Compromis multi-critères

### 8️⃣ Traitement des Données Censurées — Kaplan-Meier
- Génération de données censurées (taux ≈ 74 %)
- **Estimateur de Kaplan-Meier** avec IC 95 % (méthode log-log)
- Comparaison avec la fiabilité théorique Weibull et l'estimateur naïf
- **Papier de Weibull sur les résultats KM**

### 9️⃣ Modélisation sur Données Réelles (NASA C-MAPSS)
- Chargement du dataset **FD002** (53 759 lignes d'entraînement, 259 moteurs)
- **Log-vraisemblance** de la loi de Weibull en 3D (surface + courbes de niveau)
- **MLE** : β ≈ 4.38, η ≈ 225.64 cycles
- **Clustering KMeans** (avec PCA)
- **Random Forest** pour la classification
- **LSTM** (réseau de neurones récurrents) pour la prédiction de RUL

---

## 📊 Résultats clés

| Indicateur | Valeur |
|-----------|--------|
| **β (shape)** estimé | ≈ 2.51 |
| **η (scale)** estimé | ≈ 4.92 milliers d'heures |
| **R²** du papier de Weibull | 0.994 |
| **Seuil τ* optimal** (Cf/Cp = 10) | ≈ 1 773 heures |
| **Économie vs maintenance corrective** | **≈ 58 %** |
| **Taux de censure** (KM) | ≈ 74 % |
| **MLE sur NASA C-MAPSS** | β ≈ 4.38, η ≈ 225.64 |

---

## ⚙️ Installation

### Prérequis
- Python 3.8 ou supérieur
- pip

### Étapes

```bash
# 1. Cloner le dépôt
git clone https://github.com/BouchraEZZAIM/Projet-Statistique-Fiabilite.git
cd Projet-Statistique-Fiabilite

# 2. (Optionnel) Créer un environnement virtuel
python -m venv venv
source venv/bin/activate      # Linux/Mac
venv\Scripts\activate         # Windows

# 3. Installer les dépendances
pip install -r requirements.txt
```

---

## 🚀 Utilisation

### Option 1 — En local (Jupyter)

```bash
jupyter notebook Projet_statistique.ipynb
```

### Option 2 — Google Colab (recommandé)

Cliquez sur le badge ci-dessous :

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/BouchraEZZAIM/Projet-Statistique-Fiabilite/blob/main/Projet_statistique.ipynb)

### Option 3 — Exécution rapide

Le notebook est **entièrement autonome** :
- Les données NASA C-MAPSS sont téléchargées automatiquement depuis un dépôt public.
- Les figures sont sauvegardées automatiquement en `.png`.

---

## 🛠️ Technologies utilisées

| Bibliothèque | Utilisation |
|--------------|-------------|
| **NumPy** | Calcul numérique, génération de données |
| **Pandas** | Manipulation de données |
| **Matplotlib** | Visualisations 2D et 3D |
| **SciPy** | Fonctions statistiques (Weibull, Student, χ²) |
| **scikit-learn** | StandardScaler, KMeans, PCA, Random Forest |
| **TensorFlow / Keras** | Modèle LSTM pour la prédiction de RUL |

---

## 🔬 Méthodologie

### Modélisation de la fiabilité

$$R(t) = \exp\left[-\left(\frac{t}{\eta}\right)^{\beta}\right]$$

$$f(t) = \frac{\beta}{\eta}\left(\frac{t}{\eta}\right)^{\beta-1} \exp\left[-\left(\frac{t}{\eta}\right)^{\beta}\right]$$

$$\lambda(t) = \frac{\beta}{\eta}\left(\frac{t}{\eta}\right)^{\beta-1}$$

### Estimation par MLE

Maximisation de la log-vraisemblance :
$$\ell(\beta, \eta) = n\ln\beta - n\beta\ln\eta + (\beta-1)\sum \ln t_i - \sum \left(\frac{t_i}{\eta}\right)^\beta$$

### Optimisation de maintenance

Coût par unité de temps :
$$C(\tau) = \frac{C_p R(\tau) + C_f (1 - R(\tau))}{\int_0^\tau R(u)\,du}$$

### Estimateur de Kaplan-Meier

$$\hat{R}(t) = \prod_{t_i \le t} \left(1 - \frac{d_i}{n_i}\right)$$

---

## 📸 Aperçu des résultats

Les figures suivantes sont générées par le notebook :

- `fiabilite_weibull.png` — Courbes R(t), f(t), λ(t)
- `grille_histogrammes_beta.png` — Sensibilité à β
- `papier_weibull.png` — Régression linéaire
- `IC_beta_eta.png` — Intervalles de confiance + ellipse
- `courbe_disponibilite.png` — Disponibilité A(τ)
- `comparaison_optimums.png` — Coût vs Disponibilité
- `km_vs_weibull.png` — Kaplan-Meier vs théorique
- `km_papier_weibull.png` — Papier de Weibull sur KM
- `figure_logvraisemblance_3d.png` — Surface 3D MLE
- `figure_logvraisemblance_contour.png` — Courbes de niveau MLE

---

## 📄 Licence

Ce projet est réalisé dans un cadre **académique**. Libre d'utilisation à des fins pédagogiques.

---

## 👤 Auteur

**Bouchra EZZAIM**

- 🐙 GitHub : [@BouchraEZZAIM](https://github.com/BouchraEZZAIM)
- 📧 Email : *(ajoutez votre email si vous le souhaitez)*

---

## 🙏 Remerciements

- **NASA Prognostics Center of Excellence** pour le dataset **C-MAPSS**
- Documentation **SciPy** pour les fonctions Weibull et Kaplan-Meier
- Communauté **scikit-learn** pour les outils de ML

---

⭐ **Si ce projet vous a été utile, n'oubliez pas de lui donner une étoile !** ⭐
