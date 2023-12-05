
FROM python:3.10
 

RUN apt-get update && apt-get install -y curl

COPY server.py ./
 
EXPOSE 8000
 
CMD ["python", "server.py"]

