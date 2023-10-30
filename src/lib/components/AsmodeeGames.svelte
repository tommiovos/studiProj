<script>
    let games = [
        {
            picture: "game1.jpg",
            name: "13 Mots",
        },
        {
            picture: "game2.png",
            name: "Dixit",
        },
        {
            picture: "game3.png",
            name: "Les aventuries du rail (Europe)",
        },
        // {
        //     picture: "game4.jpg",
        //     name: "Codex",
        // }
    ]
</script>

<div class="top-cont">
    <h2>Découvrez d'autres jeux Asmodee</h2>
    <div class="games-cont">
        {#each games as game}
            <div class="game">
                <p>{game.name}</p>
                <div class="img-cont">
                    <img src="images/{game.picture}" alt="jeu {game.name}">
                </div>
            </div>
        {/each}
        <div class="discover-btn">
            <span>En découvrir d'autres</span>
            <img src="images/arrow-right-white.svg" alt="">
        </div>
    </div>    
</div>


<style>
    .top-cont {
        padding: 36px 30px
    }
    @media screen and (max-width: 1079px) {
        h2 {
            font-size: 28px;
            text-align: center;
        }
        .top-cont {
            align-items: center;
        }
        .games-cont {
            flex-direction: column !important;
            align-items: center !important;
            justify-content: center !important;
        }
        .game {
            margin: auto;
        }
        .game p {
            text-align: center !important;
            height: auto !important;
        }
        .discover-btn {
            height: 80px !important;
            border-radius: 12px;
            margin-bottom: 80px;
        }
    }

    @media screen and (max-width: 500px) {
        .discover-btn img {
            display: none !important;
        }
    }

    h2 {
        font-size: 32px;
    }
    .games-cont {
        display: flex;
        flex-direction: row;
        gap: 60px;
    }
    .game {
        width: 260px;
        display: flex;
        flex-direction: column;
    }
    .game > p {
        margin: 0;
        margin-bottom: 18px;
        height: 50px;
        font-size: 22px;
        font-weight: 500;
    }
    .img-cont {
        width: 100%;
        height: 220px;
        overflow: hidden;
    }
    .img-cont img {
        width: 100%;
        height: 100%;
        object-fit: contain;
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
</style>