<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    // Global Variables
    export let parentId = '';
    export let hasCancelButton = false;    
    export let text = '';

    // Local Veriables
    let isTextAreaDisabled = false;

    // Submit Comment
    function onSubmit () {
        // Add the comment, reply to the store
        dispatch('add', {
            id: crypto.randomUUID(),
            parentId,
            body: text,
            createdAt: new Date(),
        });

        // Clear the text area
        text = '';

        // Close the form
        dispatch('close');
    }

    // Check if text is empty or to long
    function isDisabled () {
        return text.trim().length > 200 || text.trim().length === 0;
    }

    // Hanldes the text change
    $: text,isTextAreaDisabled = isDisabled();

</script>

<div class="comment-area">
    <form action="" method="POST" on:submit|preventDefault={onSubmit}>
        <textarea name="comment" cols="30" rows="10" placeholder="Gives us some feedback!" bind:value={text} />
        <div class="comment-area-controls">
            <div class="comment-area-actions">
                <button type="submit" disabled='{isTextAreaDisabled}' >Submit</button>
                {#if hasCancelButton}
                    <button type="button" on:click={() => dispatch('close')}>Cancel</button>
                {/if}
            </div>
            <span class:error={isTextAreaDisabled}>{text.length} / 200</span>
        </div>
    </form>
</div>

<style>
    .comment-area {
        max-width: 800px;
        margin: 0 auto;
        padding: 1rem 0
    }

    .comment-area-controls {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .comment-area-actions {
        display: flex;
        gap: 1rem;
    }

    .error {
        color: red;
    }

    textarea {
        width: 100%;
        height: 100px;
        border: 1px solid #ccc;
        border-radius: 5px;
        padding: 10px;
        font-size: 1.2rem;
        resize: none;
    }
    
    button {
        background: none;
        color: #fff;
        border: none;
        border-radius: 5px;
        padding: 10px 0;
        font-size: 1rem;
        cursor: pointer;
    }

    button:hover {
        color: rgba(0, 0, 0, 0.5);
    }

    button:disabled {
        color: rgba(0, 0, 0, 0.5);
        cursor: not-allowed;
    }
</style>