<script>
</script>

<svelte:head>
    <!-- Google Tag Manager -->
    <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
    new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
    j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
    'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
    })(window,document,'script','dataLayer','GTM-WQBV9P4V');</script>
    <!-- End Google Tag Manager -->

    <!-- Google tag (gtag.js) -->
    <script async src="https://www.googletagmanager.com/gtag/js?id=G-1KEDJR3ZRW"></script>
    <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());

    gtag('config', 'G-1KEDJR3ZRW');
    </script>
    
    <script>
        (function(h,o,t,j,a,r){
            h.hj=h.hj||function(){(h.hj.q=h.hj.q||[]).push(arguments)};
            h._hjSettings={hjid:3716348,hjsv:6};
            a=o.getElementsByTagName('head')[0];
            r=o.createElement('script');r.async=1;
            r.src=t+h._hjSettings.hjid+j+h._hjSettings.hjsv;
            a.appendChild(r);
        })(window,document,'https://static.hotjar.com/c/hotjar-','.js?sv=');
    </script>
</svelte:head>

<!-- Google Tag Manager (noscript) -->
<noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-WQBV9P4V"
    height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
    <!-- End Google Tag Manager (noscript) -->
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
        margin: 0;
    }

    /* @media screen and (min-width:1080px) {
        :global(body) {
            background-color: white;
            margin: 0;
        }
    } */

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
       transition: opacity 0.2s ease-out;
    }

    :global(.btn-main) {
        background-color: var(--purple);
        padding: 24px;
        color: white;
        border: none;
        border: 2px solid var(--purple);
        text-decoration: none;
    }

    :global(.btn-sec) {
        border: 2px solid var(--purple);
        background-color: #FFF;
        color: var(--purple);
        text-decoration: none;
    }

    :global(.btn-special) {
        border: 2px solid var(--purple);
        background: #00E8FC;
        color: var(--purple);
        padding: 20px 29px 20px 24px;
        text-decoration: none;
    }

    :global(.btn-special::before) {
        content: url(/images/sparkle.svg);
        width: auto;
        height: 80%;
        display: flex;
        justify-content: center;
        align-items: center;
        padding-top: 5px;
        padding-right: 5px;
        transform: scale(1) rotate(0deg);
        transition: transform 0.1s ease-out;
    }

    :global(.btn-special:hover::before) {
        transform: scale(1.15) rotate(-10deg);
    }

    :global(.purple-text) {
        color: var(--purple);
    }

    :global(.text-center) {
        text-align: center;
    }
</style>