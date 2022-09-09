## personal web natigation station

###### Description

- static navigation station at the entrance of personal website, displaying relevant personal websites

###### Technology stack

- typescript,vue3,yarn,vite
- vue3-particles

###### Environment prepare

- install npm：
  
  - [node env install(required)](https://www.runoob.com/nodejs/nodejs-install-setup.html)
  - [node version manage install(optional)](https://www.runoob.com/w3cnote/npm-switch-repo.html)

- install yarn
  
  ```
  npm install -g yarn
  ```

- install dependencies
  
  ```shell
  yarn install
  ```

###### Start dev

```shell
yarn dev
```

visit：[http://127.0.0.1:5173](http://127.0.0.1:5173)

###### Deploy

- build
  
  ```shell
  yarn build
  ```

- static hosting
  
  - [github pages](https://cuukenn.github.io)
    
    ```shell
    yarn deploy:github
    ```
  
  - [gitee pages](https://cuukenn.gitee.io)
    
    ```shell
    yarn deploy:gitee
    ```
