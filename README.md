# TodoWebApp
Programmers - 2019 Summer Coding 2nd Work    

duration : 2019.05.13 ~ 20  
environment : Windows 10  
tool : WebStorm  
framework : Vue.js  
Library : Material Design Bootstrap 4  
author : Einere

## About TodoWebApp
TodoWebApp is todo SPA programmed by Vue.js, Material Design Bootstrap 4.    
This app use Webpack server instead of Node.js.  


## How to build
### Requirements
- npm

### Build
1. clone or download.
2. go to project directory.
3. type "npm run dev"
  
## Link
http://summercoding2019.s3-website.ap-northeast-2.amazonaws.com/#/
  
## Issue
1. Date객체 인스턴스의 getMonth()함수가 비정상적인 결과값을 리턴하여, 마감시간 기능이 오작동하는 이슈가 있습니다.
2. mdb-numeric-input태그의 min값과 max값 설정이 정상적으로 반영되지 않는 이슈가 있습니다.

2번 이슈에 대해, Vue debugger로 뜯어보니 mbd-numeric-input내부의 vue-numeric객체의 data의 amount값이 설정한 min&max값에 상관없이 증감하는 것을 확인했습니다.
하지만 어떻게 디버깅을 해야 할지 갈피를 못 잡은 상황입니다.
