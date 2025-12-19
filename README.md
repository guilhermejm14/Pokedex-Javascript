# Pokedex-Javascript
Treinamento de Javascript DIO
const pokedexContainer = document.getElementById('pokedex-list');
const searchInput = document.getElementById('search-input');
const loading = document.getElementById('loading');

const POKEMON_COUNT = 151; // Primeira geração
let allPokemons = [];

// Mapeamento de cores por tipo
const colors = {
    fire: '#ff9d55', grass: '#63bb5b', water: '#5090d6', bug: '#91c12f',
    normal: '#919aa2', electric: '#f4d23c', poison: '#b567ce', ground: '#d97845',
    fairy: '#ec8fe6', fighting: '#ce4069', psychic: '#fa7179', rock: '#c5b679',
    ghost: '#5269ac', ice: '#74ced9', dragon: '#0b6dc3'
};

// 1. Função principal para buscar dados
async function fetchPokemons() {
    loading.style.display = 'block';
    
    for (let i = 1; i <= POKEMON_COUNT; i++) {
        const url = `https://pokeapi.co/api/v2/pokemon/${i}`;
        const res = await fetch(url);
        const data = await res.json();
        allPokemons.push(data);
    }
    
    displayPokemons(allPokemons);
    loading.style.display = 'none';
}

// 2. Função para renderizar os cards
function displayPokemons(pokemons) {
    pokedexContainer.innerHTML = '';
    
    pokemons.forEach(pokemon => {
        const type = pokemon.types[0].type.name;
        const color = colors[type] || '#777';
        
        const pokemonElement = document.createElement('div');
        pokemonElement.classList.add('pokemon-card');
        pokemonElement.style.borderTop = `8px solid ${color}`;
        
        pokemonElement.innerHTML = `
            <span class="pokemon-id">#${pokemon.id.toString().padStart(3, '0')}</span>
            <img src="${pokemon.sprites.other['official-artwork'].front_default}" alt="${pokemon.name}">
            <h3>${pokemon.name}</h3>
            <span class="type-badge" style="background-color: ${color}">${type}</span>
        `;
        
        pokedexContainer.appendChild(pokemonElement);
    });
}

// 3. Sistema de Busca
searchInput.addEventListener('input', (e) => {
    const searchValue = e.target.value.toLowerCase();
    const filteredPokemons = allPokemons.filter(pokemon => 
        pokemon.name.toLowerCase().includes(searchValue) || 
        pokemon.id.toString().includes(searchValue)
    );
    displayPokemons(filteredPokemons);
});

// Iniciar app
fetchPokemons();
