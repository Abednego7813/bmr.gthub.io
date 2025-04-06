<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Payment Request</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>Request a Payment</h1>
    <form id="paymentRequestForm">
        <label for="amount">Amount:</label>
        <input type="number" id="amount" name="amount" required>
        
        <label for="service">Service Description:</label>
        <textarea id="service" name="service" required></textarea>
        
        <button type="submit">Request Payment</button>
    </form>

    <script src="script.js"></script>
</body>
</html>
/* style.css */
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #f4f4f4;
}

form {
    background: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    width: 300px;
}

h1 {
    text-align: center;
}

label {
    display: block;
    margin-bottom: 5px;
}

input, textarea, button {
    width: 100%;
    padding: 10px;
    margin-bottom: 15px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

button {
    background-color: #4CAF50;
    color: white;
    border: none;
    cursor: pointer;
}

button:hover {
    background-color: #45a049;
}
// script.js
document.getElementById('paymentRequestForm').addEventListener('submit', function(event) {
    event.preventDefault();
    
    const amount = document.getElementById('amount').value;
    const service = document.getElementById('service').value;

    // Send payment request to the backend
    fetch('/api/request-payment', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify({ amount, service }),
    })
    .then(response => response.json())
    .then(data => {
        alert("Payment request sent!");
        // Optionally handle push notifications here
    })
    .catch(error => console.error('Error:', error));
});
// server.js (Node.js with Express)
const express = require('express');
const bodyParser = require('body-parser');
const nodemailer = require('nodemailer'); // For sending email notifications (or use Firebase for push notifications)

const app = express();
const port = 3000;

app.use(bodyParser.json());

// Simple endpoint to handle payment requests
app.post('/api/request-payment', (req, res) => {
    const { amount, service } = req.body;

    // Logic to process payment via Stripe or PayPal (example, you should integrate real payment APIs here)
    console.log(`Received payment request: ${amount} for ${service}`);

    // Send a notification (example: via email or push)
    sendNotification(amount, service);

    res.json({ message: 'Payment request received!' });
});

// Function to send email notifications (you can replace with Firebase push notifications)
function sendNotification(amount, service) {
    let transporter = nodemailer.createTransport({
        service: 'gmail',
        auth: {
            user: 'your-email@gmail.com',
            pass: 'your-email-password',
        },
    });

    let mailOptions = {
        from: 'your-email@gmail.com',
        to: 'client-email@example.com',
        subject: 'Payment Request Notification',
        text: `You have a payment request of $${amount} for the following service: ${service}`,
    };

    transporter.sendMail(mailOptions, (error, info) => {
        if (error) {
            console.log('Error sending email:', error);
        } else {
            console.log('Email sent: ' + info.response);
        }
    });
}

app.listen(port, () => {
    console.log(`Server running on http://localhost:${port}`);
});
