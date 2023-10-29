<script>
    export let subject;
    export let question;
    export let answers;
    export let image;
    export let textSize;

    export let currentQuestionId;
    export let answeredList;

    let selectedAnswer = "";
    let wasAnswerPicked = false;

    function selectAnswer(answer) {
        selectedAnswer = answer.text;
        wasAnswerPicked = true;
        console.log(answeredList);
        answeredList.push({subject: subject, isRight: answer.isRight});
        currentQuestionId += 1;

        localStorage.setItem('answers', JSON.stringify(answeredList));
        localStorage.setItem('currStep', JSON.stringify(currentQuestionId));
    }

    function getTextSize(textSizeStr) {
        let size = "38px";
        if (textSizeStr == 'sm') {
            size = "38px";
        }
        else if (textSizeStr == "md") {
            size = "42px";
        }
        else if (textSizeStr == "lg") {
            size = "48px";
        }
        return size;
    }
</script>

<div class="question">
    <!-- <h1 class="question-title">{subject}</h1> -->
    <p class="question-text">{question}</p>
    {#if image}
        <div class="img-cont">
            <img src={image} alt="">    
        </div>
    {/if}
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div class="answers">
        {#each answers as answer}
            <!-- svelte-ignore a11y-interactive-supports-focus -->
            <div class="btn-wrapper" on:click={() => selectAnswer(answer)} role="button">
                <button class="{wasAnswerPicked ? (answer.isRight ? "right" : "false") : ""} {selectedAnswer == answer.text ? "selected" : ""}" style="padding: {image ? '26px 0px' : '40px 0px'}">
                    <span style={`font-size: ${getTextSize(textSize)}`}>
                        {answer.text}    
                    </span>
                </button>    
            </div>
        {/each}    
    </div>
</div>

<style>
    .question {
        width: 100%;
        height: 100%;
        max-height: 100%;
        overflow: hidden;
        display: grid;
        grid-template-columns: 1fr;
        grid-template-rows: auto 1fr;
    }
    .question > * {
        width: 100%;
        text-align: center;
    }

    /* .question-title {
        color: var(--purple);
        font-weight: 600;
        font-size: 24px;
    } */

    .question-text {
        font-weight: 600;
        font-size: 28px;
    }

    .answers {
        display: flex;
        flex-direction: column;
        gap: 16px;
    }

    button {
        box-sizing: border-box;
        padding: 50px 0px;
        width: 100%;
        border-radius: 24px;
        border: 2px solid var(--purple);
        background-color: white;
        color: var(--purple);
        transform: scale(1);
        transition: transform 0.08s ease-out, color 0.2s ease-out, background-color 0.2s ease-out, border 0.2s ease-out, padding 0.2s ease-out;
        font-weight: 600;
    }
    button:active {
        transform: scale(0.86);
    }

    button.right {
        border-radius: 24px;
        border: 2px solid #167733;
        background-color: #c3e9cf;
        color: black;
    }

    button.false {
        border-radius: 24px;
        border: 2px solid #C0212E;
        background-color: #f1cbcf;
        color: black;
    }

    button.selected {
        color: var(--purple) !important;
    }

    .img-cont {
        max-height: 180px;
        width: 100%;
        padding-bottom: 16px;
    }
    .img-cont > img {
        object-fit: contain;
        height: 100%;
    }
</style>