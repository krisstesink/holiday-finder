# holiday-finder
A simple Vue.js application that allows users to search for public holidays by country and filter or sort them by date or name. The app fetches holiday data from an external API and displays it in a user-friendly table.

## Features
    - Search for holidays by country: Select a country to get a list of public holidays for a selected year range.
    - Filter holidays by year: Choose the year range to filter holidays.
    - Sort holidays: Sort holidays by date (ascending/descending) or by name (A-Z or Z-A).
    - Wikipedia links: Click on a the "Find out more" link to view additional information from Wikipedia.

## Prerequisits
Before getting started, ensure that you have the following installed on your machine:
    - Node.js: The project requires Node.js. You can download it from nodejs.org.
    - npm: npm (Node Package Manager) comes bundled with Node.js.


## Getting Started
1. Clone the repository to your local machine using: 
```
git clone https://github.com/krisstesink/holiday-finder.git
```
2. Navigate into the project directory and install the required dependencies using: 
```
npm install
```
3. Once the dependencies are installed, you can start the development server using:
```
npm run serve
```
This will start the Vue development server and the app will be available at:
    http://localhost:8080


### Compiles and minifies for production
```
npm run build
```

