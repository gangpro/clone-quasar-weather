# [Vue] README
> YouTube : [freeCodeCamp.org](https://youtu.be/kWRpEcv2nu8)
> </br>
> SKILL : `Vue.js`, `Quasar Framework`, `OpenWeather API`
> </br>
> [크롬 개발자도구 Vue 확장 프로그램](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd/related?hl=ko)

## 퀘이사 설치 및 프로젝트 생성

### Quasar CLI 설치

```bash
➜ npm install -g @quasar/cli
```

### Quasar 프로젝트 생성
```bash
➜ npm init quasar

...

✔ What would you like to build? › App with Quasar CLI, let's go!
✔ Project folder: … clone-quasar-weather
✔ Pick Quasar version: › Quasar v2 (Vue 3 | latest and greatest)
✔ Pick script type: › Javascript
✔ Pick Quasar App CLI variant: › Quasar App CLI with Webpack (stable)
✔ Package name: … clone-quasar-weather
✔ Project product name: (must start with letter if building mobile apps) … clone-quasar-weather-gangpro
✔ Project description: … A Quasar Project
✔ Author: … gangpro
✔ Pick your CSS preprocessor: › Sass with indented syntax
✔ Check the features needed for your project: › Axios

...

➜
```

### Quasar API
> https://quasar.dev/

### 이슈. 위치 정보 가져오기
> navigator.geolocation.getCurrentPosition 사용 시 위치 정보 못가져오는 이슈 발생
- Mac > System Preferences > Security & Privacy > Privacy
  - Location Services > Google Chrome.app 위치정보 사용 활성화 하기...

### Weather Open API
> https://openweathermap.org/api
- API call
```text
https://api.openweathermap.org/data/2.5/weather?lat={lat}&lon={lon}&appid={API key}
```

### Free Weather API
> https://freegeoip.app/json/


## 기본 정보
### Install the dependencies
```bash
yarn
# or
npm install
```

### Start the app in development mode (hot-code reloading, error reporting, etc.)
```bash
quasar dev
```


### Build the app for production
```bash
quasar build
```

### Customize the configuration
See [Configuring quasar.config.js](https://v2.quasar.dev/quasar-cli-webpack/quasar-config-js).
