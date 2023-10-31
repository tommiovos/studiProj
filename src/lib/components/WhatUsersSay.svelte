<script>
    let userTexts = [
        {
            picture: "",
            name: "Mireille",
            age: 42,
            text: "“En tant que parent, je trouve ce jeu fantastique pour aider mes enfants à apprendre tout en s'amusant.”"
        },
        {
            picture: "",
            name: "Léa",
            age: 15,
            text: "“J'ai trouvé les jeux en anglais super utiles pour améliorer mon vocabulaire. C'est une manière cool de se préparer pour les examens d'anglais.”"
        },
        {
            picture: "",
            name: "Pierre",
            age: 22,
            text: "“J'ai utilisé des modules personnalisés dans ma classe et les résultats ont été impressionnants !”"
        },
        {
            picture: "",
            name: "Mehdi",
            age: 13,
            text: "“Le module de quizzs sur l'histoire est trop cool ! J'ai appris plein de choses sur les différentes civilisations.”"
        }
    ]
</script>

<div class="top-cont">
    <h2>Ce que nos utilisateurs en disent</h2>
    <div class="cards-cont">
        {#each userTexts as userText}
            <div class="card">
                <div class="img-cont">
                    <img src="images/{userText.picture}.svg" alt="">
                </div>
                <p class="name">{userText.name}</p>
                <p class="age">{userText.age} ans</p>
                <p class="text">{userText.text}</p>
            </div>
        {/each}
    </div>

</div>


<style>
    @media screen and (max-width: 1079px) {
        h2 {
            font-size: 28px;
            text-align: center;
        }
        .top-cont {
            align-items: center;
        }
        .cards-cont {
            flex-direction: column;
            align-items: center !important;
            justify-content: center !important;
        }
        .card {
            margin: auto;
        }

    }
    h2 {
        font-size: 32px;
    }
    .top-cont {
        padding: 36px 30px;
    }
    .cards-cont {
        display: flex;
        gap: 30px;
    }
    .img-cont {
        width: 40px;
        height: 40px;
        background-color: grey;
        border-radius: 100px;
        overflow: hidden;
    }
    .card {
        display: flex;
        padding: 18px 16px;
        flex-direction: column;
        align-items: center;
        align-self: stretch;
        border-radius: 14px;
        gap: 4px;
        border: 1px solid #1B065E;
        background-color: white;
        box-shadow: -8px 10px 0px 0px #1B065E;
        width: 200px;
    }
    .card > p {
        margin: 0;
    }
    .name {
        color: #000;
        font-size: 16px;
        font-style: normal;
        font-weight: 500;
        line-height: normal;
    }
    .age {
        color: #000;
        font-size: 14px;
        font-style: normal;
        font-weight: 500;
        line-height: normal;
        padding-bottom: 14px;
    }
    .text {
        text-align: center;
        font-weight: 500;
        font-size: 14px;
    }

</style>