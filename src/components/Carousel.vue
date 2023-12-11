<template>
    <div class="carousel" v-bind:style="{ background: imageColors[currentSlide - 1] }">
        <slot :currentSlide="currentSlide" />

        <div class="p-0 px-16 h-full w-full absolute flex justify-center items-center">
            <div class="toggle-page flex flex-1">
                <div class="cursor-pointer flex items-center justify-center rounded-full w-6 h-6 bg-purple-600 text-white">
                    <font-awesome-icon @click.stop="prevSlide" icon="fa-solid fa-chevron-left" class="w-3 h-3" />
                </div>
            </div>
            <div class="toggle-page flex flex-1 justify-end">
                <div class="cursor-pointer flex items-center justify-center rounded-full w-6 h-6 bg-purple-600 text-white">
                    <font-awesome-icon @click.stop="nextSlide" icon="fa-solid fa-chevron-right" class="w-3 h-3" />
                </div>
            </div>
        </div>

        <div class="absolute bottom-24 w-full flex justify-center items-center gap-4">
            <span v-for="(slide, index) in getSlideCount" :key="index" 
            :class="['cursor-pointer', 'w-5', 'h-5', 'rounded-full', 'shadow-md', index === currentSlide - 1 ? 'bg-purple-600' : 'bg-white']" @click="goToSlide(index)">
            </span>
        </div>
    </div>
</template>



<script setup lang="ts">
import { ref, onMounted, type Ref } from 'vue';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import ColorThief from 'colorthief';

const colorThief = new ColorThief();

let imageColors : string[] = [];
const currentSlide : Ref<number> = ref(1)

const getSlideCount: Ref<number> = ref(0)

const nextSlide = () => {
    if (currentSlide.value === getSlideCount.value) {
        currentSlide.value = 1
        return
    }
    currentSlide.value += 1
}

const prevSlide = () => {
    if (currentSlide.value === 1) {
        currentSlide.value = getSlideCount.value;
        return;
    }
    currentSlide.value -= 1;
}

const goToSlide = (index : number) => {
    currentSlide.value = index + 1
}


const autoPlayEnabled : Ref<boolean> = ref(true);
const timeoutDuration : Ref<number> = ref(5000);

const autoPlay = () => {
    setInterval(() => {
        nextSlide()
    }, timeoutDuration.value)
}

if(autoPlayEnabled.value) {
    // autoPlay();
}

const getColorsOfImages = () => {
    const images = document.querySelectorAll('.slide img');
    images.forEach((image : HTMLImageElement) => {
        if(image.complete) {
            const color = colorThief.getColor(image);
            const rgb = `radial-gradient(circle at center, rgb(${color[0]}, ${color[1]}, ${color[2]}), white)`;
            imageColors.push(rgb);
        }
        else{
            image.addEventListener('load', () => {
                const color = colorThief.getColor(image);
                const rgb = `radial-gradient(circle at center, rgb(${color[0]}, ${color[1]}, ${color[2]}), white)`;
                imageColors.push(rgb);
            })
        }
    })
}

onMounted(() => {
    getSlideCount.value = document.querySelectorAll('.slide').length
    getColorsOfImages();
})

</script>