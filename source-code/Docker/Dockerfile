FROM python:3.9-slim
WORKDIR /app
COPY ./*  /app/
RUN pip uninstall barcode
RUN pip install -r requirements.txt
EXPOSE 80
ENTRYPOINT ["python3"]
CMD ["app.py"]

