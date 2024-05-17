<template>
  <q-page class="column q-gutter-md" style="max-width: 1200px; margin: 0 auto">
    <q-card>
      <q-card-section>
        <q-input v-model="count" type="number" prefix="Jumlah Team: " />
        <q-input autogrow filled v-model="list" type="textare" label="List" />
      </q-card-section>
      <q-card-actions align="center">
        <q-btn outline color="primary" label="Cancel" @click="onReset" />
        <q-btn color="primary" label="Generate" @click="onSetTeam" />
      </q-card-actions>
    </q-card>
  </q-page>
</template>

<script setup>
import { ref } from "vue";
import { Dialog, Loading } from "quasar";

defineOptions({
  name: "TeamGenerate",
});

const list = ref("");
const count = ref(1);

const shuffle = (array) => {
  let counter = array.length;

  // While there are elements in the array
  while (counter > 0) {
    // Pick a random index
    let index = Math.floor(Math.random() * counter);

    // Decrease counter by 1
    counter--;

    // And swap the last element with it
    let temp = array[counter];
    array[counter] = array[index];
    array[index] = temp;
  }

  return array;
};
const onReset = () => {
  count.value = 1;
  list.value = "";
};

const onSetTeam = () => {
  const winner = shuffle(
    String(list.value)
      .split("\n")
      .filter((e) => Boolean(e))
  ).filter((e, i) => i < Number(count.value));

  Loading.show();
  setTimeout(() => {
    Loading.hide();
    Dialog.create({
      title: "The winner of Team",
      message: Array.from(winner).join("\n"),
      class: "Team",
      persistent: true,
    });
  }, 2000);
};
</script>

<style>
.Team .q-dialog__title {
  text-align: center;
  color: grey;
}

.Team .q-dialog__message {
  text-align: center;
  font-size: 14pt;
  font-weight: bold;
  white-space: pre-line;
}
</style>
