<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Pokémon Viewer</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
    }

    ul {
      list-style-type: none;
      padding: 0;
    }

    li {
      margin-bottom: 10px;
      cursor: pointer;
    }

    input {
      margin-bottom: 10px;
    }

    #pokemonDetails {
      display: none;
    }
  </style>
</head>
<body>

  <h1>Pokémon Viewer</h1>

  <input type="text" id="searchInput" placeholder="Search Pokémon by name">

  <ul id="pokemonList"></ul>

  <div id="pokemonDetails">
    <h2 id="pokemonName"></h2>
    <img id="pokemonSprite" alt="Pokemon Sprite">
    <p id="pokemonTypes"></p>
  </div>

  <script>
    document.addEventListener('DOMContentLoaded', () => {
      const pokemonList = document.getElementById('pokemonList');
      const searchInput = document.getElementById('searchInput');
      const pokemonDetails = document.getElementById('pokemonDetails');
      const pokemonName = document.getElementById('pokemonName');
      const pokemonSprite = document.getElementById('pokemonSprite');
      const pokemonTypes = document.getElementById('pokemonTypes');

      // Fetching Pokémon list from the PokeAPI
      fetch('https://pokeapi.co/api/v2/pokemon?limit=10')
        .then(response => response.json())
        .then(data => {
          data.results.forEach(pokemon => {
            const listItem = document.createElement('li');
            listItem.textContent = pokemon.name;
            listItem.addEventListener('click', () => showPokemonDetails(pokemon.url));
            pokemonList.appendChild(listItem);
          });
        });

      // Function to fetch and display Pokémon details
      function showPokemonDetails(url) {
        fetch(url)
          .then(response => response.json())
          .then(data => {
            pokemonName.textContent = data.name;
            pokemonSprite.src = data.sprites.front_default;
            pokemonTypes.textContent = `Type(s): ${data.types.map(type => type.type.name).join(', ')}`;
            pokemonDetails.style.display = 'block';
          });
      }

      // Search functionality
      searchInput.addEventListener('input', () => {
        const searchTerm = searchInput.value.toLowerCase();
        const listItems = Array.from(pokemonList.children);

        listItems.forEach(item => {
          const pokemonName = item.textContent.toLowerCase();
          if (pokemonName.includes(searchTerm)) {
            item.style.display = 'block';
          } else {
            item.style.display = 'none';
          }
        });
      });
    });
  </script>

</body>
</html>
