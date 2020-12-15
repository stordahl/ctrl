<script>
  import { getContext, onMount } from 'svelte';
	
  import PopupLong from './PopupLong.svelte';
  const { open } = getContext('simple-modal');
	
	const showPopupLong = () => {
		open(PopupLong, { message: "an intoduction to ctrl" });
  };
  
  onMount(() => {
    showPopupLong();
  });
</script>