FROM python:3.8-buster
LABEL maintainer="Djatchue Olivier <olivierdja@yahoo.fr>"
COPY . .
RUN apt update -y && apt install python-dev python3-dev libsasl2-dev python-dev libldap2-dev libssl-dev -y
RUN pip3 install -r /requirements.txt
VOLUME /data/
EXPOSE 5000
CMD [ "python3", "./student_age.py" ]
