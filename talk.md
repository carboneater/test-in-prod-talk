---
theme: night
---

## Tester en Production

Pourquoi. Comment. Comment Pas.

---

### Précédemment aux TechnoDrinks

<img src="imgs/stryker.png" width="256" />
<br/>

<img src="imgs/drone-logo-png-dark-256.png" width="256" />
<img src="imgs/semantic-release-logo.svg" width="256" />
<img src="imgs/renovate.png" width="256" />
<br/>

<img src="imgs/Grafana_logo.png" width="256" />
<img src="imgs/loki.png" width="256" />
<img src="imgs/verdaccio.png" width="256" />

---

### Précédemment aux TechnoDrinks

<img src="imgs/inverted-pyramid.png" />

---

## ToC

- Pourquoi
- Comment

---

## $ whoami

<!-- .slide: data-background="imgs/lightbulb.gif" -->

---

<!-- .slide: data-background="imgs/breaking-prod.png" -->

---

# Pourquoi

### Développer sur le serveur en prod

| Pros          | Cons            |
|---------------|-----------------|
| Vitesse + + + | Stabilité - - - |
|               | Max 1 Dev       |

--

# Pourquoi

### Développer local avec la BD de prod

| Pros           | Cons            |
|----------------|-----------------|
| Vitesse + + -  | Stabilité + - - |
| Multiples Devs |                 |

--

# Pourquoi

### Développer local avec BD de dev

| Pros           | Cons                |
|----------------|---------------------|
| Vitesse + = -  | Stabilité + + ?     |
| Multiples Devs | Works on my Machine |

--

## Works on my Machine

- Service?
- Configuration?
- Secret?
- Data-Driven Behavior?
- Pipeline?

---

# Comment?

- Architecture du projet
- Qu'est-ce que je veux tester?

---

## Architecture

Programmes Interractifs vs Non-Interractifs

- Aucune interractivité
- Interractivité partielle
- Interractivité complète

---

### Programmes Non-Interractifs

```mermaid
graph LR

subgraph CI
build --> T[Unit Tests]
end

subgraph Prod
    Deploy
end

T -.-> Deploy
```

---

### Programmes Interractifs

```mermaid
flowchart LR

subgraph CI
    B[Build]
    T[Unit + Integration Tests]
    L[Launch Service]
    W[Wait-On Service]
    E2E[E2E Tests]

    B & T --> L & W
    W --> E2E
end

subgraph Staging
    SD[Deploy]
    SW[Wait-On Service]
    SE2E[E2E Tests]

    SW --> SE2E
end

subgraph Prod
    PD[Deploy]
    PW[Wait-On Service]
    PE2E[E2E Tests]

    PW --> PE2E
end

E2E -.-> SD & SW
SE2E -.-> PD & PW
```

---

### Quels tests?

<img src="imgs/compile_test1.png" />

--

### Quels Tests?

- E2E