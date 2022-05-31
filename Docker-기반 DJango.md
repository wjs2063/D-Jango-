실행환경: mac m1



1. 먼저 빈폴더를 만든다 ( ex:Doker-DJango) 만든폴더로 이동한다   
2. Dockerfile 을 만든다 (확장자없음) (vim Dockerfile 로만들고  
3.
### --------------------------------  


#syntax=docker/dockerfile:1   
FROM python:3   
ENV PYTHONDONTWRITEBYTECODE=1   
ENV PYTHONUNBUFFERED=1   
WORKDIR /code   
COPY requirements.txt /code/   
RUN pip install -r requirements.txt   
COPY . /code/   


### --------------------------------  
Dockerfile 에 해당 명령어를 복사 붙여넣기  
4. 그리고 requirements.txt 파일을 생성한다   
Django>=3.0,<4.0   
psycopg2>=2.8   
5. 붙여넣기하고 저장   
6. 그리고 docker-compose.yml 을 만든다 ( vim docker-compose.yml)  



#### ------------------------------------------  
version: "3.9"   
   
services:   
  db:   
    image: postgres   
    volumes: 
      - ./data/db:/var/lib/postgresql/data   
    environment:   
      - POSTGRES_DB=postgres   
      - POSTGRES_USER=postgres     
      - POSTGRES_PASSWORD=postgres    
  web:   
    build: .   
    command: python manage.py runserver 0.0.0.0:8000   
    volumes:   
      - .:/code    
    ports:   
      - "8000:8000"   
    environment:    
      - POSTGRES_NAME=postgres   
      - POSTGRES_USER=postgres   
      - POSTGRES_PASSWORD=postgres   
    depends_on:   
      - db   
#### ------------------------------------------      
7. 해당 코드를 복사붙여넣기한다 여기서 db는 postgresql 을 사용함 ( mysql 로 바꾸려면 위명령어를 바꾸어주어야함)   
8.  그리고 sudo docker-compose run web django-admin startproject composeexample .  터미널에서 해당명령어 실행    
9.  설치후 ls -l 을 하면 6개의 파일이 생성되있는걸 볼수있음   
10.  그리고 composeexample.setting.py 에들어간후   

#### ------------------------------------------                 


DATABASES = {    
    'default': {   
        'ENGINE': 'django.db.backends.postgresql',   
        'NAME': os.environ.get('POSTGRES_NAME'),   
        'USER': os.environ.get('POSTGRES_USER'),   
        'PASSWORD': os.environ.get('POSTGRES_PASSWORD'),   
        'HOST': 'db',   
        'PORT': 5432,   
    } 
}   
#### ------------------------------------------  

해당 부분을 바꾸어주기


11. docker-compose up 실행 완료 
12. docker ps -a  터미널에서 입력후  해당 컨테이너 id 를 확인   
13. docker start 해당 컨테이너 id 복붙 
14. docker ps -a 로 해당 컨테이너 UP 되어있는지 확인 (되어있다면)    
15. vscode 에있는 스토어에들어가서 remote development 설치   
16. shift+command+p 누르고 attach 모드 클릭 -> 접속가능한 컨테이너가 보일것 -> 컨테이너접속하면 vscode 창이 새로뜸 -> 그러면 컨테이너 내부환경에서 vscode 를 실행하는것   
17. 그리고 ctrl +  ~  를 누르면 vscode 내에서 터미널이 나옴 -> root 디렉토리에서 우리가원하는 프로젝트파일을 찾아서 change directory 를 해줌 (cd명령어) 어디있는지 모르겠다면 ls -a 또는   Dockerfile 이나 compose.yml 에 우리가 지정한 위치가 있음    
18. 그리고 나서 Django 의 setting.py 또는 urls.py 에 접근가능 그리고 개발진행!!     

docker exec -it [컨테이너id] /bin/bash --> 해당 컨테이너를 /bin/bash 셀로 실행하겠다는 의미  
docker stop [컨테이너id] 멈추기  
docker ps -a  --> 생성된 컨테이너 를 보여줌  
docker rm [컨테이너id] 해당   


#### 중요한것 setting.py 에 ALLOWED 파트에서 빈리스트로 되어있을것임 해당 리스트를 ['*'] 로 바꿔주자 
그리고 python manage.py runserver 를 한후  로컬주소 포트 8000 ( 포트는 미리 yml 파일에서 정의함) 으로 접속하면 장고설치완료페이지가뜬다! 이제 개발진행~
