<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>User Data Fetcher</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f4f4;
      padding: 20px;
    }
    h1 {
      text-align: center;
    }
    #userContainer {
      display: flex;
      flex-wrap: wrap;
      gap: 15px;
      justify-content: center;
    }
    .userCard {
      background: white;
      padding: 15px;
      border-radius: 8px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
      width: 300px;
    }
    .error {
      color: red;
      text-align: center;
      margin-top: 20px;
    }
    #reloadBtn {
      display: block;
      margin: 20px auto;
      padding: 10px 20px;
      background-color: #007BFF;
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
    }
    #reloadBtn:hover {
      background-color: #0056b3;
    }
  </style>
</head>
<body>

  <h1>User Data</h1>
  <button id="reloadBtn">Reload Data</button>
  <div id="userContainer"></div>
  <div id="errorMessage" class="error"></div>

  <script>
    const container = document.getElementById("userContainer");
    const errorDiv = document.getElementById("errorMessage");
    const reloadBtn = document.getElementById("reloadBtn");

    async function fetchUsers() {
      container.innerHTML = ""; // Clear previous data
      errorDiv.textContent = ""; // Clear previous error
      try {
        const res = await fetch("https://jsonplaceholder.typicode.com/users");
        if (!res.ok) throw new Error("Failed to fetch users");

        const users = await res.json();

        users.forEach(user => {
          const card = document.createElement("div");
          card.className = "userCard";
          card.innerHTML = `
            <h3>${user.name}</h3>
            <p><strong>Email:</strong> ${user.email}</p>
            <p><strong>Address:</strong> ${user.address.street}, ${user.address.city}</p>
          `;
          container.appendChild(card);
        });
      } catch (error) {
        errorDiv.textContent = `Error: ${error.message}. Please check your internet connection.`;
      }
    }

    // Initial fetch
    fetchUsers();

    // Reload button handler
    reloadBtn.addEventListener("click", fetchUsers);
  </script>

</body>
</html>
