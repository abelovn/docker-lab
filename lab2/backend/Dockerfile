FROM python:3.6
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1
WORKDIR /backend
COPY ./ /backend
RUN pip install --upgrade pip && pip install -r requirements.txt
EXPOSE 3000
ENTRYPOINT ["python", "manage.py", "runserver", "0.0.0.0:3000"]
