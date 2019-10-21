<template>
    <div>


        <ion-card>
            <img src="https://source.unsplash.com/800x600/?romantic"/>
            <ion-card-header>
                <ion-card-subtitle>Tips#{{this.listItem.id}}</ion-card-subtitle>
                <!--                <ion-card-title>Madison, WI</ion-card-title>-->
            </ion-card-header>
            <ion-card-content>
                {{this.listItem.text}}
            </ion-card-content>
        </ion-card>
<!--        <ion-buttons>-->
<!--            <button ion-button icon-only @click="getRandomItem()">-->
<!--                <ion-icon name="more"></ion-icon>-->
<!--            </button>-->
<!--        </ion-buttons>-->
    </div>
</template>
<script>
    export default {
        name: "myScreen",
        props: {
            // this is the list data to render
            incomingListData: Array,
             // listItem: {id: '1', text: 'some tips'}
        },
        data() {
            return {
                listItem: {id: '1', text: 'Laster inn noe smart ...'}
            }
        },

        watch: {
            // whenever question changes, this function will run
            incomingListData: function () {
                let randomItem = this.getRandomItem();
                this.listItem = randomItem;

            }
        },

        methods: {

            getRandomItem: function () {
                let index = this.getRandomImageIndex(this.incomingListData.length);
                let listElement = this.incomingListData[index];
                return listElement;
            }
            ,

            getRandomImageIndex: function (_length) {
                let number = Math.floor(Math.random() * _length);
                return number;
            }
        }
    }
    ;
</script>

<style scoped>
    .list-container {
        /* background-color: aliceblue; */
        /* padding: 10px; */
        /* margin: 10px; */
    }

    .myUnsplash {
        background-image: url('https://source.unsplash.com/800x600/?romantic');
        -webkit-background-size: contain;
        -moz-background-size: contain;
        background-size: contain;;
        background-color: red;

    }

</style>
