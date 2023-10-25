# Getting Started with Create React App

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Requirement

- NodeJS 16 or newer
- Linux OS

## Available Scripts

At first you need to run

### `npm install`

Or

### `yarn install`

In the project directory, you can run:

### `yarn start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.\
You will also see any lint errors in the console.

## Build with Docker

Start the Terminal and run the following command to build the image:

### `docker build -t react-app .`

Use the command below to run the container on port 3000:

### `docker run -d -p 3000:3000 react-app`

Then, access [http://localhost:3000](http://localhost:3000) to view it in the browser.

## Use docker-compose to run the project on port 80 with Nginx

Run the following command:

### `docker-compose -f docker-compose.yaml up -d`

Open [http://localhost:80](http://localhost:3000) to view it in the browser.

