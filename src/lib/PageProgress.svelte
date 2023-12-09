<script lang="ts">
  import { onMount } from "svelte";

  export let color: string;
  export let height: string = "4px";
  export let breakpoints: [string, number][] = [];

  const bgColorGradient = `to right, ${breakpoints
    .map((breakpoint) => `${breakpoint[0]} calc(var(--scroll) * ${breakpoint[1] / 100}), `)
    .join("")} ${color} var(--scroll), transparent 0`;

  console.log(bgColorGradient);

  const calculateScrollPercentage = () => {
    const documentEl = document.documentElement;
    const body = document.body;

    const scrollTop = documentEl.scrollTop ?? body.scrollTop;
    const scrollHeight = documentEl.scrollHeight ?? body.scrollHeight;

    return (scrollTop / (scrollHeight - documentEl.clientHeight)) * 100;
  };

  onMount(() => {
    const progress = document.querySelector(".progress") as HTMLElement;

    const updateProgress = () => {
      const scrollPercentage = calculateScrollPercentage();
      progress.style.setProperty("--scroll", `${scrollPercentage}%`);
    };

    updateProgress();

    window.addEventListener("scroll", updateProgress);
    window.addEventListener("resize", updateProgress);
  });
</script>

<div class="progress" style="--gradient: {bgColorGradient}; --height: {height}" />

<style>
  .progress {
    background: linear-gradient(var(--gradient));
    background-repeat: no-repeat;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    width: 100%;
    height: var(--height);
    z-index: 1;
  }
</style>
