# GitLab CI image for Typescript & React Native to build Android App

This Docker image contains React-native with TypeScript supported and the Android SDK and most common packages necessary for building Android apps in a CI tool like GitLab CI.

## Disclaimer

This script is based on https://github.com/cuisines/gitlab-ci-react-native-android with TypeScript supported.

## GitLab CI example

```yaml
image: thuanle/gitlab-ci-react-native-typescript-android

stages:
- build-js
- verify-coding-style
- build-android

cache:
  key: ${CI_PROJECT_ID}
  paths:
  - android/.gradle/
  - android/build/App/
  - node_modules/
  - build/

build-verify-coding-style:
  stage: verify-coding-style
  script:
  - yarn lint
  tags:
    - docker

build-js:
  stage: build-js
  script:
  - yarn
  - tsc
  - gulp
  tags:
    - docker

build-android:
  stage: build-android
  script:
  - cd android && ./gradlew assembleRelease
  artifacts:
    paths:
    - android/app/build/outputs/apk/
  tags:
    - docker
  
```

