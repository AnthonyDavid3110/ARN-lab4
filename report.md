# ARN - Practical Work 04 - Deep Neural Networks

Auteurs :

- Anthony David : anthony.david@heig-vd.ch
- Alejandro Stadlin

Professeur : Andres Perez-Uribe

Assistants: Shabnam Ataee, Simon Walther

# Introduction







# Reconnaissance des chiffres à partir des données brutes

> Quel est l'algorithme d'apprentissage utilisé pour optimiser les poids des réseaux de neurones ? Quels sont les paramètres (arguments) utilisés par cet algorithme ? Quelle fonction de coût est utilisée ? Veuillez fournir l'équation (ou les équations).

L'algorithme utilisé pour optimiser les poids est RMSprop (Root Mean Square Propagation).




$$
\begin{align*}
E[g^2]t = \beta E[g^2]{t-1} + (1-\beta)\left(\frac{\partial C}{\partial w}\right)^2 \
w_t = w_{t-1} - \frac{\eta}{\sqrt{E[g^2]_t}}\frac{\partial C}{\partial w}
\end{align*}
$$
où $\eta$ est le taux d'apprentissage, $w_t$ le nouveau poids, $\beta$  est le paramètre de moyenne mobile, $E[g]$ est la moyenne mobile des  gradients carrés et $\frac{\partial C}{\partial w}$ est la dérivée de la fonction de coût par rapport au poids

La fonction de coût est la fonction de perte par entropie croisée catégorique :
$$
    \text{CE} = -\frac{1}{N} \sum_{k=0}^{N} \log \vec{p_i}[y_i]
$$
où $N$ est le nombre d'échantillons, $\vec{p_i}$ est la sortie du réseau de neurones et $y_i$ est l'index de la classe cible.

## Réseau neuronal superficiel

Pour cette expérience, un simple réseau neuronal superficiel est utilisé. Nous utilisons des données brutes pour classer les chiffres.

### Hyper-paramètres

Modifications apportées au modèle par rapport à l'original : réduction du nombre de neurones dans la couche cachée, passant de 300 à 100.

- Époques : 90
- Couches cachées :
  - 100 neurones, ReLU
- Fonction d'activation de sortie : softmax
- Taille du lot : 64
- Poids dans la couche cachée : 784 * 100 + 100 = 78400 + 100 = 78500
- Poids dans la couche de sortie : 10 * 100 + 10 = 1010
- Poids totaux : 78500 + 1010 = 79510

# Reconnaissance des chiffres à partir des caractéristiques des données d'entrée

## Réseau neuronal superficiel

Dans cette expérience, nous utilisons les caractéristiques du Histogramme des gradients (HOG) pour classer les chiffres.

### Hyper-paramètres

HOG :

- Nombre d'orientations : 8
- Pixels par cellule : 4

- Époques : 94
- Couches cachées :
  - 100 neurones, ReLU
- Fonction d'activation de sortie : softmax
- Taille du lot : 64

- Poids dans la couche cachée : 392 * 200 + 200 = 78400 + 200 = 78600
- Poids dans la couche de sortie : 10 * 200 + 10 = 2010
- Poids totaux : 78600 + 2010 = 80610

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

# Experiments







# 





# 

