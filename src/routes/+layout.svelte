<script></script>

<slot></slot>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@100;200;300;400;500;600;700&display=swap');


    :root {
        --purple: #1B065E;
        --blue: #00E8FC;
        --blue-light: #F0FEFF;
    }

    :global(body) {
        background-color: #F0FEFF;
    }

    :global(h1, h2, h3, h4, p, span, div, button) {
        font-family: 'Inter', sans-serif;
    }

    :global(.btn-main, .btn-sec, .btn-special) {
       border-radius: 12px;
       padding: 18px 24px;
       font-size: 24px;
       font-weight: 600;
       display: flex;
       justify-content: center;
       align-items: center;
       cursor: pointer;
       box-sizing: border-box;
    }

    :global(.btn-main) {
        background-color: var(--purple);
        padding: 24px;
        color: white;
        border: none;
        border: 2px solid var(--purple);
    }

    :global(.btn-sec) {
        border: 2px solid var(--purple);
        background-color: #FFF;
        color: var(--purple);
    }

    :global(.btn-special) {
        border: 2px solid var(--purple);
        background: #00E8FC;
        color: var(--purple);
        padding: 20px 29px 20px 24px;
    }

    :global(.btn-special::before) {
        content: url(images/sparkle.svg);
        width: auto;
        height: 80%;
        display: flex;
        justify-content: center;
        align-items: center;
        padding-top: 5px;
        padding-right: 5px;
    }

    :global(.purple-text) {
        color: var(--purple);
    }

    :global(.text-center) {
        text-align: center;
    }
</style>