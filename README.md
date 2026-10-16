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

## Dockerize the Application

- Install Docker on the Ubuntu and give your user and Jenkins access to it
- Add a Dockerfile (Java 21 JRE + `darkmode-1.0.0.jar`) in the project root
- Build the image `springboot-darkmode:1.0` from the Dockerfile
- Run the container with port 8081 mapped to 8080 and test at `http://localhost:8081`