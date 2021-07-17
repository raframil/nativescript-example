<template>
  <Page>
    <ActionBar title="Lista de Pokémon">
      <ActionItem
        ios.position="left"
        @tap="goBack()"
        android.position="actionBar"
        android.systemIcon="ic_menu_revert"
        >Voltar</ActionItem
      >
    </ActionBar>
    <StackLayout backgroundColor="#3c495e" v-if="!loading">
      <ListView
        class="list-group"
        for="pokemon in pokedex"
        separatorColor="#e6e6e6"
        @itemTap="onItemTap"
      >
        <v-template>
          <FlexboxLayout flexDirection="row" class="list-group-item">
            <Image :src="pokemon.sprite" class="thumb" />
            <Label
              :text="pokemon.name"
              class="list-group-item-heading"
              style="width: 60%"
            />
          </FlexboxLayout>
        </v-template>
      </ListView>
    </StackLayout>

    <div v-if="loading">
        <p>Carregando...</p>
    </div>
  </Page>
</template>

<script>
import axios from "axios/dist/axios";
import Pokemon from "./Pokemon";

const POKE_API_BASE_URL = "https://pokeapi.co/api/v2";

export default {
  data() {
    return {
      pokedex: [],
      isLoading: false,
    };
  },

  async mounted() {
    await this.getPokemons();
  },

  methods: {
    goBack: function (args) {
      this.$navigateBack();
    },
    async getPokemons() {
      try {
        this.isLoading = true;
        const { data } = await axios.get(
          "https://pokeapi.co/api/v2/pokemon/?limit=150"
        );
        this.pokedex = data.results.map((item) => {
          const id = item.url.split("/")[6];
          return {
            id,
            name: item.name,
            sprite: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${id}.png`,
          };
        });
      } catch (error) {
        console.log(error);
      } finally {
        this.isLoading = false;
      }
    },
    onItemTap: async function (args) {
      const id = args.item.id;
      const { data: pokemonData } = await axios.get(
        `${POKE_API_BASE_URL}/pokemon/${id}`
      );
      const { data: pokemonSpecieData } = await axios.get(
        `${POKE_API_BASE_URL}/pokemon-species/${id}`
      );

      const { types } = pokemonData;
      const { flavor_text_entries } = pokemonSpecieData;

      const pokemon_types = types.map(({ slot, type }) => {
        return {
          id: slot,
          name: type.name,
          classname: `TypeText ${type.name}`,
        };
      });

      console.log(pokemon_description);

      const pokemon_description = flavor_text_entries.find(
        (item) => item.language.name === "en"
      ).flavor_text;

      this.$navigateTo(Pokemon, {
        props: {
          id,
          name: args.item.name,
          description: pokemon_description,
          types: pokemon_types,
          image: `https://img.pokemondb.net/artwork/vector/large/${args.item.name}.png`,
        },
      });
    },
  },
};
</script>

<style scoped>
ActionBar {
  background-color: #53ba82;
  color: #ffffff;
}

.list-group-item {
  background-color: #fff;
  align-items: center;
}

.thumb {
  width: 100;
}

.list-group-item-heading {
  color: #333;
  font-size: 20;
  margin-left: 20;
}
</style>
