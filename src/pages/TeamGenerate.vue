<template>
  <q-page class="column q-gutter-md" style="max-width: 1200px; margin: 0 auto">
    <q-card>
      <q-card-section class="q-gutter-sm">
        <q-input v-model="count" type="number" prefix="Jumlah Team: " />
        <q-input
          autofocus
          v-for="(str, key) in list"
          :key="key"
          autogrow
          filled
          v-model="list[key]"
          type="textare"
          :label="`List ${key + 1}`"
          input-style="max-height:500px"
        />
        <q-btn dense flat label="add List" size="sm" @click="list.push('')" />
      </q-card-section>
      <q-card-actions align="center">
        <q-btn outline color="primary" label="Cancel" @click="onReset" />
        <q-btn color="primary" label="Generate" @click="onSetTeam" />
      </q-card-actions>
    </q-card>
  </q-page>
  <q-dialog :model-value="Boolean(result)" persistent @hide="result = null">
    <q-card style="max-width: 90vw">
      <q-card-section class="row items-center">
        <q-markup-table>
          <thead>
            <tr>
              <th class="text-center" v-for="(e, i) in result" :key="i">
                TEAM-{{ 1 + i }}
              </th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td
                class="text-center"
                v-for="(e, i) in result"
                :key="i"
                style="vertical-align: top"
              >
                <div v-for="(name, x) in e" :key="x">
                  {{ name }}
                </div>
              </td>
            </tr>
          </tbody>
        </q-markup-table>
      </q-card-section>
      <q-card-actions align="right">
        <q-btn flat label="OK" color="primary" v-close-popup />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script setup>
import { ref } from "vue";
import { Dialog, Loading } from "quasar";

defineOptions({
  name: "TeamGenerate",
});

const result = ref(null);

const list = ref([""]);
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

const setGroup = (array, counter) => {
  const v = [];
  array.forEach((e, i) => {
    const index = i % Number(counter);
    if (!!!v[index]) v[index] = [];
    v[index].push(e);
  });
  return v;
};

const onReset = () => {
  count.value = 1;
  list.value = "";
};

const onSetTeam = () => {
  const team = [];
  list.value.forEach((str) => {
    shuffle(
      String(str)
        .split("\n")
        .filter((e) => String(e).length)
    ).forEach((e) => team.push(e));
  });

  Loading.show();
  setTimeout(() => {
    Loading.hide();
    result.value = setGroup(team, count.value);
    // Dialog.create({
    //   title: "The winner of Team",
    //   message: Array.from(team).join("\n"),
    //   class: "Team",
    //   persistent: true,
    // });
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
