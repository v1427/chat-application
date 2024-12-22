<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Professional Website with Chatbot</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f0f4f8;
        }

        /* Navbar styling */
        .navbar {
            background-color: #673ab7;
            color: white;
            padding: 10px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .navbar h1 {
            margin: 0;
            font-size: 1.5em;
        }

        .navbar a {
            color: white;
            text-decoration: none;
            margin-left: 20px;
            font-size: 1em;
        }

        /* Main content styling */
        .content {
            padding: 20px;
        }

        .content h2 {
            font-size: 2em;
            color: #333;
        }

        .content p {
            font-size: 1.2em;
            color: #555;
            line-height: 1.6;
        }

        /* Footer styling */
        .footer {
            background-color: #673ab7;
            color: white;
            text-align: center;
            padding: 20px 0;
            position: fixed;
            width: 100%;
            bottom: 0;
        }

        /* Chatbot widget styles */
        .chat-widget {
            position: fixed;
            bottom: 100px;
            right: 20px;
            width: 300px;
            display: none;
            flex-direction: column;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            background-color: #fff;
            overflow: hidden;
        }

        .chat-widget-header {
            background-color: #673ab7;
            color: white;
            padding: 10px;
            text-align: center;
            cursor: pointer;
        }

        .chat-widget-body {
            max-height: 300px;
            padding: 10px;
            overflow-y: auto;
        }

        .chat-widget-input {
            display: flex;
            padding: 10px;
            border-top: 1px solid #ddd;
        }

        .chat-widget-input input {
            flex: 1;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 20px;
            margin-right: 10px;
        }

        .chat-widget-input button {
            background-color: #673ab7;
            color: white;
            border: none;
            padding: 10px;
            border-radius: 20px;
            cursor: pointer;
        }

        .chat-message {
            padding: 10px;
            margin: 5px 0;
            border-radius: 20px;
            max-width: 80%;
            word-wrap: break-word;
        }

        .chat-message.user {
            background-color: #2196f3;
            color: white;
            align-self: flex-end;
        }

        .chat-message.bot {
            background-color: #fff59d;
            color: #333;
            align-self: flex-start;
        }

        /* Chat notification button */
        .chat-notification {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background-color: #673ab7;
            color: white;
            padding: 10px 15px;
            border-radius: 30px;
            cursor: pointer;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }

        /* Responsive design */
        @media (max-width: 768px) {
            .chat-widget {
                width: 90%;
                right: 5%;
            }
        }
    </style>
</head>
<body>

    <!-- Navbar -->
    <div class="navbar">
        <h1>Sri Krishna arts and science College Website</h1>
        <div>
            <a href="#">Home</a>
            <a href="#">Services</a>
            <a href="#">Contact</a>
        </div>
    </div>

    <!-- Main content -->
    <div class="content">
        <h2>Welcome to Our Website</h2>
        <p>We offer a variety of services to help you grow your business and reach your goals. Our team of experts is here to assist you with personalized solutions that fit your needs.</p>
        <p>Feel free to explore our website and let us know how we can assist you. If you need help, just click on the chat icon below to start a conversation with our chatbot!</p>
    </div>

    <!-- Footer -->
    <div class="footer">
        &copy; 2024 My Professional Website. All rights reserved.
    </div>

    <!-- Chat notification button -->
    <div class="chat-notification" id="chat-notification">
        Chat with us!
    </div>

    <!-- Chat widget -->
    <div class="chat-widget" id="chat-widget">
        <div class="chat-widget-header" id="chat-header">
            Chat with us
            <button id="chat-close" style="float: right; background: none; border: none; color: white; cursor: pointer;">X</button>
        </div>
        <div class="chat-widget-body" id="chat-body">
            <!-- Messages will appear here -->
        </div>
        <div class="chat-widget-input">
            <input type="text" id="chat-input" placeholder="Type your message...">
            <button id="chat-send">Send</button>
        </div>
    </div>

    <script>
        document.addEventListener("DOMContentLoaded", function () {
            const chatNotification = document.getElementById('chat-notification');
            const chatWidget = document.getElementById('chat-widget');
            const chatBody = document.getElementById('chat-body');
            const chatInput = document.getElementById('chat-input');
            const chatSend = document.getElementById('chat-send');
            const chatClose = document.getElementById('chat-close');

            let messageQueue = [];

            // Function to display chat messages
            function displayMessages() {
                chatBody.innerHTML = '';
                messageQueue.forEach(function (message) {
                    const messageDiv = document.createElement('div');
                    messageDiv.classList.add('chat-message', message.type);
                    messageDiv.textContent = message.text;
                    chatBody.appendChild(messageDiv);
                });
                chatBody.scrollTop = chatBody.scrollHeight;
            }

            // Function to handle bot responses
            function getBotResponse(userMessage) {
                const lowerMessage = userMessage.toLowerCase();
                if (lowerMessage.includes('hello')) {
                    return "Hello! How can I assist you today?";
                } else if (lowerMessage.includes('help')) {
                    return "Sure! Let me know your question.";
                } else if (lowerMessage.includes('inquire about the application deadlines at sri krishna arts and science college')) {
                    return "The deadlines vary by program and institution. Common deadlines include early decision (usually in November).";
                } else if (lowerMessage.includes('thank you') || lowerMessage.includes('goodbye') || lowerMessage.includes('exit')) {
                    return "Thank you for chatting! Have a great day!";
                } else {
                    return "I'm not sure how to respond to that. Please ask something else.";
                }
            }

            // Function to send a message
            function sendMessage() {
                const userMessage = chatInput.value.trim();
                if (userMessage) {
                    messageQueue.push({ text: userMessage, type: 'user' });
                    displayMessages();

                    setTimeout(function () {
                        const botResponse = getBotResponse(userMessage);
                        messageQueue.push({ text: botResponse, type: 'bot' });
                        displayMessages();

                        // Hide the chat widget if the user says goodbye or thank you
                        if (botResponse === "Thank you for chatting! Have a great day!") {
                            setTimeout(() => {
                                chatWidget.style.display = 'none';
                                chatNotification.style.display = 'block'; // Show notification button again
                            }, 2000);
                        }
                    }, 1000);

                    chatInput.value = '';
                }
            }

            // Send message on button click
            chatSend.addEventListener('click', sendMessage);

            // Send message on Enter key press
            chatInput.addEventListener('keypress', function (e) {
                if (e.key === 'Enter') {
                    sendMessage();
                }
            });

            // Show chat widget when the notification button is clicked
            chatNotification.addEventListener('click', function () {
                chatNotification.style.display = 'none';
                chatWidget.style.display = 'flex';
                
                // Bot greets the user automatically when chat opens
                setTimeout(function () {
                    messageQueue.push({ text: "Hello! How can I help you today?", type: 'bot' });
                    displayMessages();
                }, 500);
            });

            // Close chat widget when close button is clicked
            chatClose.addEventListener('click', function () {
                chatWidget.style.display = 'none'; // Hide chat widget
                messageQueue = []; // Clear chat history
                displayMessages(); // Refresh display (will show empty chat)
                chatNotification.style.display = 'block'; // Show notification button again
            });
        });
    </script>

</body>
</html># chat-application
