# rahulgurung.com

Welcome to the codebase of my personal portfolio website. It's built with Hexo 7, a fast, simple, and powerful blog framework, powered by Node.js.

## Table of Contents

- [Getting Started](#getting-started)
- [Installation](#installation)
- [Building](#building)
- [Running Locally](#running-locally)
- [Deploy](#deploy)

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

Ensure you have the latest version of [Node.js and npm](https://nodejs.org/en/download/) installed on your machine.
Install `nvm` and just do
```bash
nvm use
```

## Installation

Follow these steps to setup the project locally:

Installing deps

```bash
npm install
```


## Building 

Build the static files of your website using the following command:

```bash
hexo generate
```

This command generates a `public` folder with all the static files ready to be served by a web server.

## Running Locally

To run the website locally, use the command:

```bash
hexo server
```

After running the command, open your web browser and navigate to `http://localhost:4000`.

## Deploy

To deploy change to gh-pages

```bash
npm run deploy
```