## Presentation dialogue

#### Titre

Bonjour, je m'appelle Andres Quintela et je suis un etudiant du Imperial College London en Erasmus. Pendant cette anne je suis en train de faire mon projet recherche avec Emmanuel Rachelson dans le group de Recherche SuReLi (SUpaero Reinforcement Learning). Aujourd'hui je vais vous parler des idees qu'on propose et de la biblio que j'ai fait pour pouvoir encandrer nos idées dans l'état de l'art et pour prende de l'inspiration.
#### Index
Je vais commencer avec une petite introduction du aprentissage par renforcement. Puis je vais vous expliquer la motivation derriere le multitask and transfer. Apres je vais vous parler de ce que j'ai trouve dans la biblio de multitask and transfer et ensuite je vais vous montres nos idees et ce qu'on propose. Finalement je vais vous montrer aussi ce que je ai trouvé dans la biblio du apprentissage continu par ça relevance avec ce qu'on propose.

#### Reinforcement Learning

L'apprentissage par reinforcement est une branche du apprentissage automatique qui cherche entrainer un agent dans un environment. Cette problematique peut etre modelisé comme un Markov decision process. Cet agent, qui est dans un état, est capable d'observer l'environement et prendre une action. Apres chaque action l'agent recoive une recompense scalaire et arrive dans un nouveau état. Son objectif est de maximiser le reward qu'il recoive tout au long d'un episode. La politique de l'agent governe les actions qu'il prenne.
Pendant l'apprentissage, l'agent modifie la politique pour atteindre cette objectif. Pour obtenir une politque optimale on a besoin de decider quelle action on doit prendre en estimant le reward qu'on peut obtenir. Pour ça on utilise le state-action value function, ou q-value function, qui est defini comme la recompense esperé tout au long d'un épisode.

#### Deep RL

Un algorithm de RL doit estimer le q-valeur de chaque paire etat-action pour obtenir la politique optimale, ce qui est fait avec l'exploration de l'environment. Quand l'espace des états est trop grand il est impossible d'explorer toutes les differentes trajectoires possibles. Au lieu, un réseau neuronal peut etre utilisé pour approximer la q-function.
Cette amelioration a permis de resoudre problèmes beaucoup plus compliqués comme, par exemple, des jeux.

#### Motivation
Le RL a evolue beaucoup avec l'utilisation des resaux neurones pour approximer la q-function. Le premier milestone a été l'algo DQN par DeepMind qui utilisait un reseau convolutionel avec un input des pixels pour estimer la q-fonction. Après il y a eu beacoup d'ameliorations qui ont tous etre comprises dans le algo Rainbow qui a atteindre une performance superhumaine et qui represente etat d'art dans les jeux atari.
Le probleme de ces algos c'est qu'ils ont besoin de beaucoup des episodes pour s'entrainer et qu'ils doivent être entrenés separement pour chaque tâche.
Le multitask RL essai de entreiner un agent qui peux resoudre differents taches sans avoir besoin d'un entrainement specifique. Aussi pour diminuer le temps d'entrainement le RL de transfer propose d'utiliser le connaisance des taches precedentes pour accélérer l'apprentissage d'une nouvelle tache.

#### Biblio Multi-Transfer

##### Slide 1

Maintenant je vais parler un peux du la biblio du domaine multitask et transfer dans le RL.
Une milestone importante a été atteindre par le concepte du policy distillation. Cette idée propose de reduire la taille et la complexité de la politique pour pouvoir combiner differents poliques plus facilement. Ca est fait en entreinant un resau pour qui'il donne les memes q-valeurs que les experts. Cet algo a ete teste avec une architecture d'un resau commun qui partage tous les parametres sauf l'output layer. Ils ont pas obtenu une performace egale aux experts dans des jeux Atari au cause de la variance des intervalles de q-values et rewards.


##### Slide 2

Après Deepmind a propose une autre algo qui prends l'idee de policy distillation et ajoute une regularisation. Cette regularization impose que l'apprentissage de chaque expert reste proche au agent commun, donc sa permettre de transferer de connaissance entre taches. Dans cet algo l'apprentissage des experts a lieau au meme temps que l'apprentisage du agent commun donc il est plus efficient.

##### Slide 3

En parallel au policy distillation des chercheurs de l'univeriste de toronto ont developpe un algo appelle actor mimic. L'idee est tres similaire. Avoir un ressau acteur qui imite les experts. Pour faire ca, le resseau neuron est entraine avec deux objectifs, le premier essais d'imiter la distribution des q-valeurs avec un softmax. Le softmax est utilise pour diminuer les effects des differnts echelles des q-valeurs. Le deuxieme objectif essay d'imiter la couche avant l'output de chaque expert. Avec ces deux objectifs cet algo peuvait juer a 10 jeux atari au meme temps, en obtenant une meilleure score que les experts en 3 jeux.

##### Slide 4

Deux singaporiens on ameliore le concept de policy distillation avec une nouvelle architecture qui preserve des couches independants pour chaque tache et combine les dernieres couches. Ils ont vu que ce type de architcture previen le transfer negative entre taches, dans ce cas les taches etaient les differents jeux d'Atari , parce que l'input visuel est vachement diffenrent pour chaque jeux.
Aussi ils on propose aussi une nouvelle concept pour le sampling qui essaie de preserver la distribution des etats initial.

Ils ont attendre des resultats meilleurs que les autres algorithms mais encore je trouve que la taille des resau est trop grand.

##### resultats

#### Our algorithm

Apres reviser la literature on a trouve que les algoritmes existant n'exploitent pas sufissament le transfer et qui'ils ont besoin des experts, ce qui veut dire des temps d'apprentissage tres longs. Aussi on a trouve que ces algoritmes, les plus performants ont besoin des tailles encore tres grands.

One propose de pas trouver un politique qui peut resoudre plusiers taches au meme temps mais de trouver une qui peut etre fine-tune tres rapidement pour etre executé dans une tache specifique. On croit qu'en entroduisant le finetuning on pourra diminuer la taille du reasau et favorer le transfer puisque le ressau pourra generaliser du conaissance plus facilement qu'il pourra utiliser pour apprendre des nouvelles taches.

####Continual learning problem

#####

On n'a pas trouve beaucoup des references au catastrophic forgetting dans la literature du RL et on croit que c'est un probleme tres important. Le catastrophic forgetting est la tendence des resaux neurones de perturber le conaissance d'une tache quand une nouvelle tache est apprisse.
Donc, comme on essay de entrenaire un resau dans des taches differentes. qui veut dire que la q-fonction pour chaque tache va etre differente, ont doit assurer que les taches precedentes sont pas alteres quand on apprende une nouvelle.



###### literature

La solution plus classique est de melanger des transitions des taches precendents quand le reseau s'entraine dans une nouvelle tache, mais pour ça on a besoin de avoir une memoire pour chaque tache, ce qui est une grande limitation.
Psedurehearsal propose de generer des transitions en faire pasant un vecteur random par le reseau. Avec cette concepte on a besoin de garder que les parametres du reseau pour generer des samples.

Deepmind a recement developpe aussi un systeme appelle Elastic Weight Consolidation qui freine le apprentissage de certaines parametres en fonction de ca importance dans le tache precedente. Dans une facon, on peut voir comme lier les parametres pour notre nouvelle tache aux parametres de la tache precendente avec un ressort dont ca rigidite depend de ca relevance dans la tache d'avant.

Une autre solution sont les systemes duals qui utilisent une reseau pour l'apprentisage et une autre differente pour consolider la memoire. Avec ce systeme les nouveaux connaisances peuvent etre memorisees et asimilees plus lentement.


#### Conclusion

Finalement, aborder le probleme du catastrophic forgetting directement va nous donner une nouvelle perspective dans cette problematique. Selon nous, les concepts de pseudorehearsal et dual-memory systmes vont etre tres aidants pour le developpement de notre algorithme. Pour conclure, grace a cette etude bibliographique on a pu posicioner notre propsoition dans le paysage du multitask and transfer RL et nous a donné de l'inspiration pour pouvoir proposer des nouvelles solutions dans ce domaine.
