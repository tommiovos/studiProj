<script>
	import Footer from './../lib/components/footer.svelte';
	import AsmodeeGames from './../lib/components/AsmodeeGames.svelte';
	import WhatUsersSay from './../lib/components/WhatUsersSay.svelte';
    import Nav from "../lib/components/Nav.svelte";
</script>


<div class="top-cont">
    <Nav/>
    <div class="hero">
        <div class="hero-img-cont">
            <img src="images/hero.png" alt="">
        </div>
        <section class="hero-desc">
            <h1>La technologie au service de l’apprentissage</h1>
            <p>Lorem ipsum dolor sit amet consectetur. Ut sit quis sagittis interdum bibendum amet id lectus ut. Cursus habitant feugiat senectus senectus in id quis pellentesque. Pellentesque nulla nulla penatibus risus amet. Lacus semper dignissim sed vel faucibus proin quisque at nisl.</p>
            <a class="btn-main" style="margin-top:16px;" href="/quizz">Découvrir</a>
        </section>
    </div>
    <section class="feedback">
        <WhatUsersSay/>    
    </section>
    <section class="asmodee-games">
        <AsmodeeGames/>
    </section>
</div>
<Footer/>


<style>
    .hero {
        display: flex;
        flex-direction: column;
    }
    .hero-desc {
        padding: 16px 24px;
        display: flex;
        flex-direction: column;
        justify-content: center;
    }
    h1 {
        font-size: 30px;
        margin-bottom: 10px;
    }
    h1, p, span {
        text-align: center;
    }

    @media screen and (min-width:600px) {
        .hero-img-cont {
            height: 440px !important;
        }
    }

    @media screen and (min-width:1080px) {
        h1, p, span {
            text-align: left;
        }
        h1 {
            font-size: 36px !important;
        }
        .hero {
            flex-direction: row;
        }
        .hero-img-cont {
            margin-left: 30px;
            flex: 4;
            height: 600px !important;
            border-radius: 16px;
        }
        .hero-desc {
            flex: 3;
            justify-content: flex-start !important;
            padding-top: 0px !important;
            padding-left: 40px !important;
        }
        .hero-desc h1 {
            margin-top: 8px;
        }
        .top-cont {
            padding: 14px 30px !important;
        }
    }
    .hero-img-cont {
        width: 100%;
        height: 280px;
        overflow: hidden;
    }

    .hero-img-cont > img {
        height: 100%;
        width: 100%;
        object-fit: cover;
    }
</style>