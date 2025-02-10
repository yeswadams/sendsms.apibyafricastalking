# Sending SMS with Africa's Talking API
This project is a Node.js application that demonstrates how to send SMS messages using the Africa's Talking API. It also sets up a simple SMS server to handle incoming messages via a webhook.

Features
Send SMS: Uses Africa's Talking API to send SMS messages.
Receive SMS: Sets up an endpoint to receive incoming SMS messages.
Modern JavaScript: Utilizes ES6 features like arrow functions and async/await.
Prerequisites
Node.js (v12 or higher recommended)
npm (comes with Node.js) or Yarn
An Africa's Talking account with API credentials (sandbox or production)
Installation
Clone the repository:

bash
Copy
Edit
git clone <repository-url>
cd <repository-folder>
Install dependencies:

bash
Copy
Edit
npm install
Configuration
For demonstration purposes, the API key and username are hard-coded in the sendSMS.js file. For production, it's best to store sensitive information like these in environment variables.

To use environment variables, follow these steps:

Install dotenv:

bash
Copy
Edit
npm install dotenv
Create a .env file in the root directory of your project and add your credentials:

dotenv
Copy
Edit
AT_API_KEY=your_api_key_here
AT_USERNAME=your_username_here
Update sendSMS.js to use the environment variables:

js
Copy
Edit
require('dotenv').config();
const AfricasTalking = require('africastalking');

const africastalking = AfricasTalking({
    apikey: process.env.AT_API_KEY,
    username: process.env.AT_USERNAME,
});

module.exports = async function sendSMS() {
    try {
        const result = await africastalking.SMS.send({
            to: '0740803075', // Adjust the phone number format if necessary
            message: 'Hey AT Ninja! Wassup...',
            from: '6741'      // Ensure this is your valid sender ID/shortcode
        });
        console.log(result);
    } catch (ex) {
        console.error(ex);
    }
};
Usage
Running the Application
The application consists of three main files:

sendSMS.js: Contains the logic to send an SMS message.
smsServer.js: Sets up an Express server to listen for incoming SMS messages.
index.js: The entry point that calls both the SMS sending function and starts the SMS server.
To start the application, run:

```bash
index.js

const sendSMS = require('./sendSMS');
const smsServer = require('./smsServer');

// Call the sendSMS function to send an SMS
sendSMS();

// Start the SMS server to listen for incoming messages
smsServer();
smsServer.js

```
Sets up a POST route for incoming messages:

```bash
const express = require('express');
const app = express();
app.use(express.json());

// Incoming messages route
app.post('/incoming-messages', (req, res) => {
    const data = req.body;
    console.log(`Received message \n ${JSON.stringify(data, null, 2)}`);
    res.sendStatus(200);
});

module.exports = function smsServer() {
    const PORT = process.env.PORT || 3000;
    app.listen(PORT, () => {
        console.log(`SMS Server is listening on port ${PORT}`);
    });
};
sendSMS.js
Contains the logic to send an SMS using Africa's Talking API (see above).

```


Contact
For any questions or feedback, please open an issue in the repository or reach out directly at yeswaadams73@gmail.com.
