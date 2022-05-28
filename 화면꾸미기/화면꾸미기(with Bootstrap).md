## 화면 예쁘게 꾸미기


#### 웹페이지에 디자인 적용하려면 css 사용, style sheet 를 pybo에 활용하려면 css파일이 static 에 존재해야함

가상환경 myvc 진입

1. config/setting.py 를열어 STATICFILES_DIRS=[BASE_DIR/'static'] 
2. mkdir static 
3.static 디렉토리에 style.css 파일을 만들고 코드작성
4.질문상세템플릿에 스타일적용--> {% load static %} <link rel='stylesheet' type='text/css' href="{% static 'style.css' %}">


## BOOTSTRAP 활용

BOOTSTRAP 4.x버젼다운

bootstrap.min.css 를 static 디렉토리에 복사붙혀넣기

question_list.html 파일의 코드대로 작성

#### 질문상세 템플릿에 BOOTSTRAP 사용 <question_detail.html>


