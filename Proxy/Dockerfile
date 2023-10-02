FROM python:3.11.5

WORKDIR /app

COPY . /app

RUN pip install --no-cache-dir -r requirements.txt
RUN pip install --no-cache-dir --upgrade Flask Jinja2

EXPOSE 8080

CMD ["python", "hello_world.py"]