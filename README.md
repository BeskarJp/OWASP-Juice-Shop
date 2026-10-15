# Hack & Juice — Cybersecurity Project (OWASP Juice Shop)


## Présentation du Projet

**Hack & Juice** est un projet de cybersécurité (module `G-SEC-200`) proposé par **Epitech** basé sur l'application vulnérable **OWASP Juice Shop**. L'objectif était de réaliser le pentest d'une application web moderne, d'identifier ses vulnérabilités et de rédiger une documentation technique complète pour chaque faille exploitée.

> **Résultat :** Notre duo a terminé **1ère du campus**.

---

## Contexte & Challenge Temporel

* **Durée du projet :** 2 semaines.
* **Format :** CTF (Capture The Flag) avec environnement dédié par équipe.
* **Périmètre :** Compte tenu du délai imparti de 2 semaines, nous avons fait le choix stratégique de cibler en priorité les challenges de **niveau 3 à 6 étoiles** (les plus complexes et formateurs) plutôt que de viser la quantité brute.
* **Note sur les contraintes Epitech :** Conformément au sujet, la documentation technique s'est focalisée sur les challenges de difficulté **≥ 3★**.

---

## Structure du Dépôt & Rédacteur Obsidian

Toute la documentation a été intégralement rédigée sous **Obsidian** en Markdown pur pour garantir un maillage d'informations fluide, propre et structuré.

Les writeups sont classés par dossiers correspondant aux **catégories du TOP 10 OWASP** et nommés selon la convention imposée :

$$\text{\`Category/Category-Difficulty-ChallengeName.md\`}$$

### Arborescence des vulnérabilités documentées

```text
.
├── Broken Access Control
│   ├── Broken Access Control-3-Forged Feedback.md
│   ├── Broken Access Control-3-Forged Review.md
│   ├── Broken Access Control-3-Manipulate Basket.md
│   └── Broken Access Control-4-Easter Egg.md
│
├── Broken Anti Automation
│   ├── Broken Anti Automation-6-Multiples likes.md
│   └── Broken Anti Automatisation-5-Extra Language.md
│
├── Broken Authentication
│   ├── Broken Anti Automatisation-3-Caaptcha Bypass.md
│   └── Broken Authentification-3-Reset Jim's Password.md
│
├── Cryptographic Issues
│   ├── Cryptographic Issues-4-Nested Easter Egg.md
│   └── Cryptographic Issues-6-Premium Paywall.md
│
├──  Improper Input Validation
│   ├── Improper Input Validation-3-Admin Registration.md
│   ├── Improper Input Validation-3-Deluxe Fraud.md
│   ├── Improper Input Validation-3-Payback Time.md
│   └── Improper Input Validation-4-Poison Null Byte.md
│
├── Injection
│   ├── Injection-3-Database Schema.md
│   ├── Injection-3-Login Bender.md
│   └── Injection-3-Login Jim.md
│
└── Sensitive Data Exposure
    ├── Sensitive Data Exposure-3-Login Amy.md
    ├── Sensitive Data Exposure-4-Access Log.md
    ├── Sensitive Data Exposure-4-Forgotten Developer Backup.md
    ├── Sensitive Data Exposure-4-Forgotten Sales Backup.md
    ├── Sensitive Data Exposure-4-Misplaced Signature File.md
    ├── Sensitive Data Exposure-4-Reset Uvogin's Password.md
    ├── Sensitive Data Exposure-5-Email Leak.md
    └── Sensitive Data Exposure-5-Leaked API Key.md

```

---

## Format standardisé des Writeups

Chaque fichier de documentation (`.md`) suit une structure rigoureuse répondant aux exigences d'un rapport de pentest professionnel :

1. **Méthodologie détaillée :**

* Étapes numérotées et reproductibles ($1, 2, 3...$)
* Outils utilisés (*Burp Suite, SQLmap, DevTools, Scripts Python, Wordlists, etc.*)
* Techniques d'énumération, de scan ou de brute-force.


2. **Analyse de la vulnérabilité :**

* Nom exact de la faille et identification de la référence CWE / OWASP.
* Composants applicatifs impactés (*Base de données, système d'authentification, API REST...*).
* Évaluation du niveau de sévérité (*Low, Medium, High, Critical*).


3. **Impact Métier (Business Impact) :**

* Risques de fuite de données, élévation de privilèges, usurpation d'identité, atteinte à la réputation, etc.


4. **Remédiation & Recommandations :**

* Stratégies d'atténuation des risques (Mitigation).
* Correctifs de code ou de configuration proposés.
* Bonnes pratiques de sécurité associées.


---

## Outils & Technologies Utilisés

* **Recon & Exploitation :** Burp Suite, Exegol, DevTools Browser.
* **Analyse de Code & Scripting :** Python 3, Bash, CyberChef, etc
* **Documentation & Prise de notes :** [Obsidian](https://obsidian.md/), Git/GitHub.

---

## Avertissement

Ce dépôt est publié dans un cadre strictement pédagogique et de démonstration de compétences. Les solutions présentées ont été élaborées dans un environnement d'entraînement fermé et autorisé (*OWASP Juice Shop*).
