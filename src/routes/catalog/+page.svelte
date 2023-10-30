
<script>
	import Footer from './../../lib/components/footer.svelte';
	import Nav from './../../lib/components/Nav.svelte';
    let pions = ['pions', 'pions2', 'pions3'];
</script>


<div class="full-page">
    <Nav/>
    <div class="top-cont">
        <section>
            <p>Pions customisés</p>
            <div class="pions-cont">
                {#each pions as pion}
                    <div class="img-cont">
                        <img src="images/{pion}.svg" alt="">
                    </div>
                {/each}
                <div class="discover-btn">
                    <span>En découvrir d'autres</span>
                    <img src="images/arrow-right-white.svg" alt="">
                </div>
            </div>
        </section>

        <section>
            <p>Plateaux de jeux populaires</p>
            <div class="boards-cont">
                {#each {length: 4} as _,i}
                    <div class="img-cont">
                        <img src="images/board-{i+1}.png" alt="">
                    </div>
                {/each}
                <div class="discover-btn">
                    <span>En découvrir d'autres</span>
                    <img src="images/arrow-right-white.svg" alt="">
                </div>
            </div>
        </section>
    </div>
</div>
<Footer/>


<style>

    .full-page {
        margin: 14px 30px;
        min-height: 100vh;
    }
    @media screen and (max-width: 1079px) {
        .discover-btn {
            height: 80px !important;
            border-radius: 12px;
            margin-bottom: 80px;
        }
    }

    @media screen and (max-width:849px) {
        .pions-cont, .boards-cont {
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }
        .top-cont {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }
        section {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }
        .pions-cont {
            gap: 28px;
        }
    }

    .top-cont {
        margin: 30px;
    }

    .discover-btn {
        min-height: 100%;
        padding: 0px 60px;
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: var(--purple);
        color: white;
        font-weight: 500;
        font-size: 22px;
        border-radius: 20px;
        cursor: pointer;
        gap: 14px;
    }
    .discover-btn > img {
        position: relative;
        left: 0px;
        transition: all 0.2s ease-out;
    }
    .discover-btn:hover > img {
        position: relative;
        left: 6px;
    }

    p {
        font-size: 24px;
        font-weight: 600;
    }
    .pions-cont {
        padding-bottom: 50px;
    }
    .pions-cont, .boards-cont {
        display: flex;
    }
    .img-cont {
        width: 200px;
        height: 200px;
    }
    .img-cont img {
        object-fit: contain;
        width: 100%;
        height: 100%;
    }
    .boards-cont {
        gap: 30px;
    }
    .boards-cont > .img-cont {
        border-radius: 14px;
        overflow: hidden;
        border: 1px solid rgba(0, 0, 0, 0.16);
    }
    .boards-cont img {
        object-fit: cover;
    }
</style>