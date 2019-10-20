<template>
    <div id="app">
        <ion-app>
            <ion-header>
                <ion-toolbar color="primary">
                    <ion-title>Romantic tips</ion-title>
                </ion-toolbar>
            </ion-header>
            <ion-content padding>
                <ion-card padding>
                    <MyName name="Aaron" @was-clicked="handleWasClicked"></MyName>
                </ion-card>
                <ion-card padding>
                    <my-list
                            :incomingListData="theTextArray"
                            @list-clicked="handleListClicked"
                            @delete-item="handleDeleteListItem"
                            @edit-item="handleEditListItem"
                    ></my-list>
                </ion-card>
                <ion-card padding>
                    <template v-if="inEditMode">
                        <my-form
                                :initialFormData="editingText"
                                @form-clicked="handleFormEdit"
                                @form-cancelled="handleFormCancelled"
                        ></my-form>
                    </template>
                    <template v-else>
                        <my-form
                                :initialFormData="{}"
                                @form-clicked="handleFormClicked"
                                @form-cancelled="handleFormCancelled"
                        ></my-form>
                    </template>
                </ion-card>
            </ion-content>
        </ion-app>
    </div>
</template>
<style src='@ionic/core/css/core.css'></style>
<style src='@ionic/core/css/ionic.bundle.css'></style>
<script>
    /**
     ALL of the components that are being used in this component
     must be imported here
     */
    import MyText from "./components/MyText.vue";
    import MyList from "./components/MyList.vue";
    import MyForm from "./components/MyForm.vue";
    import axios from 'axios';

    export default {
        name: "app",
        /**
         ALL of the components that are being used in this component
         must be listed here
         */
        components: {
            MyName: MyText,
            MyList,
            MyForm
        },
        computed: {
            inEditMode: function () {
                return this.editingText.id != undefined ? true : false;
            }
        },

        mounted() { // when the Vue app is booted up, this is run automatically.
            console.info("mounted");
            axios.get('https://generic-rest-api.herokuapp.com/romantic/list')
                .then(response => {
                    console.info("Received data from endpoint");
                    // JSON responses are automatically parsed.
                    this.theTextArray = response.data;
                    console.info("Data:" + JSON.stringify(this.theTextArray));
                })
                .catch(e => {
                    console.error("Failed to get data" + e);
                    this.errors.push(e)
                })
        }
        ,

        data() {

            return {
                editingText: {},
                /* we put some dummy data in here to pre-fill the list */
                theTextArray: [
                    {id: 0, text: "Tips 1"},
                    {id: 1, text: "Tips 2"},
                ]
            };
        },
        methods: {
            handleWasClicked: function (_data) {
               console.info(_data);
                //alert(_data);
            },
            /**
             * the data from the list click DOES contain an id becuase
             * this is handling list items, and for an object to be the
             * list it must have an id.
             *
             * it is id, name.irstName and the name.lastName
             */
            handleListClicked: function (_data) {
               console.info(_data.text);
//                alert(_data.text);
            },
            /**
             * the data from the list click DOES contain an id becuase
             * this is handling list items, and for an object to be the
             * list it must have an id.
             *
             * it is id, name.irstName and the name.lastName
             */
            handleEditListItem: function (_item) {
                //alert(_item.text);
                console.info(_item.text);
                this.editingText = {..._item};
            },

            /**
             * the data from the list click DOES contain an id becuase
             * this is handling list items, and for an object to be the
             * list it must have an id.
             *
             * it is id, name.irstName and the name.lastName
             */
            handleDeleteListItem: function (_item) {
                console.log("deleteListElement", _item);
                let result = this.theTextArray.filter(element => {
                    return element.id != _item.id;
                });
                this.theTextArray = result;
            },
            /**
             * the data from the form click DOES NOT contain an id becuase
             * this is handling when we are creating a new object.
             *
             * it is just the firstName and the lastName
             */
            handleFormClicked: function (_data) {
                console.log("handleFormClicked", _data);
                // add the item to the array
                this.theTextArray.push({
                    text: _data,
                    id: new Date().getTime()
                });
            },
            /**
             */
            handleFormEdit: function (_item) {
                console.log("handleFormEdit", _item);
                let edited = this.theTextArray.map(item => {
                    if (item.id == _item.id) {
                        return {..._item};
                    } else {
                        return item;
                    }
                });

                this.theTextArray = edited;
                this.editingText = {};
            },
            /**
             */
            handleFormCancelled: function (_data) {
                console.log(_data);
                this.editingText = {};
            }
        }
    };
</script>

<style>
    #app {
        font-family: "Avenir", Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        color: #2c3e50;
        margin-top: 60px;
        padding: 30px;
    }
</style>
