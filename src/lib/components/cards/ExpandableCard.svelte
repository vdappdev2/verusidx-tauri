<script lang="ts">
  import Modal from './Modal.svelte';

  interface Props {
    title: string;
    cardClass?: string;
    modalSize?: 'sm' | 'md' | 'lg' | 'xl' | 'full';
  }

  let { title, cardClass = '', modalSize = 'md' }: Props = $props();

  let isModalOpen = $state(false);

  function openModal() {
    isModalOpen = true;
  }

  function closeModal() {
    isModalOpen = false;
  }
</script>

<!-- Clickable Card -->
<div 
  class="{cardClass || 'bg-white dark:bg-verusidx-stone-dark text-verusidx-stone-dark dark:text-white'} rounded-lg shadow-xl cursor-pointer transition-all duration-200 hover:shadow-2xl hover:scale-[1.02]"
  onclick={openModal}
  onkeydown={(e) => e.key === 'Enter' && openModal()}
  tabindex="0"
  role="button"
  aria-label="Click to expand {title}"
>
  <div class="p-6 border-b border-verusidx-mountain-mist dark:border-verusidx-stone-medium">
    <div class="flex items-center justify-between">
      <h3 class="text-lg font-semibold text-verusidx-stone-dark dark:text-white">{title}</h3>
      <!-- Expand icon -->
      <svg 
        class="h-5 w-5 text-verusidx-mountain-mist dark:text-verusidx-mountain-grey transition-transform duration-200 group-hover:scale-110" 
        fill="none" 
        viewBox="0 0 24 24" 
        stroke="currentColor"
      >
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 8V4m0 0h4M4 4l5 5m11-1V4m0 0h-4m4 0l-5 5M4 16v4m0 0h4m-4 0l5-5m11 5l-5-5m5 5v-4m0 4h-4" />
      </svg>
    </div>
  </div>
  
  <div class="p-6">
    <div class="space-y-3">
      <slot name="preview" />
    </div>
  </div>
</div>

<!-- Modal for expanded view -->
{#if isModalOpen}
  <Modal isOpen={isModalOpen} {title} size={modalSize} onclose={closeModal}>
    <slot name="expanded" />
  </Modal>
{/if}