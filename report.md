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

# Reconnaissance des chiffres par réseau neuronal convolutionnel

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









# Questions générales







# Conclusion



