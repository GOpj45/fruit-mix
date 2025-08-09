# fruit-mix
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Random Cat Generator</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f4f4;
      margin: 0;
      padding: 0;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      text-align: center;
    }
    h1 {
      margin-bottom: 10px;
    }
    img {
      max-width: 90%;
      max-height: 60vh;
      border-radius: 10px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.2);
    }
    #cat-name {
      font-size: 1.5em;
      margin: 15px 0;
      color: #333;
    }
    button {
      background: #ff9800;
      color: white;
      border: none;
      padding: 10px 20px;
      font-size: 1em;
      border-radius: 5px;
      cursor: pointer;
    }
    button:disabled {
      background: #d7a64f;
      cursor: default;
    }
    button:hover:enabled {
      background: #e68900;
    }
  </style>
</head>
<body>
  <h1>🐱 Random Cat Generator</h1>
  <img id="cat-image" src="" alt="Random Cat" />
  <div id="cat-name">Loading...</div>
  <button id="new-cat-btn" onclick="loadCat()">New Cat</button>

  <script>
    const names1 = ["Sir", "Lady", "Captain", "Professor", "Dr.", "Count", "Queen", "King", "Duchess", "Lord"];
    const names2 = ["Fluffy", "Whiskers", "Paws", "Meow", "Furball", "Snuggles", "Claw", "Purr", "Tuna", "Sprinkles"];
    const names3 = ["McFluff", "Pawsworth", "von Meow", "the Great", "Jr.", "Sr.", "III", "of Catshire", "the Sneaky", "the Magnificent"];

    function randomName() {
      const part1 = names1[Math.floor(Math.random() * names1.length)];
      const part2 = names2[Math.floor(Math.random() * names2.length)];
      const part3 = names3[Math.floor(Math.random() * names3.length)];
      return ${part1} ${part2} ${part3};
    }

    async function loadCat() {
      const btn = document.getElementById("new-cat-btn");
      const nameDiv = document.getElementById("cat-name");
      const img = document.getElementById("cat-image");

      btn.disabled = true;
      nameDiv.textContent = "Loading...";

      // Set up a timeout promise for 5 seconds
      const timeout = new Promise((_, reject) => {
        setTimeout(() => reject(new Error("Timeout")), 5000);
      });

      try {
        const fetchCat = fetch("https://api.thecatapi.com/v1/images/search").then(res => res.json());
        const data = await Promise.race([fetchCat, timeout]);

        img.src = data[0].url;
        img.alt = "Random Cat";

        // Wait for image to fully load or fail
        await new Promise((resolve, reject) => {
          img.onload = () => resolve();
          img.onerror = () => reject(new Error("Image failed to load"));
        });

        nameDiv.textContent = randomName();
      } catch (error) {
        nameDiv.textContent = "טעינה נכשלה, נסה שוב 😿";
        img.src = "";
        img.alt = "";
      } finally {
        btn.disabled = false;
      }
    }

    loadCat();
  </script>
</body>
</html>
