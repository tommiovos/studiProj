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
            <p>
                Dans un monde où la technologie évolue à grande vitesse, 
                BrainBud fait le lien entre l'apprentissage et le jeu.
                Notre intelligence artificielle, créée avec l'aide de professionnels de l'éducation, analyse les réponses à un questionnaire ludique et personnalise 
                l'expérience de chaque joueur, en recommandant des modules adaptés à ses besoins 
                et à ses intérêts. Chaque module est conçu pour stimuler l'intellect tout en offrant 
                un divertissement captivant. Alors, prêts à embarquer dans l'aventure passionnante qu'est l'apprentissage avec BrainBud ?
            </p>
            <a class="btn-special" style="margin-top:16px;" href="/quizz">Découvrir</a>
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
    .hero-desc p {
        font-size: 18px;
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