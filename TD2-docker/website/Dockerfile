# Get base image 
FROM python:3.14-slim


COPY . /app

WORKDIR /app

# Add instructions to build the SuperDB binary

RUN pip install -r requirements.txt

ENV FLASK_APP=flask_minimal.py

CMD ["flask", "run", "--host=0.0.0.0"]
