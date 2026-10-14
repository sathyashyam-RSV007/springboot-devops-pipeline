# Pick Your Sky ☀️🌙

A small web app with an animated toggle switch that flips the whole page between **daytime** and **nighttime**. Built with **Java** and **Spring Boot**.

## Preview

| Daytime | Nighttime |
|---------|-----------|
| ![Daytime](docs/images/Day.png) | ![Nighttime](docs/images/Night.png) |

## Features

- Animated toggle switch (sun ↔ moon)
- Full-page theme change: sky gradient, sun/moon glow, stars, clouds and hills
- Label updates with the mode (*Daytime* / *Nighttime*)
- Lightweight, no database needed

## CI/CD Pipeline (Jenkins)

Every `git push` to GitHub triggers a Jenkins build that compiles, tests and packages the app with Maven.

### Auto-Trigger on Every Push

- Poll SCM (works on a local VM)
- In the Jenkins job, tick Poll SCM
- Schedule: `* * * * *` (checks GitHub every minutes)