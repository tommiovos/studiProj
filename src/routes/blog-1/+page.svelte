<script>
	import Footer from './../../lib/components/footer.svelte';
    import Nav from "../../lib/components/Nav.svelte";

    let blogPosts = [
        {
            title: "Comment l'IA peut améliorer l'apprentissage",
            text: "Brainbud",
            img: "blog2.png",
            link: "/blog-1"
        },
        {
            title: "Le développement du système BrainBud",
            text: "",
            img: "blog1.png",
        },
        {
            title: "BraindBud gagne le prix EduGame",
            text: "",
            img: "blog3.png",
        },
        {
            title: "Les cafés des jeux en pleine expansion !",
            text: "",
            img: "blog4.png",
        }
    ]
</script>


<div class="top-cont">
    <Nav/>
    <div class="main">
        <div class="blog-content">
            <div class="blog-hero">
                <img src="images/blog2.png" alt="">
            </div>
            <h1>Comment l'IA peut améliorer l'apprentissage</h1>
            <p>L'intelligence artificielle (IA), avec ses capacités innovantes et créatives, révolutionne divers secteurs, y compris l'éducation. Son rôle dans l'enrichissement des expériences d'apprentissage est à la fois fascinant et profondément impactant.</p>
            <h2>L'IA: Un mentor personnalisé</h2>
            <p>Imaginez un monde où chaque apprenant dispose d'un mentor personnel, ajustant le rythme et le style d'enseignement à ses besoins uniques. L'IA rend cela possible. Grâce à des algorithmes avancés, elle analyse les styles d'apprentissage des étudiants, adaptant les matériaux pédagogiques pour les rendre plus compréhensibles et engageants. Cette approche personnalisée assure que chaque étudiant peut apprendre de manière efficace, transformant les défis d'apprentissage en opportunités de croissance.</p>
            <h2>Réinvention de la matière éducative</h2>
            <p>Avec l'IA, le contenu d'apprentissage n'est plus statique et unidimensionnel. Elle permet la création de matériels interactifs et immersifs, tels que des simulations et des jeux éducatifs, rendant l'apprentissage plus attrayant et mémorable. De plus, l'IA peut générer des contenus pédagogiques dynamiques, qui évoluent pour refléter les dernières découvertes et tendances, gardant les étudiants au courant et engagés.</p>
            <h2>Évaluation et feedback en temps réel</h2>
            <p>L'un des aspects les plus cruciaux de l'apprentissage est le feedback (retour). L'IA peut fournir des évaluations et des retours instantanés, permettant aux étudiants de comprendre immédiatement leurs erreurs et de les corriger. Cela favorise un environnement d'apprentissage où les étudiants peuvent expérimenter et apprendre de leurs échecs sans crainte de jugement, encourageant une mentalité de croissance.</p>
            <h2>C’est quoi la suite ?</h2>
            <p>L'IA prépare les étudiants non seulement à réussir dans leurs études actuelles mais aussi à s'épanouir dans un avenir dominé par la technologie. En intégrant l'IA dans l'éducation, nous enseignons aux jeunes générations comment collaborer avec la technologie, penser de manière critique et résoudre des problèmes de manière innovante.</p>
            <h2>Conclusion</h2>
            <p>L'IA transforme l'éducation, non seulement en améliorant les méthodes d'enseignement et les matériaux d'apprentissage mais aussi en rendant l'éducation plus accessible et personnalisée. En adoptant l'IA, nous pouvons ouvrir la porte à des expériences d'apprentissage plus riches, plus inclusives et plus efficaces pour tout un chacun. L'avenir de l'éducation, stimulé par l'IA, est non seulement brillant mais également prometteur, ouvrant la voie à une ère d'apprentissage sans limites.</p>
        </div>
    </div>
</div>
<Footer/>

<style>
    @media screen and (max-width:1080px) {
        h1, h2, p {
            text-align: center;
        }
        h1 {
            width: auto !important;
        }
    }
    h1 {
        color: var(--purple);
        background-color: #00E8FC;
        width: max-content;
        padding: 20px;
        border-radius: 10px;
    }
    .blog-hero {
        width: 100%;
        overflow: hidden;
        height: 300px;
        border-radius: 10px;
    }
    .blog-hero img {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }
    .top-cont {
        padding: 14px 30px;
        height: max-content;
    }
    .blogs-cont {
        margin: 30px;
        display: grid;
        grid-template-columns: 3fr 2fr;
        grid-template-rows: 1fr 1fr 1fr;
        grid-template-areas: 
            'b1 b2'
            'b1 b3'
            'b1 b4'
        ;
        min-height: 74vh;
        max-height: 1000px;
        height: 100%;
        gap: 30px;
        overflow: hidden;
    }
    .blog {
        background-color: red;
        border-radius: 20px;
        padding: 30px;
        background-size: cover !important;
        max-width: 100%;
        overflow: hidden;
    }
    .blog:hover > p {
        transform: translateY(-10px) scale(1.02);
    }
    .blog:first-child {
        cursor: pointer;
        text-decoration: none;
    }
    .blog > p {
        display: flex;
        font-size: 24px;
        font-weight: 600;
        margin: 0;
        padding: 12px;
        width: max-content;
        background-color: #00E8FC;
        border-radius: 12px;
        max-width: 80%;
        color: var(--purple);
        transition: 0.13s ease-out;
        transform: translateY(0px) scale(1);
    }
    .blog:nth-child(1) > p {
        font-size: 30px;
        padding: 20px;
    }

    .blog:nth-child(1) {
        grid-area: b1;
        padding: 40px;
    }
    .blog:nth-child(2) {
        grid-area: b2;
    }
    .blog:nth-child(3) {
        grid-area: b3;
    }
    .blog:nth-child(4) {
        grid-area: b4;
    }

    @media screen and (max-width:849px) {
        .blogs-cont {
            grid-template-columns: 1fr !important;
            grid-template-rows: 400px 280px 280px 280px !important;
            grid-template-areas: 
                'b1'
                'b2'
                'b3'
                'b4'
                !important
            ;
            height: auto !important;
            max-height: unset !important;
        }
        .blog {
            padding: 20px !important;
        }
        .blog > p {
            font-size: 20px !important;
        }
        .blog:first-child > p {
            font-size: 24px !important;
        }
    }
</style>