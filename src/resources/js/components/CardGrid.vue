<script setup lang="ts">
    import { reactive, ref } from 'vue';
    import WordCard from './WordCard.vue';

    const wordCards = reactive({
        items: [
            {id: 1, cardtitle: "Card1", cardSubTitle: "CardSubTitle1", cardDesc: "This is Card 1."},
            {id: 2, cardtitle: "Card2", cardSubTitle: "CardSubTitle2", cardDesc: "This is Card 2."},
            {id: 3, cardtitle: "Card3", cardSubTitle: "CardSubTitle3", cardDesc: "This is Card 3."},
            {id: 4, cardtitle: "Card4", cardSubTitle: "CardSubTitle4", cardDesc: "This is Card 4."},
            {id: 5, cardtitle: "Card5", cardSubTitle: "CardSubTitle5", cardDesc: "This is Card 5."}
        ],
    });

    // @see: https://zenn.dev/kazuwombat/articles/f23b47f168f1d0
    const dragFromIndex = ref<number | null>(null)
    const saveFromIndex = (index: number) => {
        dragFromIndex.value = index
    }
    const moveItem = (targetIndex: number) => {
        if (dragFromIndex.value === null) return
        wordCards.items = moveIndex(wordCards.items, dragFromIndex.value, targetIndex)
    }
    const moveIndex = <T>(original: T[], from: number, to: number) => {
        const arr = [...original]
        const target = arr[from]
        arr.splice(from, 1)
        arr.splice(to, 0, target)
        return arr
    }
</script>

<template>
    <div class="row row-cols-1 row-cols-sm-2 g-3 m-5">
        <div v-for="(card, index) in wordCards.items">
            <div 
                class="col drop-area" 
                draggable="true" 
                @dragstart="() => saveFromIndex(index)" 
                @drop="() => moveItem(index)" 
                @dragover.prevent
            >
                <WordCard :cardTitle=card.cardtitle :cardSubTitle=card.cardSubTitle :cardDesc=card.cardDesc />
            </div>
        </div>    
    </div>
</template>
