<template>
    <div class="col-md-3 list m-2" ref="listRef">
        <h2>{{ list.title }}</h2>
        <draggable v-model="list.cards" @end="onCardDragEnd" group="list.cards" :id="list.id">
                <card v-for="card in list.cards" :key="card.id" :card="card" :id="card.id" />
        </draggable>
        <input v-model="newCard" class="form-control mb-4" @keyup.enter="addCard" placeholder="Add a card..." />
    </div>
</template>

<script>
    import { VueDraggableNext } from 'vue-draggable-next'
    import Card from './Card.vue';

    export default {
        components: {
            draggable: VueDraggableNext,
            Card,
        },
        props: ['list'],
        data() {
            return {
                newCard: '',
            };
        },
        methods: {
            onCardDragEnd(event) {

                let toListID = event.to.id
                let fromListID = event.from.id
                this.$emit('update-list-cards',toListID,fromListID);

            },
            addCard() {
                if (this.newCard.trim() !== '') {
                    this.$emit('add-card', this.list.id, this.newCard.trim());
                    this.newCard = '';
                }
            },
        },
    };
</script>

<style scoped>
    .list {
        border: 1px solid #ccc;
        border-radius: 5px;
        background-color: #f9f9f9;
    }

    input {
        margin-top: 5px;
        padding: 5px;
        width: 100%;
    }
</style>
