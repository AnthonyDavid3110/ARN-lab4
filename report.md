---
title:  |
  | \vspace{-4cm}\includegraphics[height=3cm]{./figures/logoHEIG.png}
  | \vspace{1cm} Travail pratique 4
  | \vspace{0.5cm}\large ARN

author: "Anthony David, Alejandro Stadlin"
date: \today
documentclass: report
lang: fr
geometry: "top=2cm, bottom=1.5cm, left=2cm, right=2cm"
header-includes:
    - \usepackage{listings}
    - \usepackage{float}
    - \usepackage{subfig}
    - \usepackage{color}
    - \usepackage{hyperref}
    - \usepackage{xcolor}
    - \usepackage{fvextra}
    - \fvset{breaklines}
    - \usepackage{fancyhdr}
    - \usepackage{graphicx}
    - \usepackage{lipsum}
    - \pagestyle{fancy}
    - \fancyhead{}
    - \fancyhead[L]{\includegraphics[height=1cm]{./figures/logoHEIG.png}}
    - \fancyhead[C]{Travail pratique 4}
    - \fancyhead[R]{Anthony David, Alejandro Stadlin\\\today}
    - \renewcommand{\headrulewidth}{0.4pt}
    - \fancypagestyle{plain}{\pagestyle{fancy}}
    - \lstset{
        language=C++,
        basicstyle=\small\ttfamily,
        keywordstyle=\color{blue},
        stringstyle=\color{red},
        commentstyle=\color{green},
        showstringspaces=false,
        numbers=left,
        numberstyle=\tiny,
        numbersep=5pt,
        breaklines=true,
        breakatwhitespace=true,
        xleftmargin=20pt,
        framextopmargin=2pt,
        framexbottommargin=2pt,
        }
output:
    pdf_document:
        latex_engine: xelatex
        pandoc_args: "--listings"
        toc: true
        toc_depth: 3
        number_sections: true
        highlight: pygments
---


\tableofcontents

\pagebreak

# Introduction
Pour le rapport suivant, nous détaillerons notre travail pratique où nous avons employé trois méthodes distinctes pour classer des images de chiffres issues de la base de données MNIST : un perceptron multicouche (MLP), un MLP utilisant l'Histogramme de Gradients Orientés (HOG), et un réseau de neurones convolutif (CNN). Ensuite, nous avons développé un autre CNN pour classifier des radiographies thoraciques, afin de distinguer les cas normaux de ceux présentant une pneumonie.

Ce projet a été réalisé en utilisant Keras, une bibliothèque de réseaux neuronaux de haut niveau écrite en Python et capable de fonctionner avec TensorFlow. Keras simplifie la création de modèles de machine learning complexes grâce à son interface intuitive et sa capacité à intégrer facilement avec TensorFlow, offrant ainsi un environnement robuste pour le prototypage rapide et l'exécution de réseaux de neurones profonds. Ce rapport présentera les résultats obtenus avec chacune des méthodologies appliquées, analysant leur efficacité et précision dans les tâches de classification proposées.


# Reconnaissance des chiffres à partir des données brutes

> Quel est l'algorithme d'apprentissage utilisé pour optimiser les poids des réseaux de neurones ? Quels sont les paramètres (arguments) utilisés par cet algorithme ? Quelle fonction de coût est utilisée ? Veuillez fournir l'équation (ou les équations).

L'algorithme utilisé pour optimiser les poids est RMSprop (Root Mean Square Propagation).


$$
E[g^2]t = \beta E[g^2]{t-1} + (1-\beta)\left(\frac{\partial C}{\partial w}\right)^2 \
w_t = w_{t-1} - \frac{\eta}{\sqrt{E[g^2]_t}}\frac{\partial C}{\partial w}
$$
où $\eta$ est le taux d'apprentissage, $w_t$ le nouveau poids, $\beta$  est le paramètre de moyenne mobile, $E[g]$ est la moyenne mobile des  gradients carrés et $\frac{\partial C}{\partial w}$ est la dérivée de la fonction de coût par rapport au poids

La fonction de coût est la fonction de perte par entropie croisée catégorique :
$$
\text{CE} = -\frac{1}{N} \sum_{k=0}^{N} \log \vec{p_i}[y_i]
$$
où $N$ est le nombre d'échantillons, $\vec{p_i}$ est la sortie du réseau de neurones et $y_i$ est l'index de la classe cible.

## Réseau neuronal superficiel

# Reconnaissance de chiffres à partir de données brutes
// todo

# Reconnaissance des chiffres à partir des caractéristiques des données d'entrée

## Réseau neuronal superficiel

Dans cette expérience, nous utilisons les caractéristiques du Histogramme des gradients (HOG) pour classer les chiffres.

### Hyper-paramètres

- **Nombre d'orientations** : 8
- **Pixels par cellule** : 4
- **Nombre de cellules par image** : $\frac{28}{4} \times \frac{28}{4} = 7 \times 7 = 49 \ cellules$
- **Caractéristiques HOG totales** :$49 \times 8 = 392$

### Configuration du réseau :
- **Couches cachées** : 9 neurones, puis 50 neurones, puis 100 neurones
- **Fonction d'activation de sortie** : softmax pour 10 classes (chiffres de 0 à 9)

### Calcul des poids :
1. **Première couche cachée** (9 neurones) :
   - **Poids** : $392 \times 9 + 9 (biais) = 3528 + 9 = 3537$

2. **Deuxième couche cachée** (50 neurones) :
   - **Poids** : $9 \times 50 + 50 (biais) = 450 + 50 = 500$

3. **Troisième couche cachée** (100 neurones) :
   - **Poids** : $50 \times 100 + 100 (biais) = 5000 + 100 = 5100$

4. **Couche de sortie** :
   - **Poids** : $100 \times 10 + 10 (biais) = 1000 + 10 = 1010$

### Poids totaux :
- **Total** : 3537 (première couche cachée) + 500 (deuxième couche cachée) + 5100 (troisième couche cachée) + 1010 (couche de sortie) = 10147

## Expérimentations
### 4 pixels, 8 orientations, 9 neurones
\begin{figure}[H]
  \centering
  \subfloat[Graphique d'erreur]{
    \includegraphics[scale=0.45]{./figures/HOG_pix4_ori8_9neurones_g.png}
    \label{fig:error-graph}
  }\quad % Adjusted for more consistent spacing
  \subfloat[Matrice de confusion]{
    \includegraphics[scale=0.45]{./figures/HOG_pix4_ori8_9neurones_m.png}
    \label{fig:confusion-matrix}
  }
  \caption{Modèle utilisant les caractéristiques HOG avec 4 pixels par cellule, 8 orientations et 9 neurones}
  \label{fig:hog-features}
\end{figure}

### 4 pixels, 8 orientations, 50 neurones
\begin{figure}[H]
  \centering
  \subfloat[Graphique d'erreur]{
    \includegraphics[scale=0.45]{./figures/HOG_pix4_ori8_50neurones_g.png}
    \label{fig:error-graph}
  }\quad % Adjusted for more consistent spacing
  \subfloat[Matrice de confusion]{
    \includegraphics[scale=0.45]{./figures/HOG_pix4_ori8_50neurones_m.png}
    \label{fig:confusion-matrix}
  }
  \caption{Modèle utilisant les caractéristiques HOG avec 4 pixels par cellule, 8 orientations et 50 neurones}
  \label{fig:hog-features}
\end{figure}

### 4 pixels, 8 orientations, 100 neurones
\begin{figure}[H]
  \centering
  \subfloat[Graphique d'erreur]{
    \includegraphics[scale=0.45]{./figures/HOG_pix4_ori8_100neurones_g.png}
    \label{fig:error-graph}
  }\quad % Adjusted for more consistent spacing
  \subfloat[Matrice de confusion]{
    \includegraphics[scale=0.45]{./figures/HOG_pix4_ori8_9neurones_m.png}
    \label{fig:confusion-matrix}
  }
  \caption{Modèle utilisant les caractéristiques HOG avec 4 pixels par cellule, 8 orientations et 100 neurones}
  \label{fig:hog-features}
\end{figure}

### 4 pixels, 4 orientations, 9 neurones
\begin{figure}[H]
  \centering
  \subfloat[Graphique d'erreur]{
    \includegraphics[scale=0.45]{./figures/HOG_pix4_ori4_9neurones_g.png}
    \label{fig:error-graph}
  }\quad % Adjusted for more consistent spacing
  \subfloat[Matrice de confusion]{
    \includegraphics[scale=0.45]{./figures/HOG_pix4_ori4_9neurones_m.png}
    \label{fig:confusion-matrix}
  }
  \caption{Modèle utilisant les caractéristiques HOG avec 4 pixels par cellule, 4 orientations et 9 neurones}
  \label{fig:hog-features}
\end{figure}

### 4 pixels, 4 orientations, 50 neurones
\begin{figure}[H]
  \centering
  \subfloat[Graphique d'erreur]{
    \includegraphics[scale=0.45]{./figures/HOG_pix4_ori4_50neurones_g.png}
    \label{fig:error-graph}
  }\quad % Adjusted for more consistent spacing
  \subfloat[Matrice de confusion]{
    \includegraphics[scale=0.45]{./figures/HOG_pix4_ori4_50neurones_m.png}
    \label{fig:confusion-matrix}
  }
  \caption{Modèle utilisant les caractéristiques HOG avec 4 pixels par cellule, 4 orientations et 50 neurones}
  \label{fig:hog-features}
\end{figure}

### 7 pixels, 8 orientations, 9 neurones
\begin{figure}[H]
  \centering
  \subfloat[Graphique d'erreur]{
    \includegraphics[scale=0.45]{./figures/HOG_pix7_ori8_9neurones_g.png}
    \label{fig:error-graph}
  }\quad % Adjusted for more consistent spacing
  \subfloat[Matrice de confusion]{
    \includegraphics[scale=0.45]{./figures/HOG_pix7_ori8_9neurones_m.png}
    \label{fig:confusion-matrix}
  }
  \caption{Modèle utilisant les caractéristiques HOG avec 7 pixels par cellule, 8 orientations et 9 neurones}
  \label{fig:hog-features}
\end{figure}

### 7 pixels, 4 orientations, 9 neurones
\begin{figure}[H]
  \centering
  \subfloat[Graphique d'erreur]{
    \includegraphics[scale=0.45]{./figures/HOG_pix7_ori4_9neurones_g.png}
    \label{fig:error-graph}
  }\quad % Adjusted for more consistent spacing
  \subfloat[Matrice de confusion]{
    \includegraphics[scale=0.45]{./figures/HOG_pix7_ori4_9neurones_m.png}
    \label{fig:confusion-matrix}
  }
  \caption{Modèle utilisant les caractéristiques HOG avec 4 pixels par cellule, 8 orientations et 9 neurones}
  \label{fig:hog-features}
\end{figure}

## Commentaires
Pour nous, les deux derniers systèmes sont les meilleurs. Par rapport aux autres, ils semblent offrir les meilleures performances et ne présentent pas de signes de surapprentissage ("overfitting"). Les modèles dotés d'un grand nombre de neurones, tels que ceux avec 50 et 100 neurones dans la couche cachée, montrent clairement des signes de surapprentissage. En outre, lorsque la taille des cellules est de 4x4 pixels, les résultats sont meilleurs avec 4 orientations. En revanche, pour les cellules de 7x7 pixels, une configuration à 8 orientations semble plus performante.

# Reconnaissance de chiffres par réseau neuronal convolutif

## Réseau neuronal convolutionnel profond

- Époques : 50
- Taille du lot : 128

- Couches cachées :
  - Convolution 2D : 5x5 
  - MaxPooling 2D : taille du pool : 2x2
  - Convolution 2D : 5x5
  - MaxPooling 2D : taille du pool : 2x2
  - Convolution 2D : 3x3
  - MaxPooling 2D : 2x2
  - Couche Flatten
  - Dense (25 neurones), fonction d'activation : ReLU

Sortie : 10 neurones, fonction d'activation : softmax

Complexité du modèle :
```
_________________________________________________________________
 Layer (type)                Output Shape              Param #   
=================================================================
 l0 (InputLayer)             [(None, 28, 28, 1)]       0         
                                                                 
 l1 (Conv2D)                 (None, 28, 28, 9)         234       
                                                                 
 l1_mp (MaxPooling2D)        (None, 14, 14, 9)         0         
                                                                 
 l2 (Conv2D)                 (None, 14, 14, 9)         2034      
                                                                 
 l2_mp (MaxPooling2D)        (None, 7, 7, 9)           0         
                                                                 
 l3 (Conv2D)                 (None, 7, 7, 16)          1312      
                                                                 
 l3_mp (MaxPooling2D)        (None, 3, 3, 16)          0         
                                                                 
 flat (Flatten)              (None, 144)               0         
                                                                 
 l4 (Dense)                  (None, 25)                3625      
                                                                 
 l5 (Dense)                  (None, 10)                260       
                                                                 
=================================================================
Total params: 7,465
Trainable params: 7,465
Non-trainable params: 0
_________________________________________________________________
```


## Expérimentation
### 25 neurones
\begin{figure}[H]
  \centering
  \subfloat[Graphique d'erreur]{
    \includegraphics[scale=0.45]{./figures/CNN_25neurones_g.png}
    \label{fig:error-graph}
  }\quad % Adjusted for more consistent spacing
  \subfloat[Matrice de confusion]{
    \includegraphics[scale=0.45]{./figures/CNN_25neurones_m.png}
    \label{fig:confusion-matrix}
  }
  \caption{Modèle utilisant 25 neurones CNN dans la partie feed forward}
  \label{fig:hog-features}
\end{figure}

### 200 neurones
\begin{figure}[H]
  \centering
  \subfloat[Graphique d'erreur]{
    \includegraphics[scale=0.45]{./figures/CNN_200neurones_g.png}
    \label{fig:error-graph}
  }\quad % Adjusted for more consistent spacing
  \subfloat[Matrice de confusion]{
    \includegraphics[scale=0.45]{./figures/CNN_200neurones_m.png}
    \label{fig:confusion-matrix}
  }
  \caption{Modèle utilisant 200 neurones CNN dans la partie feed forward}
  \label{fig:hog-features}
\end{figure}


### 500 neurones
\begin{figure}[H]
  \centering
  \subfloat[Graphique d'erreur]{
    \includegraphics[scale=0.45]{./figures/CNN_500neurones_g.png}
    \label{fig:error-graph}
  }\quad % Adjusted for more consistent spacing
  \subfloat[Matrice de confusion]{
    \includegraphics[scale=0.45]{./figures/CNN_500neurones_m.png}
    \label{fig:confusion-matrix}
  }
  \caption{Modèle utilisant 500 neurones CNN dans la partie feed forward}
  \label{fig:hog-features}
\end{figure}

## Commentaires
Encore une fois, le modèle avec le moins de neurones s'avère être le meilleur. Pas nécessairement en termes de performances telles qu'indiquées par la fonction de perte, qui est légèrement plus élevée que pour les autres modèles. Mais au moins, il ne surajuste pas, contrairement aux deux autres modèles restants.

Dans cet ensemble, la confusion est plus répandue. Nous ne pouvons pas observer un numéro spécifique qui échoue fréquemment dans tous les modèles.

# Radiographie pulmonaire pour détecter une pneumonie
## Réseau neuronal convolutionnel profond
- Nombre d'épques : 50

## Expérimentation
### Perte et précision
\begin{figure}[H]
  \centering
  \subfloat[valeurs de perte d'entraînement et de validation]{
    \includegraphics[scale=0.45]{./figures/CCN_pneumonia_model_loss_g.png}
    \label{fig:error-graph}
  }\quad % Adjusted for more consistent spacing
  \subfloat[valeurs de précision d'entraînement et de validation]{
    \includegraphics[scale=0.45]{./figures/CCN_pneumonia_model_accuracy_g.png}
    \label{fig:confusion-matrix}
  }
  \caption{Valeurs de perte et de précision d'entraînement et de validation}
  \label{fig:hog-features}
\end{figure}

### Matrices de confusion
\begin{figure}[H]
  \centering
  \subfloat[Set de validation]{
    \includegraphics[scale=0.45]{./figures/CCN_pneumonia_matrix_validation.png}
    \label{fig:error-graph}
  }\quad % Adjusted for more consistent spacing
  \subfloat[Set de test]{
    \includegraphics[scale=0.45]{./figures/CCN_pneumonia_matrix_test.png}
    \label{fig:confusion-matrix}
  }
  \caption{Matrices de confusion du modèle}
  \label{fig:hog-features}
\end{figure}

## Commentaires
Un des problèmes principaux rencontrés est lié à la taille restreinte de notre ensemble de données de validation. Cette limitation a probablement contribué à la difficulté du modèle à généraliser efficacement, ce qui s'est traduit par des performances mitigées lors de la phase de test. Une base de données plus grande et plus variée pourrait aider à améliorer la robustesse du modèle, en lui fournissant une meilleure représentation des variations possibles au sein de la population ciblée.

Par ailleurs, il est intéressant de noter que le modèle s'est montré particulièrement efficace pour identifier la présence de la maladie. Cependant, une grande partie des erreurs observées concerne des faux positifs, c'est-à-dire des individus sains classifiés à tort comme malades. Cette tendance du modèle à « errer du côté de la prudence » pourrait être vue comme un avantage dans des contextes cliniques où il est crucial de ne pas laisser passer de cas non diagnostiqués de maladies graves. En effet, dans une telle configuration, les coûts liés à un faux positif (par exemple, des tests de confirmation supplémentaires) sont généralement moins lourds que les conséquences d'un faux négatif, où une maladie non détectée pourrait s'aggraver.

Cela implique que si le modèle était utilisé en support dans un contexte clinique, il serait particulièrement fiable pour confirmer l'absence de la maladie, réduisant ainsi le risque de manquer des diagnostics chez des patients réellement malades. Les cas identifiés comme positifs par le modèle devraient cependant être suivis d'une confirmation diagnostique par un professionnel de santé pour vérifier l'exactitude de la prédiction.

En conclusion, bien que les résultats de notre modèle nécessitent une amélioration de la précision et de la généralisation à travers l'usage de données de validation plus conséquentes, ses capacités actuelles suggèrent qu'il pourrait servir efficacement comme outil de présélection, réduisant le risque de non-détection des maladies dans les premiers stades. Cela pourrait être particulièrement utile dans des environnements à ressources limitées où les options de dépistage sont restreintes.


# Questions générales

> **De la consigne : Les modèles CNN sont plus profonds (ils ont plus de couches), ont-ils plus de poids que les modèles peu profonds ? expliquer avec un exemple.**

Oui, il est communément admis que les réseaux de neurones convolutifs (CNN) plus profonds, ayant plus de couches, ont généralement une capacité plus grande en termes de nombre de paramètres (ou "poids") par rapport aux réseaux moins profonds. Cela est dû à l'augmentation du nombre de couches, chacune pouvant contenir des ensembles de filtres ou des neurones qui ajoutent des paramètres au modèle.

Cependant, ce n'est pas toujours le cas que plus de couches signifient automatiquement beaucoup plus de paramètres. En effet, l'architecture d'un réseau, y compris la taille des filtres et le pas de convolution, peut influencer de manière significative le nombre total de poids. Par exemple, un réseau peu profond mais avec une couche entièrement connectée très large peut parfois avoir plus de paramètres qu'un réseau plus profond avec des couches convolutives et des filtres plus petits ou plus de couches de pooling.

Pour illustrer, considérons deux exemples hypothétiques :

- **Modèle peu profond** : Ce modèle contient une seule couche entièrement connectée avec 1 000 neurones, chacun connecté à 1 000 entrées. Cela donne 1 000 x 1 000 = 1 000 000 de paramètres, sans compter les biais.
- **Modèle profond** : Ce modèle consiste en plusieurs couches convolutives, chacune avec des filtres de taille 3x3, utilisés sur plusieurs canaux, mais avec moins de connexions denses. Si une couche a 100 filtres sur un volume d'entrée de 10x10x10, cela donne 3x3x10x100 = 9 000 paramètres par couche. Même avec plusieurs de ces couches, le nombre total peut rester inférieur à celui du modèle peu profond, en raison de la nature moins connectée des couches convolutives.

En résumé, bien que les réseaux plus profonds aient souvent plus de paramètres en raison de leur complexité accrue et du nombre accru de couches, l'architecture spécifique et l'utilisation de techniques comme le pooling et des filtres de petite taille peuvent aboutir à un nombre total de paramètres qui n'est pas nécessairement proportionnel à la profondeur du réseau.



# Conclusion

En conclusion, notre étude démontre que l'utilisation de réseaux neuronaux convolutifs (CNN) plus profonds améliore significativement la précision de la classification des images par rapport aux méthodes conventionnelles. Ces résultats encouragent le développement continu de modèles de CNN avancés pour des applications en reconnaissance visuelle et autres domaines tels que l'analyse médicale d'images.

Nous recommandons de poursuivre la recherche sur l'optimisation des architectures de CNN et d'explorer l'intégration de l'apprentissage non supervisé pour améliorer l'efficacité des modèles. Malgré les progrès réalisés, les défis liés à la généralisation des modèles nécessitent des études futures axées sur l'amélioration de la robustesse et de l'adaptabilité des CNN à divers contextes d'application.

