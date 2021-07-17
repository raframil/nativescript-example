<template>
  <Page>
    <ActionBar>
      <GridLayout width="100%" columns="auto, *">
        <Label
          text="Menu"
          @tap="$refs.drawer.nativeView.showDrawer()"
          col="0"
        />
        <Label class="title" text="Pokédex 1.0.0" col="1" />
      </GridLayout>
    </ActionBar>

    <RadSideDrawer ref="drawer">
      <StackLayout ~drawerContent backgroundColor="#ffffff">
        <Label class="drawer-header" text="Drawer" />

        <Button
          text="Abrir Pokédex"
          @tap="goToPokedex()"
          class="drawerContentButton"
        ></Button>
      </StackLayout>

      <StackLayout  ~mainContent>
        <Label class="message" :text="msg" height="70"/>

        <Image src="~/assets/images/pokedex.png" class="thumb" @tap="goToPokedex()"/>
      </StackLayout >
    </RadSideDrawer>
  </Page>
</template>

<script>
import Lista from "./Lista";

export default {
  data() {
    return {
      msg: "Abra sua Pokédex clicando abaixo!",
    };
  },
  methods: {
    goToPokedex() {
      this.$navigateTo(Lista);
    },
  },
};
</script>

<style scoped>
ActionBar {
  background-color: #53ba82;
  color: #ffffff;
}

.title {
  text-align: left;
  padding-left: 16;
}

.message {
  vertical-align: center;
  text-align: center;
  font-size: 20;
  color: #333333;
  margin-top: 100px;
}

.drawer-header {
  padding: 50 16 16 16;
  margin-bottom: 16;
  background-color: #53ba82;
  color: #ffffff;
  font-size: 24;
}

.drawer-item {
  padding: 8 16;
  color: #333333;
  font-size: 16;
}

.openPokedexButton {
  padding: 8 16;
  height: 100px;
}

.thumb {
  width: 300;
}

.far {
    font-family: "Font Awesome 5 Free", "fa-regular-400";
    font-weight: 400;
}

.fas {
    font-family: "Font Awesome 5 Free", "fa-solid-900";
    font-weight: 900;
}

.fab {
    font-family: 'Font Awesome 5 Brands', 'fa-brands-400';
    font-weight: 400;
}
</style>
