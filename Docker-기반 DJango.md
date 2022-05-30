실행환경: mac m1



1. 먼저 빈폴더를 만든다 ( ex:Doker-DJango) 만든폴더로 이동한다   
2. Dockerfile 을 만든다 (확장자없음) (vim Dockerfile 로만들고  
3.
###--------------------------------


#syntax=docker/dockerfile:1   
FROM python:3   
ENV PYTHONDONTWRITEBYTECODE=1   
ENV PYTHONUNBUFFERED=1   
WORKDIR /code   
COPY requirements.txt /code/   
RUN pip install -r requirements.txt   
COPY . /code/   


###--------------------------------
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
