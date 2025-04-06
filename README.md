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
