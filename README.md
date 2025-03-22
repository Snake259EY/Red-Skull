<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Chat Red Skull</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #111;
      color: #fff;
      text-align: center;
      margin: 0;
      padding: 0;
    }
    .container {
      width: 350px;
      margin: 60px auto 20px;
      padding: 20px;
      background-color: #222;
      border-radius: 10px;
      box-shadow: 0 0 15px red;
    }
    h1 {
      animation: glow 1.5s infinite alternate;
    }
    @keyframes glow {
      from { text-shadow: 0 0 10px red; }
      to { text-shadow: 0 0 20px red; }
    }
    input, button {
      width: 90%;
      padding: 10px;
      margin: 10px 0;
      border: none;
      border-radius: 5px;
    }
    input {
      background-color: #333;
      color: white;
    }
    button {
      background-color: red;
      color: white;
      cursor: pointer;
    }
    button:hover {
      background-color: darkred;
      transform: scale(1.05);
    }
    #chat-box {
      height: 250px;
      overflow-y: auto;
      background-color: #333;
      padding: 10px;
      margin-bottom: 10px;
      border-radius: 5px;
      text-align: left;
    }
    .message {
      background-color: #222;
      padding: 8px;
      border-radius: 5px;
      margin: 5px 0;
      display: flex;
      align-items: center;
      justify-content: space-between;
      position: relative;
    }
    .message .info {
      display: flex;
      align-items: center;
    }
    .message .info img {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      margin-right: 10px;
      border: 2px solid red;
    }
    /* Styles pour les différents statuts */
    .founder {
      background: linear-gradient(to right, gold, black);
      -webkit-background-clip: text;
      color: transparent;
      font-weight: bold;
    }
    .vip {
      color: red !important;
      text-shadow: 0 0 5px red;
      font-weight: bold;
    }
    .badge-founder {
      background-color: gold;
      color: black;
      padding: 3px 5px;
      border-radius: 5px;
      margin-left: 10px;
      font-size: 0.8em;
    }
    .badge-vip {
      background-color: red;
      color: white;
      padding: 3px 5px;
      border-radius: 5px;
      margin-left: 10px;
      font-size: 0.8em;
    }
    .delete-btn,
    .edit-btn {
      background-color: darkred;
      color: white;
      border: none;
      cursor: pointer;
      padding: 5px;
      border-radius: 3px;
      margin-left: 5px;
      font-size: 0.8em;
    }
    .edit-btn {
      background-color: #333;
    }
    .emoji {
      font-size: 1.5em;
    }
    /* Bouton Paramètres (crâne) */
    #settings-btn {
      position: fixed;
      top: 10px;
      right: 30px;
      font-size: 30px;
      background: transparent;
      border: none;
      color: white;
      cursor: pointer;
      display: none;
      z-index: 200;
    }
    #settings-btn:hover {
      color: red;
    }
    /* Bouton pour gérer les VIP (visible seulement pour le fondateur) */
    #manage-vip-btn {
      margin-top: 10px;
      background-color: #333;
      display: none;
    }
    /* Section Paramètres */
    #settings-container {
      display: none;
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: #333;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 15px red;
    }
    #settings-container input {
      margin: 10px 0;
    }
    .image-input {
      margin-top: 10px;
    }
    .context-menu {
      position: absolute;
      background-color: #333;
      border-radius: 5px;
      box-shadow: 0 0 10px red;
      color: white;
      padding: 10px;
      cursor: pointer;
      z-index: 100;
    }
    /* Formulaires */
    #auth {
      width: 350px;
      margin: 60px auto 20px;
      padding: 20px;
      background-color: #222;
      border-radius: 10px;
      box-shadow: 0 0 15px red;
      margin-bottom: 20px;
    }
    #auth form {
      display: none;
    }
    #auth form.active {
      display: block;
    }
    #auth button.switch {
      background-color: #333;
      margin-top: 10px;
    }
  </style>
</head>
<body onbeforeunload="return confirmExit();">
  <!-- Bouton Paramètres (Crâne) -->
  <button id="settings-btn" onclick="toggleSettings()">💀</button>

  <!-- Container des paramètres -->
  <div id="settings-container">
    <h2>Paramètres</h2>
    <form id="settings-form">
      <label for="newName">Nouveau pseudo :</label>
      <input type="text" id="newName" placeholder="Nouveau pseudo" />
      <br />
      <label for="newPassword">Nouveau mot de passe :</label>
      <input type="password" id="newPassword" placeholder="Nouveau mot de passe" />
      <br />
      <label for="newAvatar">Nouvelle photo de profil :</label>
      <input type="file" id="newAvatar" accept="image/*" class="image-input" />
      <br />
      <button type="button" onclick="saveSettings()">Enregistrer</button>
      <button type="button" onclick="toggleSettings()">Annuler</button>
    </form>
  </div>

  <!-- Authentification (connexion/inscription) -->
  <div id="auth">
    <!-- Formulaire de connexion -->
    <form id="loginForm" class="active">
      <h2>Connexion</h2>
      <input type="email" id="loginEmail" placeholder="Email" required />
      <input type="password" id="loginPassword" placeholder="Mot de passe" required />
      <button type="submit">Se connecter</button>
      <button type="button" class="switch" onclick="switchForm('registerForm')">S'inscrire</button>
    </form>
    <!-- Formulaire d'inscription -->
    <form id="registerForm">
      <h2>Inscription</h2>
      <input type="text" id="regName" placeholder="Pseudo" required />
      <input type="email" id="regEmail" placeholder="Email" required />
      <input type="password" id="regPassword" placeholder="Mot de passe" required />
      <input type="file" id="regAvatar" accept="image/*" class="image-input" />
      <button type="submit">S'inscrire</button>
      <button type="button" class="switch" onclick="switchForm('loginForm')">Se connecter</button>
    </form>
  </div>

  <!-- Chat -->
  <div class="container" id="chat" style="display:none;">
    <h1>
      Bienvenue <span id="username"></span>
      <span id="badge"></span>
    </h1>
    <div id="chat-box"></div>
    <input type="text" id="message" placeholder="Tapez un message..." />
    <br />
    <!-- Bouton pour envoyer une image -->
    <input type="file" id="imageMessage" accept="image/*" style="display:inline-block;width:auto;" />
    <button onclick="sendImageMessage()">Envoyer Image</button>
    <br />
    <!-- Bouton pour message vocal -->
    <button onclick="startVoiceRecognition()">🎤 Voice</button>
    <br />
    <button onclick="sendMessage()">Envoyer</button>
    <button onclick="logout()">Déconnexion</button>
    <!-- Bouton pour gérer les VIP (visible seulement pour le fondateur) -->
    <button id="manage-vip-btn" onclick="manageVIP()">Gérer VIP</button>
  </div>

  <script>
    const CREATOR_EMAIL = "bellonealessandro48@gmail.com";
    // Création automatique du compte créateur si inexistant
    if (!localStorage.getItem("user_" + CREATOR_EMAIL)) {
      const creator = {
        name: "Blood Scarlet",
        email: CREATOR_EMAIL,
        password: "Sarenza14."
      };
      localStorage.setItem("user_" + CREATOR_EMAIL, JSON.stringify(creator));
    }
    let currentUser = null;
    let longPressTimer;
    // Sauvegarde et restauration des messages
    function saveMessages() {
      const messages = document.getElementById("chat-box").innerHTML;
      localStorage.setItem("chatMessages", messages);
    }
    function loadMessages() {
      const messages = localStorage.getItem("chatMessages");
      if (messages) {
        document.getElementById("chat-box").innerHTML = messages;
        document.querySelectorAll(".message").forEach(message => {
          message.addEventListener("contextmenu", (e) => {
            e.preventDefault();
            showMessageContextMenu(e, message);
          });
          attachLongPress(message);
        });
      }
    }
    function confirmExit() {
      return "Êtes-vous sûr de vouloir quitter ?";
    }
    function toggleSettings() {
      const settingsContainer = document.getElementById("settings-container");
      settingsContainer.style.display = (settingsContainer.style.display === "none" || settingsContainer.style.display === "") ? "block" : "none";
    }
    function saveSettings() {
      const newName = document.getElementById("newName").value.trim();
      const newPassword = document.getElementById("newPassword").value.trim();
      const newAvatar = document.getElementById("newAvatar").files[0];
      if (newName) {
        currentUser.name = newName;
        document.getElementById("username").textContent = newName;
      }
      if (newPassword) {
        currentUser.password = newPassword;
        alert("Mot de passe changé !");
      }
      if (newAvatar) {
        const reader = new FileReader();
        reader.onload = function(event) {
          currentUser.avatar = event.target.result;
          alert("Avatar modifié !");
          document.querySelectorAll(".avatar").forEach(img => {
            img.src = currentUser.avatar;
          });
        };
        reader.readAsDataURL(newAvatar);
      }
      localStorage.setItem("user_" + currentUser.email, JSON.stringify(currentUser));
      toggleSettings();
    }
    // Bascule entre les formulaires
    function switchForm(formId) {
      document.querySelectorAll("#auth form").forEach(form => {
        form.classList.remove("active");
      });
      document.getElementById(formId).classList.add("active");
    }
    // Inscription
    document.getElementById("registerForm").addEventListener("submit", function(e) {
      e.preventDefault();
      const name = document.getElementById("regName").value.trim();
      const email = document.getElementById("regEmail").value.trim();
      const password = document.getElementById("regPassword").value;
      const avatarFile = document.getElementById("regAvatar").files[0];
      // Vérifier que le pseudo n'est pas déjà utilisé
      for (let key in localStorage) {
        if (key.startsWith("user_")) {
          const user = JSON.parse(localStorage.getItem(key));
          if (user.name.toLowerCase() === name.toLowerCase()) {
            alert("Ce pseudo est déjà utilisé.");
            return;
          }
        }
      }
      const newUser = { name, email, password };
      if (avatarFile) {
        const reader = new FileReader();
        reader.onload = function(event) {
          newUser.avatar = event.target.result;
          localStorage.setItem("user_" + email, JSON.stringify(newUser));
          alert("Inscription réussie !");
          switchForm("loginForm");
        };
        reader.readAsDataURL(avatarFile);
      } else {
        localStorage.setItem("user_" + email, JSON.stringify(newUser));
        alert("Inscription réussie !");
        switchForm("loginForm");
      }
    });
    // Connexion
    document.getElementById("loginForm").addEventListener("submit", function(e) {
      e.preventDefault();
      const email = document.getElementById("loginEmail").value.trim();
      const password = document.getElementById("loginPassword").value;
      const userData = localStorage.getItem("user_" + email);
      const user = userData ? JSON.parse(userData) : null;
      if (!user || user.password !== password) {
        alert("Identifiants incorrects.");
        return;
      }
      currentUser = user;
      // Afficher le bouton paramètres
      document.getElementById("settings-btn").style.display = "block";
      // Affichage des badges et style du pseudo
      const usernameSpan = document.getElementById("username");
      if (user.email === CREATOR_EMAIL) {
        usernameSpan.className = "founder";
        document.getElementById("badge").innerHTML = '<span class="badge-founder">Fondateur</span>';
        // Afficher le bouton pour gérer les VIP
        document.getElementById("manage-vip-btn").style.display = "block";
      } else if (user.vip) {
        usernameSpan.className = "vip";
        document.getElementById("badge").innerHTML = '<span class="badge-vip">VIP</span>';
      } else {
        usernameSpan.className = "";
        document.getElementById("badge").innerHTML = '';
      }
      usernameSpan.textContent = user.name;
      // Afficher l'avatar dans l'en-tête s'il existe
      if (user.avatar) {
        const avatarImg = document.createElement("img");
        avatarImg.src = user.avatar;
        avatarImg.className = "avatar";
        usernameSpan.parentNode.insertBefore(avatarImg, usernameSpan);
      }
      document.getElementById("chat").style.display = "block";
      document.getElementById("auth").style.display = "none";
      loadMessages();
    });
    // Envoi d'un message texte
    function sendMessage() {
      const messageText = document.getElementById("message").value.trim();
      if (messageText === "") return;
      addMessage({ type: "text", content: messageText });
      document.getElementById("message").value = "";
    }
    // Envoi d'un message image
    function sendImageMessage() {
      const fileInput = document.getElementById("imageMessage");
      if (!fileInput.files[0]) return;
      const file = fileInput.files[0];
      const reader = new FileReader();
      reader.onload = function(event) {
        addMessage({ type: "image", content: event.target.result });
      };
      reader.readAsDataURL(file);
      fileInput.value = "";
    }
    // Ajout d'un message dans le chat
    function addMessage(messageData) {
      const chatBox = document.getElementById("chat-box");
      const messageElement = document.createElement("div");
      messageElement.classList.add("message");
      messageElement.addEventListener("contextmenu", function(e) {
        e.preventDefault();
        showMessageContextMenu(e, messageElement);
      });
      attachLongPress(messageElement);
      const infoDiv = document.createElement("div");
      infoDiv.className = "info";
      if (currentUser.avatar) {
        const avatarImg = document.createElement("img");
        avatarImg.src = currentUser.avatar;
        avatarImg.className = "avatar";
        infoDiv.appendChild(avatarImg);
      }
      const nameSpan = document.createElement("span");
      if (currentUser.email === CREATOR_EMAIL) {
        nameSpan.className = "founder";
      } else if (currentUser.vip) {
        nameSpan.className = "vip";
      }
      nameSpan.textContent = currentUser.name;
      infoDiv.appendChild(nameSpan);
      const badgeSpan = document.createElement("span");
      badgeSpan.innerHTML = currentUser.email === CREATOR_EMAIL 
        ? '<span class="badge-founder">Fondateur</span>' 
        : (currentUser.vip ? '<span class="badge-vip">VIP</span>' : '');
      infoDiv.appendChild(badgeSpan);
      const contentDiv = document.createElement("div");
      if (messageData.type === "text") {
        contentDiv.innerHTML = `<p>${messageData.content}</p>`;
      } else if (messageData.type === "image") {
        const img = document.createElement("img");
        img.src = messageData.content;
        img.style.maxWidth = "100%";
        contentDiv.appendChild(img);
      }
      const timeDiv = document.createElement("div");
      timeDiv.innerHTML = `<small>${new Date().toLocaleTimeString()}</small>`;
      const btnContainer = document.createElement("div");
      const deleteBtn = document.createElement("button");
      deleteBtn.className = "delete-btn";
      deleteBtn.textContent = "X";
      deleteBtn.onclick = () => { messageElement.remove(); saveMessages(); };
      const editBtn = document.createElement("button");
      editBtn.className = "edit-btn";
      editBtn.textContent = "✏️";
      editBtn.onclick = () => { editMessage(messageElement); };
      btnContainer.appendChild(deleteBtn);
      btnContainer.appendChild(editBtn);
      messageElement.appendChild(infoDiv);
      messageElement.appendChild(contentDiv);
      messageElement.appendChild(timeDiv);
      messageElement.appendChild(btnContainer);
      chatBox.appendChild(messageElement);
      saveMessages();
    }
    // Menu contextuel
    function showMessageContextMenu(event, messageElement) {
      const existingMenu = document.querySelector(".context-menu");
      if (existingMenu) existingMenu.remove();
      const contextMenu = document.createElement("div");
      contextMenu.classList.add("context-menu");
      contextMenu.style.left = `${event.pageX}px`;
      contextMenu.style.top = `${event.pageY}px`;
      contextMenu.innerHTML = `
        <div onclick="editMessageFromContextMenu()">Modifier</div>
        <div onclick="deleteMessageFromContextMenu()">Supprimer</div>
      `;
      document.body.appendChild(contextMenu);
      contextMenu.dataset.targetId = messageElement.dataset.messageId || "";
      window.contextTargetMessage = messageElement;
      window.addEventListener("click", function removeMenu() {
        contextMenu.remove();
        window.removeEventListener("click", removeMenu);
      });
    }
    function editMessageFromContextMenu() {
      if (window.contextTargetMessage) {
        editMessage(window.contextTargetMessage);
      }
    }
    function deleteMessageFromContextMenu() {
      if (window.contextTargetMessage) {
        window.contextTargetMessage.remove();
        saveMessages();
      }
    }
    // Modification d'un message
    function editMessage(messageElement) {
      const currentContentElem = messageElement.querySelector("div:nth-child(2) p");
      if (!currentContentElem) return;
      const currentContent = currentContentElem.textContent;
      const newContent = prompt("Modifiez votre message :", currentContent);
      if (newContent) {
        currentContentElem.textContent = newContent;
        const timeDiv = messageElement.querySelector("div:nth-child(3) small");
        if(timeDiv) timeDiv.textContent = new Date().toLocaleTimeString();
        saveMessages();
      }
    }
    // Long press sur un message pour mobile
    function attachLongPress(messageElement) {
      messageElement.addEventListener("touchstart", function(e) {
        longPressTimer = setTimeout(() => {
          showMessageContextMenu(e, messageElement);
        }, 800);
      });
      messageElement.addEventListener("touchend", function(e) {
        clearTimeout(longPressTimer);
      });
    }
    // Fonction vocale
    function startVoiceRecognition() {
      if (!('webkitSpeechRecognition' in window) && !('SpeechRecognition' in window)) {
        alert("Votre navigateur ne supporte pas la reconnaissance vocale.");
        return;
      }
      const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
      const recognition = new SpeechRecognition();
      recognition.lang = "fr-FR";
      recognition.start();
      recognition.onresult = function(event) {
        const transcript = event.results[0][0].transcript;
        document.getElementById("message").value = transcript;
      };
      recognition.onerror = function() {
        alert("Une erreur est survenue lors de la reconnaissance vocale.");
      };
    }
    // Déconnexion avec confirmation
    function logout() {
      if (confirm("Êtes-vous sûr de vouloir vous déconnecter ?")) {
        document.getElementById("chat").style.display = "none";
        document.getElementById("auth").style.display = "block";
        document.getElementById("settings-btn").style.display = "none";
        localStorage.removeItem("chatMessages");
      }
    }
    // Fonction pour que le fondateur puisse choisir les VIP
    function manageVIP() {
      const email = prompt("Entrez l'email de l'utilisateur à basculer en VIP :");
      if (!email) return;
      const userData = localStorage.getItem("user_" + email.trim());
      if (!userData) {
        alert("Utilisateur non trouvé.");
        return;
      }
      let user = JSON.parse(userData);
      user.vip = !user.vip; // Inverse le statut VIP
      localStorage.setItem("user_" + email.trim(), JSON.stringify(user));
      alert("Le statut VIP de " + user.name + " est maintenant " + (user.vip ? "activé" : "désactivé") + ".");
    }
  </script>
</body>
</html>
