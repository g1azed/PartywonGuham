<template>
    <div
        id="tooltip"
        class="fixed z-20 right-0 bottom-0 pr-5 pb-5"
    >
        <button
            v-on:click="goToTop"
            v-on:mousedown="onMouseDown"
            v-on:mouseup="onMouseUp"
            aria-label="go-up-btn"
            id="go-up-btn"
            class="w-10 h-10 flex justify-center items-center rounded-full text-lg translate-x-16 opacity-0 duration-200 bg-orange-f6 lg:bg-orange-f6 text-white"
        >
            <i class="fa-solid fa-angle-up"></i>
        </button>
    </div>
</template>

<script setup lang="ts">
    const goToTop = () => {
        window.scrollTo({ top: 0, behavior: 'smooth' });
    }

    const onMouseDown = (e: MouseEvent) => {
        const el = e.currentTarget as HTMLButtonElement;
        el.classList.remove("lg:bg-orange-f6");
        el.classList.add("lg:bg-orange-f3");
    }

    const onMouseUp = (e: MouseEvent) => {
        const el = e.currentTarget as HTMLButtonElement;
        el.classList.remove("lg:bg-orange-f3");
        el.classList.add("lg:bg-orange-f6");
    }

    const onGlobalMouseUp = () => {
        const el = document.getElementById("go-up-btn") as HTMLButtonElement;
        el.classList.remove("lg:bg-orange-f3");
        el.classList.add("lg:bg-orange-f6");
    }

    const onGlobalScroll = () => {
        const el = document.getElementById("go-up-btn") as HTMLButtonElement;
        const tooltip = document.getElementById("tooltip") as HTMLDivElement;
        const footer = document.getElementsByTagName("footer")[0];
        const windowBottom = window.scrollY + window.innerHeight;

        // 풋터를 만나면
        if (windowBottom >= (document.body.offsetHeight - footer.offsetHeight)) {
            tooltip.classList.remove("fixed", "bottom-0");
            tooltip.classList.add("absolute", "bottom-20");
        } else {
            tooltip.classList.remove("absolute", "bottom-20");
            tooltip.classList.add("fixed", "bottom-0");
        }

        // 뷰포트 높이를 벗어나면
        if (window.innerHeight <= window.scrollY) {
            el.classList.remove("translate-x-16", "opacity-0");
        } else {
            el.classList.add("translate-x-16", "opacity-0");
        }
    }

    onMounted(() => {
        window.addEventListener("scroll", onGlobalScroll);
        window.addEventListener("mouseup", onGlobalMouseUp);
    });

    onUnmounted(() => {
        window.removeEventListener("scroll", onGlobalScroll);
        window.removeEventListener("mouseup", onGlobalMouseUp);
    });
</script>