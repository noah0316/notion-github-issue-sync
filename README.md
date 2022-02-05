# Github Actions x notion-sdk-js x docker

![Slide 16_9 - 3](https://user-images.githubusercontent.com/63908856/152655068-abe614f8-46cc-4e2b-addf-d775068b15d4.png)


## Demo
 ![screencast 2022-02-06 02-55-56](https://user-images.githubusercontent.com/63908856/152653248-95228c28-97f2-4584-a486-cc6d64a2eb97.gif)

## Abstract
[notion-sdk-js](https://github.com/makenotion/notion-sdk-js/tree/main/examples/notion-github-sync)는 
Notion 데이터베이스와 Github의 issue의 를 동기화시켜주는 Node-JS application 입니다.  
이를 사용하기 위해선 사용자가 local computer에 dependency module을 install 한 후에  
사용자가 직접 `node index.js` 명령어를 사용해 실행해주어 실행 시점을 기준으로 Notion 데이터베이스와 Github의 issue를 동기화합니다.  

사용자의 편의를 향상시키기 위해 해당 Node-JS application을 Dockerizing하여  
Notion 데이터베이스와 Github issue 연동을 Github Actions에서 사용자가 쉽게 사용할 수 있게 구성하였습니다.

## Usage

아래의 Workflow 정의를 통해   
쉽게 여러분의 Repo에서 Github Issue와 Notion 데이터베이스 연동을 구성할 수 있습니다.

```yml
on:
  issues:
    types: [opened, reopened, closed, deleted]
jobs:
  build:
    runs-on: ubuntu-latest
    name: "Run github issue notion sync"
    steps:
      - uses: actions/checkout@v2
      - name: create env file
        run: |
          touch .env
          echo PERSONAL_ACCESS_KEY=${{ secrets.PERSONAL_ACCESS_KEY }} >> .env
          echo NOTION_KEY=${{ secrets.NOTION_KEY }} >> .env
          echo NOTION_DATABASE_ID=${{ secrets.NOTION_DATABASE_ID }} >> .env
          echo REPO_OWNER=${{ secrets.REPO_OWNER }} >> .env
          echo REPO_NAME=${{ secrets.REPO_NAME }} >> .env
      - name: docker compose up
        run
```

본 Application을 Github Actions에서 사용하기 위해서는 5개의 환경변수(environment variable)를 필요로 합니다.
1. PERSONAL_ACCESS_KEY: Github Personal Access Token
2. NOTION_KEY: Notion API KEY
3. NOTION_DATABASE_ID: Notion Database ID
4. REPO_OWNER: Github Repository Owner
5. REPO_NAME: Github Repository Name
---
## STEP 1
- GitHub Personal Access token을 발급받는 방법은 해당 가이드를 참조해주세요!  [here](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token).  
## STEP 2
- Notion API key를 발급받는 방법은 해당 가이드를 참조해주세요! [here](https://www.notion.com/my-integrations)
## STEP 3
- NOTION_DATABASE_ID를 발급받기 위해 먼저 해당 [DataBase template](https://www.notion.com/367cd67cfe8f49bfaf0ac21305ebb9bf?v=bc79ca62b36e4c54b655ceed4ef06ebd)을 여러분의 Notion workspace에 복제해주세요!

![Notion](https://user-images.githubusercontent.com/63908856/152652176-92e61d13-0759-45b3-b41a-d4521bb62ac3.png)

- 발급받은 integration을 DataBase template이 있는 Notion에 초대해줍니다.

- Notion Database ID는 다음과 같습니다.  
해당 Notion Page를 공유했을 때 나오는 link는 다음과 같이 생겼을 것입니다.  
`https://www.notion.com/367cd67cfe8f49bfaf0ac21305ebb9bf?v=bc79ca62b36e4c54b655ceed4ef06ebd`  
위 링크에서 데이터베이스의 ID는  
367cd67cfe8f49bfaf0ac21305ebb9bf입니다.


- 아래의 링크에서 long_hash_1이 Notion Database의 ID입니다.  
`https://www.notion.so/<long_hash_1>?v=<long_hash_2>`  
모든 링크의 패턴이 위와 같습니다.   

## STEP 4
- 발급 받은 키들을 Repo의 환경변수로 등록합니다.

1. 연동을 원하는 Repo의 Settings 탭으로 이동합니다.
2. Secrets의 Actions button을 click 해주세요.
![Screen Shot 2022-02-06 at 2 38 14](https://user-images.githubusercontent.com/63908856/152652504-acf7f59a-8f3d-4e46-8aad-13baaa287d2d.png)
3. New Repository Secret button을 이용해 Repository Secret을 등록합니다.
![repo](https://user-images.githubusercontent.com/63908856/152652563-7a4b8619-f414-4d0a-892a-a8c525ded6c6.png)
4. 현재 Repository에서 REPO_OWNER는 noah0316이며,  
   REPO_NAME은 notion-github-issue-sync입니다. 
![Screen Shot 2022-02-06 at 2 41 21](https://user-images.githubusercontent.com/63908856/152652614-a452da1b-84c9-4feb-94df-12be2b270887.png)

## STEP 5
- 해당 [docker-compose.yml](https://github.com/noah0316/notion-github-issue-sync/blob/main/docker-compose.yml) 파일을 여러분의 Repo에 upload해주세요!

---

이제 모든 설정이 끝났습니다!!

이제 Workflow 구성을 통해 Notion과 Github Issue가 연동되는 것을 확인하세요!!

