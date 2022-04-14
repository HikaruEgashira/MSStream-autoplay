<template>
  <div
    v-if="isDev"
    class="fixed right-0 bottom-0 m-5 z-100 flex font-sans select-none leading-1em"
  >
    <div
      class="flex w-10 h-10 rounded-full shadow cursor-pointer"
      bg="teal-600 hover:teal-700"
    >
      <pixelarticons-power class="block m-auto text-white text-lg" />
    </div>
  </div>
</template>

<script setup lang="ts">
import "virtual:windi.css";
import { useMediaControls } from "@vueuse/core";

const isDev = process.env.NODE_ENV === "development";
const sleep = (ms: number) => new Promise((resolve) => setTimeout(resolve, ms));

const video = ref<HTMLVideoElement | null>(null);
const { ended } = useMediaControls(video);
watch(ended, async (whenEnded) => {
  if (whenEnded) {
    document.querySelectorAll("button.video-action-unpin")[0]?.click();
    document.querySelectorAll("a.video-item-image-link")[0]?.click();

    await sleep(5000);
    ended.value = false;
    video.value = await getVideo();
  }
});

async function getVideo(): Promise<HTMLVideoElement> {
  const video = document.querySelector("video");
  if (video) return video;

  await sleep(1000);
  return await getVideo();
}

onMounted(async () => {
  video.value = await getVideo();
});
</script>
