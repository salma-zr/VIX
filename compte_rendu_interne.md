# Compte rendu interne (non à rendre)

## 1) Compréhension du problème

L'assignment demande :

1. Calibration d'un modèle SLV par **particle method** sur une surface vanille cible plate (15%).
2. Étude d'impact des paramètres \(\gamma\), \(\rho\), \(\kappa\) sur :
   - le smile du modèle SV pur (\(l \equiv 1\)),
   - la forme de la leverage calibrée \(l(t,S)\).
3. Pricing d'un payoff forward-start call spread, comparaison Black-Scholes vs SLV calibré.
4. Partie Bergomi 2 facteurs :
   - condition sur la matrice de corrélation,
   - moyenne/covariance du pas gaussien,
   - simulation SPX,
   - preuve martingale de \(\xi_t^u\),
   - formule VIX et pricing VIX futures/options via Gauss-Hermite,
   - sensibilités \(\omega\) et \(k_1\) (en 1 facteur).

Concepts de cours utilisés :
- projection locale (type Gyöngy) : \(\sigma_{\mathrm{Dup}}^2 = l^2 \mathbb E[a^2 \mid S]\),
- OU exact + Euler log-spot,
- régression noyau conditionnelle,
- conditions PSD corrélation (mineurs/déterminant),
- exponentielles gaussiennes martingales,
- quadrature de Gauss-Hermite multidimensionnelle.

Hypothèses :
- cadre risque-neutre \((\Omega,\mathcal F,(\mathcal F_t),\mathbb Q)\),
- conditions usuelles, intégrabilité des quantités manipulées,
- \(r=q=0\) dans l'implémentation.

## 2) Analyse du travail du binôme

### Correct

- Bonne architecture générale (théorie -> code -> résultats).
- Formule de leverage correctement utilisée.
- Schéma de simulation cohérent en partie SLV.
- Partie Bergomi globalement solide (corrélation, covariance, GH).
- Résultats numériques plausibles économiquement :
  - \(\gamma\uparrow\) : smile SV plus marquée,
  - \(\rho<0\) : skew gauche equity,
  - \(\omega\uparrow\) : IV VIX augmente fortement,
  - \(k_1\uparrow\) (1 facteur) : IV VIX diminue.

### À renforcer

- Hypothèses probabilistes pas toujours explicitées dès le départ.
- Ambiguïté potentielle sur la notation \(\kappa\) (réversion vs bande passante).
- Commentaires parfois trop qualitatifs sans métriques synthétiques.
- Erreurs/IC Monte Carlo pas homogènes sur toutes les sous-parties.

## 3) Solution corrigée (version améliorée)

- Clarification du cadre mathématique (mesure \(\mathbb Q\), filtration, intégrabilité).
- Désambiguïsation des notations :
  - \(\kappa_{\text{mr}}\) : mean reversion,
  - \(\kappa_{\text{bw}}\) : bandwidth kernel.
- Mise en forme plus "rendu" : réponses découpées par sous-question, enchaînement logique explicite.
- Lien renforcé théorie/intuition financière :
  - rôle de \(\mathbb E[a_t^2 \mid S_t=S]\),
  - pourquoi deux modèles calibrés vanilles peuvent diverger sur un forward-start.
- Formalisation propre de la partie VIX :
  - martingale de \(\xi_t^u\),
  - expression fermée \(\mathrm{VIX}_T=\psi(T,X_T^1,X_T^2)\),
  - pricing GH 2D.

## 4) Différences et justification des changements

1. **Ajout d'un cadre probabiliste explicite**  
   Justification : niveau de rigueur attendu en M2.

2. **Notations harmonisées**  
   Justification : éviter toute confusion de paramètres.

3. **Rédaction recentrée "copie finale"**  
   Justification : meilleure lisibilité pour correction.

4. **Résultats clés mieux mis en avant**  
   Justification : démontrer calibration et sensibilités avec des chiffres.

5. **Explications financières renforcées**  
   Justification : relier formalisme mathématique et usage en pricing.

## 5) Mention IA (discrète, factuelle)

Formulation proposée :

> "Nous avons utilisé ponctuellement un assistant d'IA pour relire certaines étapes algébriques (notamment des formules de covariance) et améliorer la clarté rédactionnelle. La modélisation, l'implémentation numérique et l'interprétation financière restent de notre fait."
