<script>
	import Footer from './../../lib/components/footer.svelte';
	import Nav from './../../lib/components/Nav.svelte';
    import { onMount } from "svelte";


    let orderConfirmed = false;
    let savedOrder;
     onMount(() => {
        let storedOrderConfirmed = localStorage.getItem('orderConfirmed');
        let storedOrder = localStorage.getItem('order');
        if (storedOrderConfirmed) {
            orderConfirmed = JSON.parse(storedOrderConfirmed);
        }
        if (storedOrder) {
            savedOrder = JSON.parse(storedOrder);
        }
    });
</script>


<div class="full-page">
    <Nav/>
    <div class="page">
        <div class="pp">
            <div class="pp-img-cont">
                <img src="" alt="" class="pp-img">
            </div>
            <p class="name">Rob LAUCHON</p>
        </div>
        <div class="bot-sections">
            <div class="orders">
                <p class="orders-title">Mes commandes</p>
                <div>
                    {#if orderConfirmed}
                        <div class="titles line">
                            <p>N° de commande</p>
                            <p>Statut</p>
                        </div>
                        <div class="order-line line">
                            <p>754</p>
                            <p>En cours de production</p>
                        </div>
                    {:else}
                        <a class="btn-main" href="/quizz">
                            Passer une commande
                        </a>
                    {/if}
                </div>   
            </div>
            <div class="configs">
                <div class="configs-list">
                    <p class="configs-title">Mes créations</p>
                </div>
                <div>
                    {#if savedOrder != null}
                        <div class="titles line">
                            <p>Plateau</p>
                            <p>Pions</p>
                        </div>
                        <div class="order-line line">
                            <div class="board-img">
                                <img src="images/board-{savedOrder.board}.png" alt="">
                            </div>
                            <div class="pawns-img">
                                <img src="images/pions.svg" alt="">
                            </div>
                        </div>
                    {:else}
                        <a class="btn-main" href="/quizz">
                            Passer une commande
                        </a>
                    {/if}
                </div>
            </div>    
        </div>
    </div>    
</div>
<Footer/>


<style>
    .full-page {
        margin: 14px 30px;
    }
    .page {
        min-height: 76vh;
    }
    .btn-main {
        width: max-content;
    }
    .selected-games {
        display: flex;
        flex-direction: column;
        gap: 10px;
        justify-content: center;
    }
    .selected-games > p {
        font-weight: 500;
    }

    .board-img, .pawns-img {
        width: 120px !important;
        height: 120px !important;
        margin-right: 80px;
        overflow: hidden;
        border-radius: 6px;
    }
    .board-img > img, .pawns-img > img {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }
    .pawns-img > img {
        object-fit: contain !important;
    }

    .bot-sections {
        display: flex;
        flex-direction: row;
        gap: 70px;
    }

    @media screen and (max-width:1280px) {
        .bot-sections {
            flex-direction: column;
        }
        .board-img > img {
            object-fit: contain !important;
        }
        .pp {
            flex-direction: column !important;
            align-items: center;
        }
        .pp-img-cont {
            width: 140px !important;
            height: 140px !important;
        }
    }

    @media screen and (max-width:460px) {
        .bot-sections {
            flex-direction: column;
        }
        .board-img > img {
            object-fit: contain !important;
        }
        .pp-img-cont {
            width: 120px !important;
            height: 120px !important;
        }
    }

    .bot-sections > * {
        padding-top: 30px;
    }

    .page {
        padding: 50px;
    }

    .pp {
        display: flex;
        flex-direction: row;
    }
    .name {
        padding-left: 18px;
        font-size: 26px;
        font-weight: 600;
    }
    .pp-img-cont {
        width: 160px;
        height: 160px;
        background-color: rgb(194, 194, 194);
        border-radius: 1000px;
        display: flex;
        justify-content: center;
        align-items: center;
        cursor: pointer;
    }
    .pp-img-cont::after {
        content: "Changer ma photo de profil";
        text-align: center;
        color: transparent;
        transition: all 0.2s ease-out;
    }
    .pp-img-cont:hover::after {
        color: black;
    }
    .orders {
        max-width: 400px;
    }
    .orders-title, .configs-title {
        font-weight: 600;
        font-size: 26px;
    }
    .titles {
        padding: 0px 10px;
        padding-bottom: 10px;
    }
    .titles p {
        font-size: 14px;
        font-weight: 500;
        color: var(--purple);
    }
    .line {
        display: flex;
        flex-direction: row;
    }
    .line >* {
        width: 200px;
    }
    .line p {
        margin: 0;
    }
    .line:not(.titles) {
        background-color: #9B5DE5;
        padding: 10px 10px;
        border-radius: 8px;
    }
    .line:not(.titles) > p {
        font-weight: 600;
    }
</style>