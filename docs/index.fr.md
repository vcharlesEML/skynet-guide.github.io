# Introduction à Skynet
Avant de lire cet article, nous vous conseillons de lire l'article [Introduction to Sia](/sia/introduction/index.html) et l'article sur l'aspect technique de la location pour mieux comprendre comment tout fonctionne et qui est à l'origine du projet. Si les idées générales peuvent être saisies assez facilement, la conception de Sia peut être assez complexe, alors gardez cela à l'esprit.

>Si vous voulez savoir ce qu'est Skynet, ce qu'il peut faire aujourd'hui et ce qu'il peut réaliser à l'avenir, de la bouche même des auteurs, ne manquez pas [A Deep Dive into Skynet](https://blog.sia.tech/a-deep-dive-into-skynet-a0fa037feea) sur le blog de Sia (publié en février 2021).

## C'est quoi Skynet?
Skynet est une solution de premier niveau pour un espace de fichiers décentralisé qui a été construite sur le réseau Sia. En gros, il permet le partage de fichiers.

Souvent, les gens demandent si Sia est mort parce que Skynet a repris les priorités des développeurs pour le moment (bien que le développement de base sera bientôt repris par la Fondation Sia, pour en savoir plus [ici](/sia/foundation/index.html)). Il s'agit d'une idée fausse. Contrairement à d'autres projets comme IPFS et Filecoin qui sont développés par les mêmes personnes mais dont les codes sont complètement différents, Sia doit exister pour que Skynet fonctionne. Après tout, un nœud de location Skynet est juste un nœud de location Sia avec beaucoup de contrats. Ainsi, Sia lui-même n'arrêtera pas, et n'a pas arrêté, la production depuis la création de Skynet.

> Vous trouverez le site d'assistance et les guides officiels de Siasky.net [ici](https://support.siasky.net).

### Comment Skynet fonctionne-t-il en théorie ?
Je vais entrer dans un domaine un peu plus technique ici, alors assurez-vous de lire l'article sur la location avant.

Donc, la base du fonctionnement de Skynet est que chaque noeud qui veut avoir accès aux `Skyfiles` génère des contrats avec un maximum d'hôtes possible sur le réseau. Ensuite, quand un noeud veut télécharger un fichier, il envoie les données à un sous-ensemble de cet énorme pool d'hôtes et génère ce qu'on appelle un `Skylink`.

>Un `Skylink` est une chaîne de 46 caractères de données encodées en base64. Ces données représentent plusieurs choses, mais la plupart du temps, ce lien stocke la racine Merkle du fichier. Les racines Merkle sont compliquées et vous pouvez en savoir plus dans ce document écrit par le directeur de la Fondation Sia, Luke Champine. Mais en gros, les `Skylinks` sont juste des identifiants uniques de fichiers ; chaque combinaison unique de données (pensez au fichier vidéo) et de métadonnées (pensez à la taille du fichier) a une `Racine Merkle` unique. Pour en savoir plus sur les Skyfiles, voir [ici(EN)](/skynet/concepts/index.html).

Ensuite, lorsque vous voulez interroger un `Skylink` spécifique (pensez que vous demandez une vidéo sur Skyfeed), votre noeud Skynet fait un tas de demandes aux hôtes avec lesquels il a passé un contrat pour voir s'ils ont le `Skylink` en question. Le premier hôte à répondre avec le morceau de données envoie alors le fichier au noeud loueur, et est payé. En pratique, la latence de récupération peut être incroyablement faible, rivalisant avec celle de l'Internet centralisé.

## Comment Skynet fonctionne-t-il en pratique ?
Malheureusement, à ce jour, il n'est pas possible d'exécuter un portail Skynet dans votre navigateur (bien que la [Fondation Sia](/sia/foundation/index.html) y travaille avec le soutien de [Utreexo](https://forum.sia.tech/t/core-development-utreexo/54/14)). En raison des nombreuses limitations de l'exécution d'un nœud Sia complet, évoquées dans l'article [Before You Start](/renting/before-you-start/index.html), l'exécution d'un nœud de loueur est difficile pour votre système. Les parties principales dures sont :

- Le consensus est de 22GB (pas vraiment quelque chose que vous pouvez faire dans votre navigateur)
- Cela nécessite un temps de fonctionnement élevé pour maintenir les fichiers
- Cela nécessite beaucoup de RAM
- Cela génère une charge élevée du processeur (qui tue l'autonomie de la batterie)

Comment l'équipe a-t-elle contourné ce problème ? L'idée est d'utiliser ce que l'on appelle un **Portail Skynet**.

## Portails Skynet
Un portail Skynet est un concept simple dans la pratique. Il s'agit en fait d'un point d'entrée pour utiliser le réseau Sia sans avoir à gérer soi-même un nœud. Quelqu'un d'autre fait tourner un serveur dédié qui fait tourner un nœud Sia avec Skynet activé, et il y a une interface qui permet aux utilisateurs d'interagir avec lui. Par exemple, consultez [siasky](https://siasky.net), le portail officiel de Skynet Lab. Vous pouvez trouver d'autres portails dans notre liste de [Portails Skynet](/skynet/portals/index.html).

Mais alors qu'est-ce qui différencie Skynet Lab des autres plateformes centralisées de partage de fichiers comme Dropbox ou Mediafire ? Le principal élément de différenciation est que, puisque les portails utilisent le réseau Sia comme backend au lieu de leurs propres serveurs, chaque fichier est disponible sur chaque portail. Par exemple, Big Buck Bunny (un ancien fichier vidéo open source de Blender) est entièrement disponible sur [siasky.net](https://siasky.net/CACqf4NlIMlA0CCCieYGjpViPGyfyJ4v1x3bmuCKZX8FKA), [skyportal.xyz](https://skyportal.xyz/CACqf4NlIMlA0CCCieYGjpViPGyfyJ4v1x3bmuCKZX8FKA) et sur tous les autres portails Skynet à ce lien Sia : sia://CACqf4NlIMlA0CCCieYGjpViPGyfyJ4v1x3bmuCKZX8FKA. Mais ce ne sont pas des doublons du fichier. Vous accédez toujours au même fichier, mais par des portails différents. C'est le niveau supérieur à "fédéré", c'est décentralisé !

>Remarque : les gens prétendent souvent que Skynet n'est pas décentralisé parce que les points d'entrée et de sortie sont centralisés. C'est un point juste. Mais actuellement, il n'est pas possible de créer quelque chose avec un niveau de décentralisation plus élevé. On y travaille, comme indiqué ci-dessus, mais l'interopérabilité des sites est assez impressionnante en soi. N'importe quel fichier est accessible depuis n'importe quel portail, peu importe qui l'épingle et où il se trouve. Maintenant, chaque portail peut respectivement bloquer n'importe quel fichier qu'il souhaite (par exemple, Skynet Labs ne paiera pas pour que vous accédiez à un contenu illégal dans sa juridiction), mais comme tout le monde peut créer son propre portail, ce n'est pas un gros problème. Il n'est pas facile de créer son propre portail Skynet, mais c'est faisable si vous avez des compétences techniques. Pour un guide sur la façon de procéder, consultez [ici] (https://github.com/NebulousLabs/skynet-webportal/tree/master/setup-scripts).

## Qu'en est-il du registre ?
Je n'en ai pas parlé jusqu'à présent car je ne voulais pas submerger le lecteur avec de nouveaux éléments. Mais le registre, par essence, est un pointeur de fichier mutable construit sur le réseau Sia. D'accord, mais qu'est-ce que cela signifie ? Chaque entrée du registre lui-même ne fait que 256B sur le stockage de l'hôte (généralement sur le SSD principal) et il contient plusieurs choses, mais la seule sur laquelle je vais me concentrer est le champ de données. Dans ce champ de données, vous pouvez stocker jusqu'à 128B de n'importe quelles données que vous voudriez (bien que cela soit généralement utilisé pour stocker un seul `Skylink`).

Le registre fonctionne en générant effectivement une paire de clés privée-publique et en publiant la clé publique pour que tout le monde puisse y accéder. Ensuite, à l'intérieur de cette entrée se trouve le champ de données qui peut être modifié à tout moment. Mais comme le créateur original est le seul à pouvoir signer les données avec la clé privée, toute personne accédant à l'entrée sait que cette version a été générée par l'utilisateur original avec la clé privée, les entrées publiques ne sont pas une chose pour le moment.

>Maintenant, techniquement, vous pouvez stocker n'importe quelle donnée que vous voulez dans ce champ "data", donc vous vous demandez pourquoi les gens ne stockent pas les données directement dans un registre au lieu d'un simple pointeur vers un `Skyfile` (comme une photo par exemple). Et bien la raison principale est le coût. Afin de télécharger ce fichier de 256B, vous payez 50 hôtes pour 64KB hors contrat. Donc au total, pour ce champ de 128B, vous payez 3,2MB d'espace ! Comme il s'agit d'un espace hors contrat, vous n'avez pas à vous préoccuper de la taille minimale des fichiers, mais vous devez payer pour une surcharge de 25600x, ce qui n'est pas très pratique. De plus, une autre chose amusante est que vous payez une fois pour former une entrée de registre pour 10 ans à l'avance et ne payez pas la maintenance pour l'entrée de registre, donc une fois qu'elle est en place, elle restera en place à perpétuité (bien qu'elle se dégrade lentement en redondance au fil du temps parce que les hôtes n'ont pas à mettre en place une garantie pour les entrées de registre). La mise à jour de l'entrée avec de nouvelles informations rafraîchira le nombre d'hôtes.

>Remarque : le registre est une base de données, il s'appelle [SkyDB](/skynet/concepts/index.html) après tout. Mais il ne s'agit pas d'une base de données relationnelle comme SQL, mais d'une architecture de type key-value store. Cela peut être quelque peu limitatif pour certaines applications.

Pour en savoir plus sur le registre, voir [ici](/skynet/concepts/index.html).

## Que puis-je faire avec Skynet ?
Comme indiqué ci-dessus, Skynet possède actuellement deux fonctionnalités réelles, à savoir les `Skyfiles`, qui sont accessibles depuis n'importe quel portail, et le registre (qui, lorsqu'il est accessible via le SDK, est appelé `SkyDB`), qui sont des pointeurs mutables auxquels on peut accéder ou qui peuvent être mis à jour depuis n'importe quel portail.

Actuellement, les choses les plus avancées sur Skynet sont Skyfeed et la distribution de contenu en utilisant Skynet comme un CDN sur des sites comme [DTube](https://dtube.video).

#### [Skyfeed](https://skyfeed.hns.siasky.net)
Skyfeed est un réseau social de type Facebook construit au-dessus de Skynet par un membre de la communauté, Redsolver. Ses principales caractéristiques sont que si vous gérez votre propre portail, personne ne peut limiter votre discours et qu'il n'y a pas de publicité ni de trackage.

#### Capacités du CDN
Skynet a d'abord été présenté comme un CDN avec des fonctionnalités décentralisées, et c'est toujours le cas. Cependant, en raison de l'architecture web traditionnelle sur laquelle sont construits les portails web, ils dépendent entièrement de l'équilibrage de charge horizontal pour évoluer. De plus, en raison de la nature de Sia, les vitesses sont limitées à environ 1GBPS par nœud, ce qui nécessite une quantité énorme de nœuds et d'équilibrage de charge pour accomplir quelque chose comme un grand événement en direct. Par exemple, si vous souhaitez disposer d'une plateforme de streaming avec 100 000 spectateurs simultanés, vous aurez besoin de 1 000 à 2 000 nœuds au minimum (en fonction de la qualité du streaming). Cela sera corrigé une fois que Utreexo sera implémenté (ou hôte→utilisateur websockets), mais pour l'instant, c'est beaucoup de travail comme tout autre CDN. Bien que, contrairement aux architectures web traditionnelles, le pool de données lui-même peut être massif par nœud de portail parce qu'ils ont accès à chaque entrée `Skyfile` et `SkyDB`.

>Découvrez plus [Skynet Apps](/discover/skynet-apps/index.html).

## Prochaines étapes
Skynet fait toujours l'objet d'un développement intensif de la part de Skynet Labs et un grand nombre de fonctionnalités sont en préparation. Par exemple, les fonctionnalités à venir comprennent :

- Utreexo
- Amélioration du pipeline `SkyDB` avec des sockets web.
- Monétisation du contenu
- Hôte direct → sockets web utilisateur
- Améliorations massives des performances par nœud
- Amélioration de la scalabilité des nœuds
- Et bien plus encore

Cet article est pratiquement terminé, pour en savoir plus, consultez [building on skynet](/skynet/building-on-skynet/index.html).

*Écrit par : Covalent, Dernière modification : 14 avril 2021 - Traduit de l'anglais par Vincent CHARLES le 19 avril 2021*
