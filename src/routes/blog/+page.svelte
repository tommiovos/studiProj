<script>
	import Footer from './../../lib/components/footer.svelte';
    import Nav from "../../lib/components/Nav.svelte";

    let blogPosts = [
        {
            title: "Le développement du système BrainBud",
            text: "",
            img: "blog1.png",
        },
        {
            title: "Comme l'IA peut améliorer l'apprentissage",
            text: "Brainbud",
            img: "blog2.png",
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
    <div class="blogs-cont">
        {#each blogPosts as blogPost}
            <div class="blog" style="background: url(images/{blogPost.img})">
                <p>{blogPost.title}</p>
            </div>
        {/each}
    </div>
</div>
<Footer/>

<style>
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