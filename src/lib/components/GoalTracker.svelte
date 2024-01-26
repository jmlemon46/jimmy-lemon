<script>
    import { onMount } from 'svelte';

    export let title;
    export let goalNumber;
    export let completedNumber;
    export let emoji;

    let progressBar;
    let goalPercent = 0;

    onMount(() => {
        goalPercent = (completedNumber / goalNumber) * 100;

        const observer = new IntersectionObserver(entries => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    progressBar.style.width = `${goalPercent}%`;
                    progressBar.style.opacity = '1'; // Set opacity to 100%
                    progressBar.style.paddingRight = '0.75rem'; // Restore padding at end of animation
                }
            });
        }, { threshold: 0.1 });

        observer.observe(progressBar);

        return () => observer.disconnect(); // Cleanup observer on component destruction
    });
</script>

<div class="progress">
    <p style="font-size:1.2rem;"><strong>Progress:</strong> {title}</p>
    <div class="progressCard">
        <div class="goalBar">/ {goalNumber} {emoji}</div>
        <div bind:this={progressBar} class="progressBar">
            {completedNumber}
        </div>
    </div>
</div>

<style>
    .progress {
        background:#fff;
        padding:.75rem 1rem 1.15rem 1rem;
        margin-bottom: 2rem;
        border:1px solid var(--black);

    }
    .progressCard {
        width: 100%;
        display: grid;
    }

    .goalBar,
    .progressBar {
        grid-column-start: 1;
        grid-row-start: 1;
        padding: .5rem .75rem;
        display: flex;
        justify-content: flex-end;
        border:1px solid var(--black);
    }
    .goalBar {
        width: 100%;
        background-color: #fff;
        background-color: var(--cream);
    }

    .progressBar {
        background-color: var(--yellow);
        width: 0; /* Start with 0% width */
        opacity: 0; /* Start with 0% opacity */
        transition: width .75s cubic-bezier(0.175, 0.885, 0.32, 1.275), opacity .75s ease-in-out, padding-right .75s ease-in-out; 
        padding-right: 0; /* Start with no right padding */
        transition-delay: 250ms;
        padding-left:1.25rem;
    }
</style>
