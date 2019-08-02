<template id="q-prompt-validation-dialog">
  <q-dialog
    ref="dialog"
    v-model="showDialog"
    v-bind="{
        show,
        persistent,
        noEscDismiss,
        noBackdropDismiss,
        noRouteDismiss,
        autoClose,
        transitionShow,
        transitionHide,
        noRefocus,
        noFocus,
        seamless,
        maximized,
        fullWidth,
        fullHeight,
        position,
        contentClass,
        contentStyle,
        square
    }"
  >
    <q-card class="q-dialog-plugin">
      <q-card-section>
        <div class="text-h6">{{ headerText }}</div>
      </q-card-section>

      <q-card-section>
        <q-input
          ref="input"
          debounce="500"
          v-model="textValue"
          autofocus
          @keyup.enter="save"
          :rules="rules"
          @input="updateValue"
        ></q-input>
      </q-card-section>

      <q-card-actions align="right" class="text-primary">
        <q-btn flat label="Cancel" @click="cancel" v-close-popup></q-btn>
        <q-btn flat label="Save" @click="save"></q-btn>
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>


<script>
import { QDialog } from "quasar";

export default {
  extends: QDialog,
  props: {
    inputValue: {
      type: String,
      required: true
    },
    rules: Array,
    headerText: String,
    showDialog: Boolean
  },
  data() {
    return {
      textValue: ""
    };
  },
  mounted() {
    this.textValue = this.inputValue;
  },
  methods: {
    async save() {
      await this.$refs.input.validate();

      if (this.$refs.input.hasError) {
        this.$refs.dialog.shake();
      } else {
        this.$emit("ok", this.textValue);
        this.show = false;
        this.$emit("hide", false);
      }
    },
    cancel() {
      this.show = false;
      this.$emit("hide", false);
    },
    updateValue(val) {
      this.$emit("input", val);
      this.$refs.input.validate(val);
    }
  }
};
</script>