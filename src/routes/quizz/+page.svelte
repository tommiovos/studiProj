<script>
	import { onMount } from 'svelte';
    import { writable, derived } from 'svelte/store';
	import FullForm from './../../lib/components/FullForm.svelte';
    import { tweened } from 'svelte/motion';
    import { quintInOut } from 'svelte/easing';
    import { browser } from '$app/environment'; 

    let loaded = false;
    let windowWidth = 0;

    function onResize() {
        windowWidth = window.innerWidth;
        if (el) {
            storedEl = el;
            if (!observerActive) {
                setTimeout(() => {
                    observerActive = true;
                    observer.observe(el);
                }, 10);
            }
            $popupHeight = el.scrollHeight;     
        }
        if (windowWidth >= 1080) {
            document.querySelector("body").style.backgroundColor = "white";    
        }
        else {
            document.querySelector("body").style.backgroundColor = "#F0FEFF";
        }
    }

    const popupHeight = tweened(0, {
        duration: 300,
        easing: quintInOut,
    })


    let el;
    let elHeight;
    let observer;
    let observerActive = false;
    let storedEl;
    onMount(() => {
        setTimeout(() => {
            loaded = true;    
        }, 500);

        setTimeout(() => {
            observer = createObserver();
            if (el) {
                storedEl = el;
                observerActive = true;
                observer.observe(el);
            };
        }, 10);
        
        
        windowWidth = window.innerWidth;
        window.addEventListener('resize', onResize);
        
        if (windowWidth >= 1080) {
            document.querySelector("body").style.backgroundColor = "white";    
        }
        
 		return () => {
            window.removeEventListener('resize', onResize);
            if (windowWidth >= 1080) {
                //TODO: check this
                document.querySelector("body").style.backgroundColor = "";
            }
            if (observerActive && observer != null) {
                observer.unobserve(storedEl);    
            }
        };
    });

    function createObserver() {
        const resizeObserver = new ResizeObserver(entries => {
                // We're only watching one element
            const entry = entries.at(0);

                //Get the block size
            elHeight = entry.contentBoxSize[0].blockSize;
            $popupHeight = elHeight;
        });

        return resizeObserver;
    }
</script>


<div>
        {#if windowWidth < 1080}
            <div class="mobile-cont">
                <FullForm/>
            </div>
        {:else}
            <div class="pc-cont" style="opacity : {loaded ? '1' : '0'}">
                <div class="pc-popup" style="height: {$popupHeight}px">
                    <div class="pc-content" bind:this={el}>
                        <FullForm/>
                    </div>    
                </div> 
            </div>  
        {/if}    
</div>

<style>
    .mobile-cont {
        width: 100%;
        height: 100%;
    }

    .pc-cont {
        height: 100dvh;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .pc-popup {
        display: flex;
        transform: rotate(0.85deg);
        padding: 46px 56px 70px 56px;
        flex-direction: column;
        align-items: flex-start;
        gap: 10px;
        border-radius: 26px;
        border: 3px dashed #1B065E;
        background: #F0FEFF;
        box-shadow: -17px 14px 0px 0px #1B065E;
        width: 80%;
        height: auto;
        overflow: hidden;
        justify-content: center;
    }

    .pc-content {
        transform: rotate(-0.85deg);
        width: 100%;
    }
</style>




