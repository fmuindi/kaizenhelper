<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Kaizen Helper Chat</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<style>
  :root {
    --primary: #4361ee;
    --primary-dark: #3a56d4;
    --primary-light: #eef2ff;
    --secondary: #7209b7;
    --dark: #1e293b;
    --light: #f8fafc;
    --gray: #64748b;
    --gray-light: #e2e8f0;
    --success: #10b981;
    --danger: #ef4444;
    --user-msg: #4361ee;
    --bot-msg: #f1f5f9;
    --shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.05);
    --radius: 16px;
    --radius-sm: 12px;
    --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  }

  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body {
    font-family: "Inter", sans-serif;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    margin: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    padding: 20px;
    color: var(--dark);
  }

  .chat-container {
    background: white;
    width: 100%;
    max-width: 440px;
    height: 700px;
    display: flex;
    flex-direction: column;
    border-radius: var(--radius);
    box-shadow: var(--shadow);
    overflow: hidden;
    position: relative;
  }

  .chat-header {
    background: linear-gradient(135deg, var(--primary), var(--secondary));
    color: white;
    padding: 20px;
    text-align: center;
    font-weight: 600;
    letter-spacing: 0.5px;
    font-size: 18px;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
    position: relative;
    z-index: 10;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  }

  .chat-header::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 4px;
    background: linear-gradient(90deg, #10b981, #3b82f6, #8b5cf6);
    z-index: 11;
  }

  .header-icon {
    width: 24px;
    height: 24px;
    filter: brightness(0) invert(1);
  }

  .chat-messages {
    flex: 1;
    padding: 20px;
    overflow-y: auto;
    background: var(--light);
    scroll-behavior: smooth;
    display: flex;
    flex-direction: column;
    gap: 16px;
  }

  .message-wrapper {
    display: flex;
    align-items: flex-end;
    animation: fadeIn 0.4s ease;
    max-width: 85%;
  }

  .user-wrapper {
    align-self: flex-end;
  }

  .bot-wrapper {
    align-self: flex-start;
  }

  .message {
    max-width: 100%;
    padding: 14px 18px;
    border-radius: var(--radius-sm);
    line-height: 1.5;
    word-wrap: break-word;
    position: relative;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
    transition: var(--transition);
  }

  .user-message {
    background: var(--user-msg);
    color: white;
    border-bottom-right-radius: 4px;
    box-shadow: 0 4px 12px rgba(67, 97, 238, 0.2);
  }

  .bot-message {
    background: white;
    color: var(--dark);
    border-bottom-left-radius: 4px;
    border: 1px solid var(--gray-light);
  }

  .avatar {
    width: 36px;
    height: 36px;
    border-radius: 50%;
    margin: 0 10px;
    object-fit: cover;
    border: 2px solid white;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  }

  .user-avatar {
    order: 2;
  }

  .bot-avatar {
    order: 0;
  }

  .timestamp {
    font-size: 11px;
    color: var(--gray);
    margin-top: 5px;
    text-align: right;
    font-weight: 400;
  }

  .bot-timestamp {
    text-align: left;
  }

  .chat-input-container {
    display: flex;
    border-top: 1px solid var(--gray-light);
    background: white;
    padding: 16px;
    gap: 12px;
    align-items: center;
  }

  .chat-input {
    flex: 1;
    border: 1px solid var(--gray-light);
    padding: 14px 18px;
    font-size: 15px;
    outline: none;
    border-radius: 50px;
    background: var(--light);
    transition: var(--transition);
  }

  .chat-input:focus {
    border-color: var(--primary);
    box-shadow: 0 0 0 3px rgba(67, 97, 238, 0.1);
  }

  .send-btn {
    background: var(--primary);
    color: white;
    border: none;
    width: 48px;
    height: 48px;
    border-radius: 50%;
    cursor: pointer;
    font-weight: 500;
    transition: var(--transition);
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 4px 12px rgba(67, 97, 238, 0.3);
  }

  .send-btn:hover {
    background: var(--primary-dark);
    transform: translateY(-2px);
    box-shadow: 0 6px 16px rgba(67, 97, 238, 0.4);
  }

  .send-btn:active {
    transform: translateY(0);
  }

  .send-icon {
    width: 20px;
    height: 20px;
    filter: brightness(0) invert(1);
  }

  .typing-indicator {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 12px 18px;
    background: white;
    border-radius: var(--radius-sm);
    border: 1px solid var(--gray-light);
    align-self: flex-start;
    max-width: 140px;
    margin-bottom: 5px;
  }

  .typing-text {
    color: var(--gray);
    font-size: 14px;
    font-weight: 500;
  }

  .typing-dots {
    display: flex;
    gap: 4px;
  }

  .typing-dot {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background: var(--gray);
    animation: typingAnimation 1.4s infinite ease-in-out;
  }

  .typing-dot:nth-child(1) {
    animation-delay: -0.32s;
  }

  .typing-dot:nth-child(2) {
    animation-delay: -0.16s;
  }

  @keyframes fadeIn {
    from {opacity: 0; transform: translateY(10px);}
    to {opacity: 1; transform: translateY(0);}
  }

  @keyframes typingAnimation {
    0%, 80%, 100% { 
      transform: scale(0.8);
      opacity: 0.5;
    }
    40% { 
      transform: scale(1);
      opacity: 1;
    }
  }

  /* Scrollbar styling */
  .chat-messages::-webkit-scrollbar {
    width: 6px;
  }

  .chat-messages::-webkit-scrollbar-track {
    background: transparent;
  }

  .chat-messages::-webkit-scrollbar-thumb {
    background: var(--gray-light);
    border-radius: 10px;
  }

  .chat-messages::-webkit-scrollbar-thumb:hover {
    background: var(--gray);
  }

  /* Welcome message */
  .welcome-message {
    text-align: center;
    padding: 20px;
    color: var(--gray);
    font-size: 14px;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 10px;
  }

  .welcome-icon {
    width: 60px;
    height: 60px;
    opacity: 0.7;
    margin-bottom: 10px;
  }

  /* Responsive adjustments */
  @media (max-width: 480px) {
    .chat-container {
      height: 100vh;
      max-height: -webkit-fill-available;
      border-radius: 0;
    }
    
    body {
      padding: 0;
    }
    
    .message-wrapper {
      max-width: 90%;
    }
  }
</style>
</head>
<body>
  <div class="chat-container">
    <div class="chat-header">
      <svg class="header-icon" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M20 2H4C2.9 2 2 2.9 2 4V22L6 18H20C21.1 18 22 17.1 22 16V4C22 2.9 21.1 2 20 2ZM20 16H5.17L4 17.17V4H20V16ZM7 9H17V11H7V9ZM7 12H17V14H7V12ZM7 6H17V8H7V6Z" fill="white"/>
      </svg>
      Kaizen Helper
    </div>
    <div id="chatMessages" class="chat-messages">
      <div class="welcome-message">
        <svg class="welcome-icon" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M12 2C6.48 2 2 6.48 2 12C2 17.52 6.48 22 12 22C17.52 22 22 17.52 22 12C22 6.48 17.52 2 12 2ZM12 20C7.59 20 4 16.41 4 12C4 7.59 7.59 4 12 4C16.41 4 20 7.59 20 12C20 16.41 16.41 20 12 20ZM16.59 7.58L10 14.17L7.41 11.59L6 13L10 17L18 9L16.59 7.58Z" fill="#4361ee"/>
        </svg>
        <div>Welcome to Kaizen Helper! How can I assist you today?</div>
      </div>
    </div>
    <div class="chat-input-container">
      <input id="userInput" type="text" class="chat-input" placeholder="Type your message..." />
      <button id="sendBtn" class="send-btn">
        <svg class="send-icon" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M2.01 21L23 12 2.01 3 2 10L17 12 2 14V21Z" fill="white"/>
        </svg>
      </button>
    </div>
  </div>

<script>
  const chatMessages = document.getElementById("chatMessages");
  const userInput = document.getElementById("userInput");
  const sendBtn = document.getElementById("sendBtn");
  const webhookURL = "https://tools.kaizenhelper.com/webhook/77771a5f-5ccb-4a83-966f-0a7b00e6732a/chat";

  const botAvatar = "https://cdn-icons-png.flaticon.com/512/4712/4712109.png";
  const userAvatar = "https://cdn-icons-png.flaticon.com/512/1077/1077012.png";

  function getTimestamp() {
    const now = new Date();
    return now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
  }

  function appendMessage(text, sender) {
    const wrapper = document.createElement("div");
    wrapper.classList.add("message-wrapper", sender === "user" ? "user-wrapper" : "bot-wrapper");

    const avatar = document.createElement("img");
    avatar.src = sender === "user" ? userAvatar : botAvatar;
    avatar.classList.add("avatar", sender === "user" ? "user-avatar" : "bot-avatar");

    const messageDiv = document.createElement("div");
    messageDiv.classList.add("message", sender === "user" ? "user-message" : "bot-message");
    messageDiv.textContent = text;

    const timestamp = document.createElement("div");
    timestamp.classList.add("timestamp", sender === "bot" ? "bot-timestamp" : "");
    timestamp.textContent = getTimestamp();

    if (sender === "user") {
      wrapper.appendChild(messageDiv);
      wrapper.appendChild(avatar);
    } else {
      wrapper.appendChild(avatar);
      wrapper.appendChild(messageDiv);
    }

    chatMessages.appendChild(wrapper);
    chatMessages.appendChild(timestamp);
    chatMessages.scrollTop = chatMessages.scrollHeight;
  }

  function showTypingIndicator() {
    const typingIndicator = document.createElement("div");
    typingIndicator.classList.add("typing-indicator");
    typingIndicator.id = "typingIndicator";
    
    const dotsContainer = document.createElement("div");
    dotsContainer.classList.add("typing-dots");
    
    for (let i = 0; i < 3; i++) {
      const dot = document.createElement("div");
      dot.classList.add("typing-dot");
      dotsContainer.appendChild(dot);
    }
    
    const typingText = document.createElement("div");
    typingText.classList.add("typing-text");
    typingText.textContent = "Kaizen is typing";
    
    typingIndicator.appendChild(typingText);
    typingIndicator.appendChild(dotsContainer);
    
    chatMessages.appendChild(typingIndicator);
    chatMessages.scrollTop = chatMessages.scrollHeight;
    
    return typingIndicator;
  }

  async function sendMessage() {
    const message = userInput.value.trim();
    if (!message) return;

    appendMessage(message, "user");
    userInput.value = "";

    const typingIndicator = showTypingIndicator();

    try {
      const res = await fetch(webhookURL, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ message })
      });
      const data = await res.json();
      
      if (typingIndicator && typingIndicator.parentNode) {
        chatMessages.removeChild(typingIndicator);
      }

      const reply = data.response || "Sorry, I couldn't get a response.";
      appendMessage(reply, "bot");
    } catch (err) {
      if (typingIndicator && typingIndicator.parentNode) {
        chatMessages.removeChild(typingIndicator);
      }
      appendMessage("⚠️ Error connecting to server. Please try again.", "bot");
    }
  }

  sendBtn.addEventListener("click", sendMessage);
  userInput.addEventListener("keypress", (e) => {
    if (e.key === "Enter") sendMessage();
  });
  
  // Focus on input when page loads
  window.addEventListener('load', () => {
    userInput.focus();
  });
</script>
</body>
</html>
