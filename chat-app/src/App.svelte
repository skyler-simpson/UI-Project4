<script>
  class communication{
    constructor(m, p){
      this.m = m;
      this.p = p;
    }
  }

  let subjects = ["What's your passion?", "Dream travel destination?", "Favorite childhood memory?", "Biggest recent achievement?", "Any hidden talents?", "Go-to comfort food?", "What motivates you?", "Best book you have read?", "Bucket list item?", "Most memorable trip?"];
  let responses = ["Hi", "Hello", "That's really cool", "Wow", "I like trains", "Can you explain more", "Have a good day", "Nice talking to you"];

  let message = "";
  let messages = [];

  let timeLeft = 300;

  const sendMessage = (byWho, ) => {
    if (message.trim() !== "" && timeLeft > 0) {
      messages = [...messages, new communication(message, byWho)];
      message = "";
      if(byWho == "user"){
      setTimeout(() => {
        messages = [...messages, new communication(responses[Math.floor(Math.random() * responses.length)], "response")]
      }, 1000 * (Math.floor(Math.random() * 3)) + 100);
    }
    }
  };

  let timer;

  let connect = () => {
    timeLeft = 300;
    messages = [];

    const button = document.getElementById("con-text");
    button.classList.add("blinking");
    button.innerText="Connecting";

    clearInterval(timer);

    setTimeout(() => {
      button.classList.remove("blinking");
      button.innerText = "Connected";
      setTimeout(() => {
        button.innerText = "Reconnect";
      timer = setInterval(() => {
        if (timeLeft <= 0) {
            clearInterval(timer);
            document.getElementById("timer").innerText = "Time's up!";

            return;
          }
          
        const minutes = Math.floor(timeLeft / 60);
        const seconds = timeLeft % 60;
        document.getElementById("timer").innerText = `${minutes}:${seconds < 10 ? '0' + seconds : seconds}`;
        timeLeft--;
        }, 1000);
      }, 1500);
    }, 4000);
    document.getElementById("subject").innerText = subjects[Math.floor(Math.random() * subjects.length)]
  }
</script>

<style>
  
  .message-board {
    width: 100%;
    max-width: 600px;
    margin: auto;
    font-family: Arial, sans-serif;
  }

  .messages {
    border: 1px solid #ccc;
    padding: 10px;
    margin-bottom: 10px;
    height: 300px;
    overflow-y: scroll;
    background: #f9f9f9;
  }

  .message-item {
    margin-bottom: 5px;
    padding: 5px;
    border-bottom: 1px solid #ddd;
  }

  .user {
    color: blue;
    text-align: right;
  }

  .response {
    color: red;
    text-align: left;
  }

  .input-container {
    display: flex;
    gap: 10px;
  }

  input[type="text"] {
    flex: 1;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

  button {
    padding: 10px 20px;
    border: none;
    background-color: #007bff;
    color: white;
    border-radius: 4px;
    cursor: pointer;
  }

  button:hover {
    background-color: #0056b3;
  }

  .modifiers {
    padding: 10px;
    display: flex;
    justify-self: center;
  }

  .insides {
    padding: 10px 30px 10px;
  }

  .blinking {
    animation: blink 1s infinite alternate;
  }

  @keyframes blink {
    from { opacity: 1; }
    to { opacity: 0; }
  }

  .ice-breaker {
    font-size: large;
    font-weight: 500;
  }
</style>

<h1 style="color:green;">Welcome to 5 Minute Mingle</h1>
<h2 style="color:#F28500;">You have 5 minutes to talk to someone. Once time is up, reconnect to do it again</h2>

<div class="modifiers">
  <div id="timer" class="insides" style="font-size:larger; font-weight: 400;">
    5:00
  </div>
  <button id="con" class="insides" on:click={connect}>
    <span id="con-text">Connect</span>
  </button>
</div>

<div class="ice-breaker">
  <p style="color:purple">If you're stuck try, talking about this:</p>
  <p id="subject" style="color:goldenrod;">{subjects[Math.floor(Math.random() * subjects.length)]}</p>
</div>

<div class="message-board">
  <div class="messages">
    {#each messages as msg}
      <div class="message-item {msg.p}">{msg.m}</div>
    {/each}
  </div>

  <div class="input-container">
    <input id = "enter" type="text" bind:value={message} placeholder="Type your message here..." on:keydown={(e) => e.key === 'Enter' && sendMessage("user")} 
    />
    <button id = "words" on:click={() => sendMessage("user")}>Send</button>
  </div>
</div>
