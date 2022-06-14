<script>
  // Components
  import MobileMenuTopics from "./topics.svelte";
  import MobileMenuSubMenu from "./sub-menu.svelte";

  // States
  import topicsState from "../states/topics-state";

  import docsCurrentSectionStore from "$lib/stores/docs-current-section";
  import EditInGitpod from "../edit-in-gitpod.svelte";
  import { navigating } from "$app/stores";
  export let MENU;

  $: if ($navigating) {
    $topicsState = false;
  }
  $: currentSection = MENU.find(({ path }) =>
    $docsCurrentSectionStore
      ? path.indexOf(
          /self-hosted\/\d\.\d\.\d/.test($docsCurrentSectionStore)
            ? $docsCurrentSectionStore.replace(/\d\.\d\.\d/, "latest")
            : $docsCurrentSectionStore
        ) >= 0
      : /\/docs$/.test(path)
  );
</script>

<div class="mobile-menu mb-10 lg:hidden">
  {#if $topicsState}
    <MobileMenuTopics {MENU} />
  {:else}
    <MobileMenuSubMenu {currentSection} />
  {/if}
  <div class="mt-4">
    <EditInGitpod renderedOn="mobile" />
  </div>
</div>
