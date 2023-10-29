<script>
    export let checked = false
    export let checkboxes = []
    export let group = []
 
 </script>
 
 { #each checkboxes as checkbox }
    <label class="label" for="{checkbox.id}">{ checkbox.text }</label>
    <input type="checkbox" id="{checkbox.id}" value={ checkbox.id } bind:group>
 { /each }


 <style>
    .checkbox {
        background-color: white;
        border-radius: 5px;
        display: flex;
        padding: 16px;
        justify-content: center;
        align-items: center;
        gap: 7px;
        width: max-content;
        color: var(--purple);
        border: 1px solid var(--purple);
        accent-color: var(--purple);
        font-weight: 500;
    }

    .checkbox.checked {
        background-color: var(--purple);
        color: white;
    }
 </style>