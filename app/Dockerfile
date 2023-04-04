FROM ubuntu:18.04

# Install Prerequisite Packages
RUN apt-get update && apt-get install -y \
    bison \
    cmake \
    flex \
    g++ \
    gcc \
    gfortran \
    grace \
    libboost-numpy1.65-dev \
    libboost-python1.65-dev \
    libopenblas-dev \
    make \
    python3 \
    python3-dev \
    python3-numpy \
    tk \
    wget \
    zlib1g-dev

# Download, Extract, & Compile Source Code
WORKDIR /tmp
RUN wget https://code-aster.org/FICHIERS/aster-full-src-14.6.0-1.noarch.tar.gz
RUN tar xzvf aster*
WORKDIR /tmp/aster-full-src-14.6.0

RUN pwd

ENV LANG C.UTF-8
RUN python3 setup.py install --noprompt

# set work directory
WORKDIR /usr/src/app

# set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# install psycopg2 dependencies
RUN apk update \
    && apk add postgresql-dev gcc python3-dev musl-dev

# install dependencies
RUN pip install --upgrade pip
COPY ./requirements.txt .
RUN pip install -r requirements.txt

# copy entrypoint.sh
COPY ./entrypoint.sh .
RUN sed -i 's/\r$//g' /usr/src/app/entrypoint.sh
RUN chmod +x /usr/src/app/entrypoint.sh

# copy project
COPY . .

# run entrypoint.sh
ENTRYPOINT ["/usr/src/app/entrypoint.sh"]
