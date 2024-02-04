# Introduction

Fastweb is a kind of cms developed using a specific stack:

- Actix-web as a basic framework
- Tera as template engine
- Sqlx as conection to database 
- Postgres as database
- Redis for cache

## Objective

The main objective of FastWeb is serve as starter project to develop sites and web systems with Rust. 

I used Web2py (a Python Framework), for years, and liked the fact we can start with a template application (welcome), 
which we can 'copy' and start configuring and modifying. 

## How start

The intended use of FastWeb is you clone the github repository to a a new folder to your project:

    git clone https://github.com/aerun/fastweb.git myproject

## The structure

The current structure is:

TODO: place a copy of tree on fastweb directory


## What modify?

You probably want to change the name of project on Cargo.toml
You need to copy ```.env.example``` to ```.env``` and replace the data to your local configuration

