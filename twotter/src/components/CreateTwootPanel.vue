<template>
  <form
    class="create-twoot-panel"
    @submit.prevent="createNewTwoot"
    :class="{ '--exceeded': newTwootCharacterCount > 180 }"
  >
    <label for="newTwoot"
      ><strong>New Twoot</strong> ({{ newTwootCharacterCount }}/180)</label
    >
    <textarea
      name="newTwoot"
      id="newTwoot"
      rows="4"
      v-model="state.newTwootContent"
    />

    <div class="create-twoot-panel_submit">
      <div class="create-twoot-type">
        <label for="newTwootType"><strong>Type: </strong></label>
        <select
          name="newTwootType"
          id="newTwootType"
          v-model="state.selectectTwootType"
        >
          <option
            :value="option.value"
            v-for="(option, index) in state.twootTypes"
            :key="index"
          >
            {{ option.name }}
          </option>
        </select>
      </div>

      <button>Twoot It!</button>
    </div>
  </form>
</template>

<script>
import { reactive, computed } from "vue";

export default {
  name: "CreateTwootPanel",
  setup(props, context) {
    const state = reactive({
      newTwootContent: "",
      selectectTwootType: "instant",
      twootTypes: [
        { value: "draft", name: "Draft" },
        { value: "instant", name: "Instant" },
      ],
    });

// computed
    const newTwootCharacterCount = computed(() => state.newTwootContent.length);

// methods
    function createNewTwoot() {
      if (
        state.newTwootContent &&
        state.selectectTwootType !== "draft" &&
        this.newTwootCharacterCount <= 180
      ) {
        context.emit("add-twoot", state.newTwootContent);
        state.newTwootContent = "";
      }
    }

    return { state, newTwootCharacterCount, createNewTwoot };
  },
};
</script>

<style lang="scss" scoped>
.create-twoot-panel {
  margin-top: 20px;
  padding: 20px 0;
  display: flex;
  flex-direction: column;

  textarea {
    border: 1px solid #dfe3e8;
    border-radius: 5px;
  }

  .create-twoot-panel_submit {
    display: flex;
    justify-content: space-between;

    .create-twoot-type {
      padding: 10px 0;
    }

    button {
      padding: 5px 20px;
      margin: auto 0;
      border-radius: 5px;
      border: none;
      background-color: deeppink;
      color: white;
      font-weight: bold;
    }
  }

  &.--exceeded {
    color: red;
    border-color: red;

    .create-twoot-panel_submit {
      button {
        background-color: red;
        color: white;
      }
    }
  }
}
</style>