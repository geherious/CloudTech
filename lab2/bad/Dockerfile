FROM python:latest
 

RUN apt-get update
RUN apt-get install -y curl
RUN apt-get install python3-pip -y

COPY server.py ./
 
EXPOSE 8000
 
CMD python server.py

