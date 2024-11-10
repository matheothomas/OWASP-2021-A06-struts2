---
type: slide
slideOptions:
  transition: slide
---

# A06:2021-Composants vulnérables et obsolètes

----

<!-- .slide: data-background="https://cdn.openart.ai/stable_diffusion/857d0707f7439e00a89e2b716d5431bca7367e76_2000x2000.webp" -->

# Plan
1. Les risques
2. Attaque : démonstration
3. Mesures pour s'en prémenir

---

<!-- .slide: data-background="https://cdn.businessinsider.de/wp-content/uploads/2019/12/bill-gates-reuters-bild.png" -->

## 1. Les Risques


----

## De la CWE à gogo
Aucune CVE, que des Common Weakness Enumeration
- Plus de 30 000 cas documentés


----

## Les composants
Tout ce qui a été écrit par quelqu'un d'autre

- Bibliothèques
- frameworks
- APIs
- système d'exploitation
- gestionaires de bases de données


----

### Des chiffres qui datent (de 2014)
L'exemple des bibliothèques
- 30 millions (26%) de téléchargements concernait des librairies vulnérables
- En moyenne, 10 000 lignes de code correspond à 5 à 10 vulnérabilités


----

- Une application utilise beaucoup de libraires (>30) --> grande surface d'attaque
- Plusieurs mois après la découverte de Struts2 dans le parser Jakarta Multipart

    - 4 000 organisations ont téléchargé la librairie 180 000 fois
- Les bibliothèques possèdent souvent les même permissions que l'application qui l'utilise


----

### Du pain béni
On peut facilement automatiser la détection et l'exploitation de composants vulnérables
- Utilisation de moteurs de recherches spécialisés comme Shodan
- Pour le javascript, Retire.js
- Après quelques mois les vulnérabilités sont bien connues avec du code déjà disponible


----

- Certains outils comme metasploit peuvent exploiter la vulnérabilité
- Les politiques de sécurité concernant l'utilisation de composants tierces sont souvent inadaptées, inexistantes ou ignorées


---

<!-- .slide: data-background="https://www.windowsblogitalia.com/wp-content/uploads/2019/11/Bill-Gates-sorridente.jpeg" -->
## 2. Attaque

----

### Apache Struts2 CVE-2017–5638
- Remote Code Execution (RCE)
- CVSS score : **10**

----

#### Versions vulnérables
Apache Struts 2 2.3.x - 2.3.32 et 2.5.x - 2.5.10.1

----

### Détails de la faille
- Faille dans le **Jakarta Multipart** parser
- Content-Type Header Injection
- Expression **OGNL** (Object Graph Notation Language)
- Déclenche une **exception**
- Appelle une fonction de **construction de message d'erreur**
- Évalue l'expression OGNL/**exécute** son contenu.

----

```j=
Content-Type: %{(#_='multipart/form-data').
(#_memberAccess=@ognl.OgnlContext@DEFAULT_MEMBER_ACCESS).
(@java.lang.Runtime@getRuntime().
exec('curl localhost:8000'))}
```

----

### Exploitation
- [Struts2-showcase-2.3.12.war](https://repo1.maven.org/maven2/org/apache/struts/struts2-showcase/2.3.12/struts2-showcase-2.3.12.war)
- [L'exploit](https://www.exploit-db.com/exploits/41570/)

- [Serveur tomcat](https://tomcat.apache.org/download-70.cgi)


---

<!-- .slide: data-background="https://cbsnews1.cbsistatic.com/hub/i/2013/11/20/cf6a8acd-eee6-42b7-ac17-7d09ab9bc60c/bill_gates__crop.jpg" -->

## 3. Mesures pour s'en prémunir 

----

### Bonnes pratiques
- Maintenir ses systèmes à jour
- Supprimer toutes les dépendances, fonctionalités et composants inutiles
- Répertorier les versions des composants utilisés et leurs dépendances 
    - [OWASP Dependency Check](https://owasp.org/www-project-dependency-check/) 
    - [retire.js](https://github.com/retirejs/retire.js/)

----

### Sans oublier...
- Être alerté des vulnérabilités affectant vos composants
    - [NIST NVD](https://www.nist.gov/itl/nvd)
- Télécharger ses composants au niveau de sources officielles
- Remplacer les composants qui ne sont plus maintenus


---

## Sources
- [OWASP (Le type de faille)](https://owasp.org/Top10/fr/A06_2021-Vulnerable_and_Outdated_Components/)
- [MITRE (Notre CVE)](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-5638)
- [Explication de la faille](https://www.blackduck.com/blog/cve-2017-5638-apache-struts-vulnerability-explained.html)
- [Exemple détaillé de mise en place d'exploit](https://medium.com/@lucideus/exploiting-apache-struts2-cve-2017-5638-lucideus-research-83adb9490ede)
- [The unfortunate reality of insecure libraries pas Contrast Security](https://cdn2.hubspot.net/hub/203759/file-1100864196-pdf/docs/Contrast_-_Insecure_Libraries_2014.pdf)
