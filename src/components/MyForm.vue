<template>
  <div class="form-container">
    <h3>{{formTitle}}</h3>
    <div>
      <ion-item>
        <ion-label>Tips</ion-label>
        <ion-input @input="textData.text = $event.target.value" :value="textData.text"></ion-input>
      </ion-item>
      <!--<ion-item>
        <ion-label>Last Name</ion-label>
        <ion-input @input="textData.lastName = $event.target.value" :value="textData.lastName"></ion-input>
      </ion-item>-->
      <ion-button @click="handleBtnClicked()">Save</ion-button>
      <ion-button  color="danger" @click="handleCancelClicked()">Cancel</ion-button>
    </div>
  </div>
</template>
<script>
export default {
  name: "MyForm",
  props: {
    // this is the data being passed into the component, see the watch section
    // below
    initialFormData: Object
  },
  computed: {
    /**
     * if there is an id then we know we ae editing and we
     * are adjusting the title appropriately
     */
    formTitle: function() {
      return this.initialFormData.id !== undefined
        ? "Editing tips: " + this.initialFormData.id
        : "Add a new tips";
    }
  },
  watch: {
    /**
     * The best way is to watch your prop then update your data if you want. You
     * will always access the last props even if they change after the creation
     * of the component.
     */
    initialFormData: function(_initialData) {
      // if we have a name field then set the form to
      // be able to edit the name
      if (_initialData.text) {
        this.textData = { ..._initialData.text };
      }
    }
  },
  data() {
    return {
      // here we are using the initialFormData to set the v-model data
      // that will be used in the component
      textData: ""
    };
  },
  methods: {
    /**
     * this will emit an "form-clicked" event to the parent with the
     * value from the textData property
     *
     * the textData will only contain the firstName and LastName
     */
    handleBtnClicked: function() {
      var returnVal = {};
      if (this.initialFormData.id !== undefined) {
        returnVal = {
          id: this.initialFormData.id,
          text: this.textData
        };
      } else {
        returnVal = this.textData;
      }

      this.$emit("form-clicked", returnVal);
      // clear the ui
      this.textData = {};
    },
    /**
     * this will emit an "form-cancelled" event to the parent with the
     * value from the textData property
     *
     * the textData will be empty
     */
    handleCancelClicked: function() {
      this.$emit("form-cancelled", {});
      // clear the ui
      this.textData = {};
    }
  },
  created() {}
};
</script>
<style scoped>
.form-container {
  /* background-color: aliceblue;
  padding: 10px;
  margin: 10px; */
}
</style>
