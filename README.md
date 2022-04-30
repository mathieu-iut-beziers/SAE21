# SAE21

Repository pour la SAE21

-------

## Premier jours

Repartition des taches :

* DHCP / WEB-Intranet / ACL -> VALENTIN
* DMZ / shemat -> reseaux MATHIEU
* DNS / WEB -> LUIGI

-------

## Journal de bord

24 / 03 -> TD presentation

25 / 03 -> decouverte du sujet

28 / 03 -> debut de travaille sur nos partie respective

29 / 03 -> creation des git (pour moi creation du schéma)

04 / 04 -> suite du travaille

11 / 04 -> TP -> ralisation du GNS3 des serv par Valentin et shemet fini en .doi par Mathieu

14 / 04 -> travaille continuer

15 / 04 -> decouverte et création des deocerfile

![img_reseau](reseaux_v2.png)

-------

### notes

dot1q -> trunk

Deux type de nat

source nat (snat)

    trame traduite par le routeur au bieeaux de l'ip src
    le serv parle au routeur 
    il suit les numereau de cinexion
    on ne le voi pas au niveaux ip

Dnat :

    on vas dnat un port
    ex tout ce quie est connecter au port 80 vas aller jusqu'au serv web

deux dns un recurcif et un publique

metre sur lme dns pour le WEB-intra l'ip priver

DMZ selon r/mikrotik

On refue toute les conection de l'exterieur sur le routeur
on vas autoriser les connexion au port 80 qui vas etre rediriger ver le serv web
on vas autoriser les conexion au pot 56 qui vas etre rediriger vers le serv DNS
autorize tout ce qui est establich
et on filtre les new
de mmeme pour les eserv pour les update
il faut aussi que je full nat le mikrotik

NAT :

cree un liste pour chaque sous interface et on leur met le nat pour chacune d'entrelle


    [admin@MikroTik] /ip firewall> export 
    # apr/30/2022 21:20:43 by RouterOS 6.49.6
    # software id = 944W-E5JF
    #
    # model = RB750Gr3
    # serial number = CC210ED20FBE
    /ip firewall filter
    add action=accept chain=input connection-state=established,related
    add action=accept chain=input protocol=icmp
    add action=drop chain=input in-interface-list=!LAN
    add action=accept chain=forward connection-state=established,related,new dst-port=80 \
        in-interface=ether1 protocol=tcp
    add action=accept chain=forward connection-state=established,related,new dst-port=443 \
        in-interface=ether1 protocol=tcp
    add action=accept chain=forward connection-state=established,related,new dst-port=53 \
        in-interface=ether1 protocol=udp
    add action=accept chain=forward connection-state=established,related
    add action=drop chain=forward connection-state=invalid
    add action=drop chain=forward connection-nat-state=!dstnat connection-state=new \
        in-interface-list=WAN
    add action=accept chain=forward connection-state=established,related,new in-bridge-port-list=\
        LAN
    /ip firewall nat
    add action=masquerade chain=srcnat comment="defconf: masquerade" ipsec-policy=out,none \
        out-interface-list=WAN
    add action=dst-nat chain=dstnat dst-port=80 in-interface=ether1 protocol=tcp src-port=\
        !32768-61000 to-addresses=192.168.40.80 to-ports=80
    add action=dst-nat chain=dstnat dst-port=443 in-interface=ether1 protocol=tcp src-port=\
        !32768-61000 to-addresses=192.168.40.80 to-ports=443
    add action=dst-nat chain=dstnat dst-port=53 in-interface=ether1 protocol=udp src-port=\
        !54-61000 to-addresses=192.168.40.53 to-ports=53
