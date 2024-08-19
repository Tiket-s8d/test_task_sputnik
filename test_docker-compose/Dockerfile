FROM python:3.9-slim

WORKDIR /opt/app

COPY /web_app . 
RUN pip install -r requirements.txt

ENTRYPOINT ["python", "main.py"]
