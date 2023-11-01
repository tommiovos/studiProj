<script>
    import { onMount } from "svelte";

    let openMobileNav = false;

    function toggleNav() {
        let mobileNav = document.getElementById('mobileNav');
        if (!openMobileNav) {
            let newNavWidth = window.innerWidth - 70;
            mobileNav.style.width = newNavWidth + "px";    
        }
        else {
            mobileNav.style.width = "60px";
        }
        
        openMobileNav = !openMobileNav;
    }

    function onResize() {
        if (openMobileNav) {
            let mobileNav = document.getElementById('mobileNav');
            if (mobileNav) {
                let newNavWidth = window.innerWidth - 70;
                mobileNav.style.width = newNavWidth + "px";    
            }
        }
    }

    onMount(() => {
        window.addEventListener('resize', onResize);

        return(() => {
            window.removeEventListener('resize', onResize);
        })
    })
</script>

<div class="nav-cont">
    <nav>
        <a href="/">
            <div class="logos">
                <img class="logo-purple" src="images/logo-purple.svg" alt="">
                <img class="logo-white" src="images/logo-white.svg" alt="">
            </div>    
        </a>
        
        <ul>
            <li><a href="/">Accueil</a></li>
            <li><a href="/quizz">Créer un jeu</a></li>
            <li><a href="/blog">Blog</a></li>
            <li><a href="/catalog">Catalogue</a></li>
        </ul>
    </nav>  
    
    <div class="other-links-pc">
        <div><a href="/account"><img src="images/account.svg" alt=""></a></div>
        <div><a href="/account"><img src="images/cart.svg" alt=""></a></div>
    </div>

    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <div class="links-mobile {openMobileNav ? 'open' : ''}" id="mobileNav">
        <div class="hidden-links">
            <a href="/">
                <img src="images/home.svg" alt="">
            </a>
            <a href="/quizz">
                <img src="images/create-game.svg" alt="">
            </a>
            <a href="/account">
                <img src="images/account-white.svg" alt="">
            </a>
            <a href="/">
                <img src="images/cart-white.svg" alt="">
            </a>
        </div>
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <div class="burger-img-cont" on:click={() => toggleNav()}>
            <img src="images/burger.svg" alt="">    
        </div>
    </div>
</div>



<style>

    .links-mobile {
        position: fixed;
        bottom: 26px;
        right: 20px;
        display: flex;
        width: 60px;
        height: 55px;
        padding: 10px 13px;
        align-items: center;
        justify-content: flex-start;
        gap: 10px;
        flex-shrink: 0;
        border-radius: 26px;
        border: 2px solid #FFF;
        background-color: #1B065E;
        transition: all 0.25s ease-out;
        overflow: hidden;
        z-index: 900;
    }

    .hidden-links {
        padding-left: 20px;
        display: flex;
        flex-direction: row;
        align-items: center;
        opacity: 0;
        transition: opacity 0.15s ease-out;
        justify-content: space-between !important;
        gap: 0px !important;
         width: 64%;
    }

    .links-mobile.open > .hidden-links {
        opacity: 1;
    }

    .burger-img-cont {
        position: absolute;
        right: 9px;
        height: 55px;
    }
    .burger-img-cont > img {
        height: 100%;
        object-fit: contain;
    }

    .nav-cont {
        width: 100%;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .other-links-pc {
        margin: 30px;
        display: flex;
        gap: 12px
    }

    .logo-white {
        height: 50px;
    }
    nav {
        display: flex;
        padding: 28px 84px 28px 44px;
        align-items: center;
        gap: 60px;
        flex-shrink: 0;
        border-radius: 30px;
        background-color: #1B065E;
        margin: 30px;
        width: max-content;
    }

    ul {
        display: flex;
        gap: 30px;
        list-style-type: none;
    }

    li {
        font-size: 18px;
        font-weight: 400;
    }

    a {
        text-decoration: none;
        color: white;
    }

    @media screen and (max-width: 849px) {
        .logo-white {
            display: none !important;
        }
        nav {
            background-color: transparent;
            padding: 0;
        }
        .other-links-pc {
            display: none !important;
        }
        ul {
            display: none !important;
        }
        .nav-cont {
            justify-content: center !important;
        }
    }

    @media screen and (max-width:400px) {
        .hidden-links > a {
            height: 28px;
        }
        .hidden-links img {
            height: 22px;
        }
        .hidden-links img:nth-child(2) {
            height: 20px;
        }
    }

    @media screen and (min-width: 850px) {
        .logo-purple {
            display: none !important;
        }

        .links-mobile {
            display: none !important;
        }
    }
    
</style>