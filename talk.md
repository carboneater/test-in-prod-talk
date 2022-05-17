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

- Tester en Prod?
- Pourquoi
- Comment

---

## $ whoami

<!-- .slide: data-background="imgs/lightbulb.gif" -->

---

<!-- .slide: data-background="imgs/idontalwaystestmycode.jpg" -->

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
|                | DB de Prod      |

--

# Pourquoi

### Développer local avec BD de dev

| Pros           | Cons                |
|----------------|---------------------|
| Vitesse + = -  | Stabilité + + ?     |
|                | Works on my Machine |

--

## Works on my Machine

- Service?
- Configuration?
- Secret?
- Data-Driven Behavior?
- Pipeline?
- ... ?

---

# Comment?

<img src="imgs/DevOpsLoop.png" />

---

# Comment?

- Qu'est-ce qu'un test?
- Architecture du projet
- Qu'est-ce que je veux tester?

---

## Qu'est-ce qu'un test?

<img src="imgs/DevOpsLoop.png"/>

_*La longueur des segments n'est pas à l'échelle avec leur durée_

--

## Qu'est-ce qu'un test?

<img src="imgs/compile_test1.png" />

--

## Qu'est-ce qu'un test?

<img src="imgs/opentelemetry-icon-color.png" width="256"/>
<img src="imgs/prometheus.png" width="256"/>
<br/>
<img src="imgs/Grafana_logo.png" width="256" />
<img src="imgs/loki.png" width="256" />
<img src="imgs/tempo.png" width="256" />

--

## Qu'est-ce qu'un test?

<!-- .slide: data-background="imgs/phone.png" -->

---

## Architecture

Programmes Interactifs vs Non-Interactifs

- Aucune interactivité
- Interactivité partielle
- Interactivité complète

--

### Programmes Non-Interactifs

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

--

### Programmes Interactifs

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

--

### Programmes Semi-Interactifs

```mermaid
flowchart LR

subgraph CI
    B[Build]
    T[Unit + Integration Tests]
    L[Launch Service]
    W[Wait-On Service]

    B & T --> L & W
end

subgraph Staging
    SD[Deploy]
    SW[Wait-On Service]
end

subgraph Prod
    PD[Deploy]
    PW[Wait-On Service]
end

W -.-> SD & SW
SW -.-> PD & PW
```

--

### Interactivité

```mermaid
sequenceDiagram

participant E as Emulator
participant A as Board
participant U as GPS
participant J as GenX/J
participant T as GenX/TS
participant M as Message Bus

E ->> +M: Register on Position Event Channel

Note Over E: Generate Test Payload
activate E
E ->> +A: Transmit Payload
Note over A: UUEncode
A ->> +U: Transmit
deactivate A
U ->> +J: Transmit OtA
deactivate U
Note over J: Decode Message
J ->> +T: Handoff
deactivate J
Note over T: Parse Payload
Note over T: Save
T ->> +M: Emit Position
deactivate T
M ->> E: Position
deactivate M
Note over E: Compare Position / Payload

```

---

### Quels tests?

<img src="imgs/compile_test1.png" />

--

### Quels Tests?

- E2E
- Lifetime