<script>
    let socialMediaLinks = [
        'twitter',
        'instagram',
        'tiktok',
    ];
</script>


<footer>
    <div class="social-media-bloc">
        <p>Nous retrouver</p>
        <div class="social-medias">
            {#each socialMediaLinks as socialMediaLink}
                <div class="img-cont">
                    <img src="images/{socialMediaLink}.svg" alt="">
                </div>
            {/each}    
        </div>    
    </div>
</footer>


<style>
    footer {
        left: 0;
        height: 100px;
        background-color: var(--purple);
        display: flex;
        justify-content: center;
        align-items: center;
    }
    p {
        margin: 0;
        padding-bottom: 10px;
        color: white;
        font-weight: 500;
    }
    .social-media-bloc {
        width: max-content;
    }
    .social-medias {
        display: flex;
        justify-content: center;
        gap: 10px;
    }
</style>