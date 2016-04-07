SNES to CDi
-----------

SNEStoCDi est un projet basée sur un la plateforme Arduino, qui permet d'utiliser une manette de jeu Nintendo SNES (Super NES) / SFC (Super Famicom) sur un lecteur / console de jeu Philips CDi. SNEStoCDi ne nécessite pas plus d'électronique que l'Arduino lui-même. Il s'agit d'un projet facile à réaliser pour quiconque ayant quelques connaissances basiques à propos des Arduino et de la soudure électronique.

SNEStoCDi est distribué sous la licence Creative Common CC-BY 4.0 licence : http://creativecommons.org/licenses/by/4.0/legalcode


Comment faire votre propre adaptateur SNEStoCDi
-----------------------------------------------

Tout d'abord, vous allez avoir besoin de l'IDE Arduino. Si nécessaire, vous pouvez la télécharger gratuitement ici : https://www.arduino.cc/en/Main/Software

Vous aurez aussi besoin d'un Arduino, bien sûr. J'utilise des Arduinos UNO et Nano mais SNEStoCDi devrait fonctionnent avec la plupart des versions 5v de l'Arduino.

Pour fabriquer un adaptateur bon et durable, vous aurez vesoin d'un connecteur de manette SNES/SFC (une façon simple d'en obtenir est de recycler un cable d'extension) et une prise mâle mini-DIN 8 broches.

Réalisez le cablage :
- connectez la prise SNES/SFC à votre Arduino : reportez-vous au fichier wiring.jpg pour plus d'informations.
- connectez la prise CDi mini-DIN à votre Arduino : reportez-vous au fichier wiring.jpg pour plus d'informations.

IMPORTANT :
- la ligne RTS du CDi *doit* être connectée à une entrée analogique. si vous êtes curieux à ce sujet, reportez-vous à la section Trucs Techniques.
- l'Arduino sera alimenté par la VCC fournie par le CDi. ne connectez aucune autre source d'alimentation à l'Arduino !

Uploadez le logiciel SNEStoCDi sur votre Arduino :
- connectez l'Arduino à votre ordinateur.
- ajoutez la librairie SNESpad (par Rob Duart) à l'IDE Arduino. la librairie fournie avec SNEStoCDi est une version modifiée de SNESpad, optimisée pour les performances et pour une meilleure compatibilité avec la version actuelle de l'IDE Arduino. pour plus d'informations sur l'installation des librairies, consultez : http://arduino.cc/en/Guide/Libraries
- ouvrez le sketch SNEStoCDi.
- vérifier que les broches (pins) de l'Arduino paramétrées dans le sketch correspondent à votre cablage, si non ajustez le sketch avec le bon brochage.
  le sketch correspond au brochage indiqué sur le fichier wiring.jpg.
- uploadez le sketch compilé sur votre Arduino.

Et voilà, vous avez déjà fini :) l'adaptateur SNEStoCDi est plug-and-play : branchez-le simplement à votre CDi, branchez votre manette SNES, démarrer votre CDi et allez souffrir sur quelques horribles jeux ... :)
Je serais heureux d'être informé de votre réalisation, n'hésitez pas à m'envoyer un email pour me faire savoir que vous avez fabriqué votre propre adaptateur SNEStoCDi.

SNEStoCDi a été testé avec succès sur des Arduinos UNO et Nano, sur un Philips CDi 470..


Mapping SNEStoCDi & fioritures
------------------------------

La correspondance des boutons est probablement exactement ce que vous imaginez :
- le Dpad agit comme un Dpad
- le bouton "Y" agit comme le bouton 1.
- le bouton "B" agit comme le bouton 2.
- les boutons "A" et "X" agissent tous deux comme le bouton 3, qui correspond en fait aux boutons 1 et 2 pressés simultanément.

La SNES dispose de beaucoup plus de boutons qu'une manette CDi, ce qui m'a donné l'opportunité d'ajouté quelques fioritures :
- la vitesse du curseur est réglable en 5 pas. la gachette "L" ralenti le curseur d'un pas, la gachette "R" augmente la vitesse d'un pas.
- le bouton "start" réinitialise la vitesse avec le réglage par défaut (pas N°3).
- le réglage de vitesse est sauvegardé dans l'EEPROM de l'Arduino, ainsi SNEStoCDi appliquera votre dernier réglage de vitesse la prochaine fois que vous l'utiliserez.
- le bouton "select" change le mapping des boutons et inverse "B" et "Y". un autre appui sur "select" change à nouveau le mapping.


Trucs techniques
----------------

Vous êtes peut-être surpris par l'utilisation d'une entrée analogique alors qu'il n'y a aucun signal analogique. La raison est que le CDi et l'Arduino ont des niveaux logiques différents : le CDi communique sur la ligne RTS à environ 2,4 volts alors que l'Arduino nécessite environ 3 volts sur ses entrées numériques. En lisant la ligne RTS sur une entrée analogique, j'ai pu définir manuellement un "niveau logique virtuel" plus bas. Cela permet d'utiliser l'Arduino seul, sans autre composants électroniques pour adapter les niveaux.

Toutes mes tentatives d'utiliser le UART matériel pour envoyer des données au CDi se sont soldées par des échecs. C'est la raison pour laquelle j'ai utilisé NewSoftSerial (appelé SoftwareSerial dans l'IDE Arduino) pour émuler un autre UART. Dans la mesure où il est utilisé uniquement pour envoyer des données, l'interruption RX n'est pas réellement utilisée, c'est pourquoi la broche RX n'est pas connectée. Le CDi n'envoie rien sur son TX, il communique uniquement sur la ligne RTS, c'est la raison pour laquelle la communication entre l'Arduino et le CDi est en quelque sorte asymétrique.


Remerciements
-------------

Merci beaucoup d'avoir lu tout cela, j'espère sincèrement que SNEStoCDi vous aidera à utiliser votre CDi sans dépenser des sommes indécentes dans une manette CDi originale. Si vous réalisez votre SNEStoCDi, merci de me le faire savoir par email, je serais très heureux de savoir que mon travail a été utile :)

Remerciements particuliers à Rob Duart qui a créé l'excellente librairie SNESpad, et à Paul Hackmann qui a fourni de la documentation très utile à propos des périphériques CDi et de leur protocole de communication.

bye !
Laurent Berta / terbac_@hotmail.com
