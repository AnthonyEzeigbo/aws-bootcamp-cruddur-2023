FROM python:3.10-slim-buster

# inside container
# WORKDIR makes a new folder inside the comtainer
WORKDIR /backend-flask

# outside -> inside container
# this contains the libraries we want to install to run the app
COPY requirements.txt requirements.txt

# inside container
# this will install the python libraries that will be used for the app 
RUN pip3 install -r requirements.txt

# outside container -> inside container
# "." means everything in the current directory
# first "." represent everything in backend-flask (outside the container)
# second "." represent everything in the  backend-flask (inside the container)
COPY . .

# sets environment Variables (Env Vars)
# inside the container and will remain set when the container is running
ENV FLASK_ENV=development

EXPOSE ${PORT}

# cmd (command)
# python3 -m flask run --host=0.0.0.0 --port=4567
CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]