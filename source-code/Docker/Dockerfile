FROM python:3.9-slim
WORKDIR /app
COPY . .
RUN pip uninstall barcode
RUN pip install -r requirements.txt
EXPOSE 8083
ENTRYPOINT ["python3"]
CMD ["python", "app.py"]
