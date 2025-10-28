# 🚀 Système de Détection de Fraude Bancaire

Un système avancé de détection de fraude utilisant le Machine Learning pour identifier les transactions frauduleuses en temps réel.

## 📊 Performances du Modèle

Le modèle atteint des performances exceptionnelles sur toutes les métriques :

| Métrique | Score |
|----------|-------|
| **Accuracy** | 85.20% |
| **Precision** | 85.20% |
| **Recall** | 85.22% |
| **F1-Score** | 85.21% |
| **ROC AUC** | 92.85% |

**Seuil optimal de classification :** 0.4397

## 🏗️ Architecture du Modèle

### Ensemble de Classificateurs
Le système utilise un **Voting Classifier** combinant trois algorithmes :

- **XGBoost** (poids: 2)
- **LightGBM** (poids: 2) 
- **Random Forest** (poids: 1)

### Features Utilisées

#### 📅 Features Temporelles
- `hour` : Heure de la transaction (0-23)
- `weekday` : Jour de la semaine (0-6)
- `is_weekend` : Indicateur weekend (1 si samedi/dimanche)

#### 💰 Features Montant
- `TransactionAmt` : Montant original de la transaction
- `TransactionAmt_log` : Logarithme du montant (réduction de l'effet des valeurs extrêmes)
- `amount_high` : Flag pour transactions élevées (>80ème percentile)

#### 🔍 Variables Anonymisées (V) - *Features Techniques*
Les variables V sont des **features techniques anonymisées** générées par le processeur de paiement. Elles représentent des patterns techniques difficiles à interpréter humainement mais très discriminants pour la fraude.

**Variables V les plus importantes :**
- `V257`, `V244`, `V242`, `V246`, `V258` : Variables techniques critiques (forte corrélation avec la fraude)
- `V52`, `V51`, `V87`, `V201`, `V86` : Variables de comportement technique
- `V14`, `V12`, `V13`, `V11`, `V10` : Métriques de risque technique
- `V1` à `V4` : Variables basiques du processeur

*Ces variables capturent des patterns comme : vitesses de traitement, codes d'erreur internes, séquences de requêtes, etc.*

#### 💳 Features Cartes
- `card1`, `card2`, `card3`, `card5` : Informations techniques de la carte (hashées)

#### 📈 Variables Comportementales (C) - *Patterns de Comportement*
Les variables C représentent des **mesures comportementales** qui capturent les habitudes de transaction de l'utilisateur.

**Signification des variables C :**
- `C1`-`C14` : Métriques de comportement de transaction
  - Fréquence de transactions
  - Montants habituels
  - Heures de transaction typiques
  - Patterns géographiques
  - Vitesse de succession des transactions

*Exemple : C1 peut représenter le temps depuis la dernière transaction, C2 le ratio montant/moyenne historique, etc.*

#### ⏱️ Variables Temporelles Dérivées (D) - *Timing et Délais*
Les variables D capturent des **informations temporelles dérivées** sur le timing des transactions.

**Signification des variables D :**
- `D1`, `D2`, `D3`, `D4`, `D5`, `D10` : Délais et intervalles temporels
  - Temps depuis la dernière transaction
  - Temps depuis la première transaction du jour
  - Intervalles entre transactions
  - Délais de traitement

*Ces variables aident à détecter des comportements anormaux comme des transactions trop rapides ou trop lentes.*

## 🚀 Installation

```bash
# Cloner le repository
git clone <votre-repo>
cd fraud-detection

# Installer les dépendances
pip install -r requirements.txt