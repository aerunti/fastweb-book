## Install FastAPI

With python we don't 'install' something. 
It's better say 'prepare the enviroment' for it. 


the recipes bellow are for Ubuntu distro. you need to adapt to your OS, or the last python version avaliable. 

    sudo apt install build-essential python3.11-venv python3-pip libpq-dev 

Create a folder to your project Ex: myfirstapi

    mkdir myfirstapi; cd myfirstapi


Create a python enviroment to your project 


    python3 -m venv .venv


Every time you came back to work with this project, you will neeed activate the env. Do it now: 


    source .venv/bin/activate


Create a file named 'requirements.txt' and edit it. Replace 'nano' by your code editor


    touch requirement.txt
    nano requirements.txt 

Put this content inside:


    fastapi[all]
    pydal
    psycopg2 # or your database python driver



You can find PyDAL(DAL) documentation here: [PyDAL Documentation](http://www.web2py.com/books/default/chapter/29/06/the-database-abstraction-layer)

Now you can install the requirements:

    pip install -r requirements.txt



lets create some folders we will later:

    mkdir databases
    mkdir logs
    mkdir services


Now lets create a very basic project content to start. Create a file named 'main.py':

    touch main.py
    nano main.py


and put this content. Adapt the code below to your real case:




    from typing import Union
    from starlette.responses import  RedirectResponse, Response
    from fastapi import FastAPI
    from pydantic import BaseModel
    # add log
    from pydal import DAL,Field

    import logging
    from logging.handlers import RotatingFileHandler
    log_format = '%(asctime)s || %(processName)-10s || %(name)s [%(levelname)-5s]: %(message)s'
    logging.basicConfig(level=logging.DEBUG, format=log_format)
    root_logger = logging.getLogger()
    h = logging.handlers.RotatingFileHandler(
        'logs/main.log', 'a', 30 * 1024 * 1024, 10)
    f = logging.Formatter(log_format)
    h.setFormatter(f)
    root_logger.addHandler(h)
    logger = logging.getLogger(__name__)


    app = FastAPI()

    # you can generate a token using :
    # openssl rand -hex 40
    token = "57da97b81116d12323f30747ada940386ff3cfbf91e97de9a2b9cb0b73cbb0c6d2b021f49f259826"

    ## postgresql
    # db = DAL('postgres://username:password@localhost/dbname')

    ## mysql
    # db = DAL('mysql://username:password@localhost/test?set_encoding=utf8mb4')

    ## other databases check http://www.web2py.com/books/default/chapter/29/06/the-database-abstraction-layer#Connection-strings-the-uri-parameter-
    # they may require install python drivers


    db = DAL('sqlite://database.sqlite')

    class Login(BaseModel):
        token: str
        email: str
        senha: str


    def encript_password(senha):
        #implement your hash
        hashed_password = senha


        return hashed_password



    @app.get("/")
    def read_root():
        return RedirectResponse(url='/docs')

    @app.get("/v1/login")
    def login(login:Login):
        logger.info(f"trying to login {login.email}")
        ## example, adapt the query for your database
        rows = db.executesql(""" select email, full_name, passw, perfil 
                            from user
                            where email = $1 """,placeholders = (login.email))
        print(rows)
        if len(rows) > 0:
            row = rows[0]
            encripted_password = encript_password(login.senha)

            if encript_password == row['passw']:
                # adapte para as colunas do seu banco de dados
                return {'full_name': row['full_name'],
                        'email': row['email'],
                        'perfil': row['perfil']}


        return {'message': 'the login is not valid'}
    
    


Let's create a file dev_start to start our project on development mode:

    touch dev_start.sh


And put this content:

    uvicorn main:app --reload --host 0.0.0.0 --port 8000


Execute it to start development:

    bash dev_start.sh


Lets create a file 'do_login.sh' to test our endpoint

    touch do_login.sh

and put this content inside:

    # you should change the url if necessary and login data to a real one
    curl -X POST http://localhost:8000/v1/login -d {"email": "myuser@mydomain.com", "senha": "123456"}

Save it and execute it with 

    bash do_login.sh

Adapt your code in main.py until it works as expected. 



