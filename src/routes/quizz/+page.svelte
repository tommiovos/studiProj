<script>
	import CheckboxGroup from './../../lib/components/CheckboxGroup.svelte';
    import { quintInOut } from "svelte/easing";
    import Question from "../../lib/components/Question.svelte";
    import { fade, slide } from 'svelte/transition';
    import { onMount } from "svelte";

    let maths = "Mathématiques et résolution de problèmes";
    let french = "Français";
    let en = "Anglais";
    let history = "Histoire";
    let geo = "Géographie";


    function transitionIn(node, params) {
        return {
            easing: params.easing,
            duration: params.duration,
            delay: params.delay,
            css: (t, u) => `transform: translateX(${u * params.distance}px) scale(${t/2 + 0.5}); opacity: ${t}`,
        }
    }

    function transitionOut(node, params) {
        return {
            easing: params.easing,
            duration: params.duration,
            delay: params.delay,
            css: (t, u) => `transform: translateX(${-u * params.distance}px) scale(${t/2 + 0.5}); opacity: ${t}`,
        }
    }

    let quizzList = [
        {
            subject: maths,
            question: "Combien font 8 x 9 ?",
            answers:
            [
                {
                    text: 68,
                    isRight: false
                },
                {
                    text: 72,
                    isRight: true
                },
                {
                    text: 81,
                    isRight: false
                }
            ],
            textSize: "lg"
        },
        {
            subject: maths,
            question: "Combien font 13 x 6 ?",
            answers:
            [
                {
                    text: 78,
                    isRight: true
                },
                {
                    text: 87,
                    isRight: false
                },
                {
                    text: 74,
                    isRight: false
                }
            ],
            textSize: "lg"
        },
        {
            subject: maths,
            question: "Si un un rond bleu pèse 3Kg, combien pèse un carré jaune ?",
            image: "images/scale_image.svg",
            answers:
            [
                {
                    text: "6Kg",
                    isRight: true
                },
                {
                    text: "8Kg",
                    isRight: false
                },
                {
                    text: "4Kg",
                    isRight: false
                }
            ],
            textSize: "md"
        },



        {
            subject: french,
            question: "Comment se conjugue le verbe \“pouvoir\” à la 1ère personne du pluriel au passé simple ?",
            answers:
            [
                {
                    text: "Nous pourrons",
                    isRight: false
                },
                {
                    text: "Nous pûmes",
                    isRight: true
                },
                {
                    text: "Nous pouvions",
                    isRight: false
                }
            ],
            textSize: "sm"
        },


        {
            subject: en,
            question: "Comment dit-on \“fauteil roulant\” en anglais ?",
            answers:
            [
                {
                    text: "Wheelchair",
                    isRight: true
                },
                {
                    text: "Rollchair",
                    isRight: false
                },
                {
                    text: "Chairroller",
                    isRight: false
                }
            ],
            textSize: "md"
        },
        {
            subject: en,
            question: "Que signifie \“Carpet\” ?",
            answers:
            [
                {
                    text: "Rideaux",
                    isRight: false
                },
                {
                    text: "Tapis",
                    isRight: true
                },
                {
                    text: "Crapeau",
                    isRight: false
                }
            ],
            textSize: "md"
        },


        {
            subject: history,
            question: "En quelle année fût instaurée la 5ème république en France ?",
            answers:
            [
                {
                    text: "1968",
                    isRight: false
                },
                {
                    text: "1960",
                    isRight: false
                },
                {
                    text: "1958",
                    isRight: true
                }
            ],
            textSize: "md"
        },
        {
            subject: history,
            question: "Quel traité est à l’origine de l’Union européenne ?",
            answers:
            [
                {
                    text: "Le traité de Lisbonne",
                    isRight: false
                },
                {
                    text: "Le traité de Maastricht",
                    isRight: true
                },
                {
                    text: "Le traité d’Amsterdam",
                    isRight: false
                }
            ],
            textSize: "sm"
        },


        {
            subject: geo,
            question: "Quelle est la capitale du Pérou ?",
            answers:
            [
                {
                    text: "La Paz",
                    isRight: false
                },
                {
                    text: "Piura",
                    isRight: false
                },
                {
                    text: "Lima",
                    isRight: true
                }
            ],
            textSize: "sm"
        },
        {
            subject: geo,
            question: "De quel continent fait partie l'Arménie ?",
            answers:
            [
                {
                    text: "L'Asie",
                    isRight: true
                },
                {
                    text: "L'Afrique",
                    isRight: false
                },
                {
                    text: "L'Europe",
                    isRight: false
                }
            ],
            textSize: "sm"
        },
    ]

    
    function getQuizzSections() {
        const subjectMap = new Map();

        // Grouping array indices (as IDs) by subject
        quizzList.forEach((item, index) => {
            if (!subjectMap.has(item.subject)) {
                subjectMap.set(item.subject, []);
            }
            subjectMap.get(item.subject).push(index); // Use index as the ID
        });

        // Converting the map to the desired array format
        return Array.from(subjectMap, ([subject, ids]) => ({ subject, ids }));
    }

    function summarizeAnswers(arr) {
        const answerSummary = new Map();

        arr.forEach(({ subject, isRight }) => {
            if (!answerSummary.has(subject)) {
            answerSummary.set(subject, { rightAnswers: 0, wrongAnswers: 0 });
            }
            isRight ? answerSummary.get(subject).rightAnswers++ : answerSummary.get(subject).wrongAnswers++;
        });

        return Array.from(answerSummary, ([subject, counts]) => ({
            subject,
            rightAnswers: counts.rightAnswers,
            wrongAnswers: counts.wrongAnswers,
            ratio: counts.wrongAnswers === 0 ? Infinity : counts.rightAnswers / counts.wrongAnswers
        }));
    }
    
    let currentQuestionId = null;
    let currentConfigStep = null;
    let answeredList = [];

    let quizzSections = [];



    let games = [
        {
            recommendedFor: [french],
            title: "Grammaire renforcé",
            text: "Jeux de renforcement du français comportant ......."
        },
        {
            recommendedFor: [maths],
            title: "Réflexion",
            text: "Jeux pour travailler la logique et la résolution de problèmes ......."
        },
        {
            recommendedFor: [maths],
            title: "Tables de multiplications",
            text: "Jeux pour travailler la logique et la résolution de problèmes ......."
        },
        {
            recommendedFor: [geo, history],
            title: "Renforcement en Géographie",
            text: "Jeux de stratégie basé sur la carte du monde réelle pour apprendre sa Géographie !"
        },
        {
            recommendedFor: [history],
            title: "Quizzs histoire",
            text: "Des questions sur l'histoire pour s'améliorer en uin rien de temps !"
        },
        {
            recommendedFor: [en],
            title: "Jeux en anglais",
            text: "Jeux pour renforcer ses connaissances dans la langue de Shakespear"
        }
    ];


    function findWorstPerformingSubjects(arr) {
        const subjectSummary = new Map();

        arr.forEach(({ subject, isRight }) => {
            if (!subjectSummary.has(subject)) {
            subjectSummary.set(subject, { rightAnswers: 0, wrongAnswers: 0 });
            }
            isRight ? subjectSummary.get(subject).rightAnswers++ : subjectSummary.get(subject).wrongAnswers++;
        });

        const subjectsSorted = Array.from(subjectSummary)
            .map(([subject, counts]) => ({
            subject,
            ratio: counts.wrongAnswers === 0 ? Infinity : counts.rightAnswers / counts.wrongAnswers
            }))
            .sort((a, b) => a.ratio - b.ratio);


        let result = subjectsSorted.slice(0, 3).map(item => item.subject);
        console.log(result);
        // Selecting the top 3 subjects with the worst ratio
        return result
    }


    function recommendedGames(data, games) {
        const worstSubjects = findWorstPerformingSubjects(data);

        // Score each game based on the revised criteria
        const scoredGames = games.map(game => {
            let score = 0;
            game.recommendedFor.forEach(subject => {
            if (worstSubjects.includes(subject)) {
                score += 1; // Add 1 point if the subject is in the worst-performing list
            } else {
                score -= 0.5; // Subtract 0.5 points if it's not
            }
            });

            return { ...game, score };
        });

        // Sort games by score in descending order and select the top 3
        scoredGames.sort((a, b) => b.score - a.score);
        return scoredGames.slice(0, 3);
    }


    onMount(() => {
        quizzSections = getQuizzSections();
        let storedAnswers = localStorage.getItem('answers');
        let storedProgressId = localStorage.getItem('currStep');
        if (storedAnswers) {
            formDone = true;
            doQuizz = true;
            answeredList = JSON.parse(storedAnswers);
        }
        if (storedProgressId) {
            currentQuestionId = JSON.parse(storedProgressId);
        }
        else {
            currentQuestionId = 0;
        }
        loaded = true;
    });


    let loaded = false;
    let formDone = false;
    let doQuizz = false;

    let group = 1;
	let selection = [];

    let checkboxes = [
        {id: "maths", text: "Mathématiques"},
        {id: "french", text: "Français"},
        {id: "science", text: "Sciences"},
        {id: "dextrity", text: "Dextérité"},
        {id: "problems", text: "Résolution de problèmes"},
        {id: "english", text: "Anglais"},
        {id: "info", text: "Informatique"},
    ];

    function checkboxChanged(checkId, event) {
        const index = selection.indexOf(checkId);
        if (event.target.checked) {
            selection =[...selection, checkId];
        }
        else if (index > -1) {
            selection.splice(index, 1);
            selection = selection;
        }

        console.log(selection);
    }

    let levelValue=1;

    function getLevel(val) {
        switch(val) {
            case 1: return "Petite section";
            case 2: return "Moyenne section";
            case 3: return "Grande section";
            case 4: return "CP";
            case 5: return "CE1";
            case 6: return "CE2";
            case 7: return "CM1";
            case 8: return "CM2";
            case 9: return "6eme";
            case 10: return "5eme";
            case 11: return "4eme";
            case 12: return "3eme";
            case 13: return "Seconde";
            case 14: return "Première";
            case 15: return "Terminale";
            case 16: return "Licence";
            case 17: return "Master";
        }
    }
</script>


{#if loaded}
<div class="top-cont">
    {#if formDone == false}
        <div class="form-steps-cont" in:transitionIn={{easing:quintInOut, distance: 500, duration: 550}} out:transitionOut={{easing:quintInOut, distance: 500, duration: 550}}>
            <h1 class="purple-text text-center">Bienvenue sur le configurateur de jeu BrainBud !</h1>
            <p class="text-center" style="min-height: 200px; padding-bottom: 16px; padding-top:12px;">C’est ici que vous pourrez lorem ipsum dolor sit amet consectetur. Tristique eleifend non ultrices in nisi pretium tortor leo. Elementum vitae metus ullamcorper condimentum vulputate. Libero enim enim sed natoque tempor diam leo ac. Luctus aliquet tellus semper in tellus nulla. Scelerisque suspendisse metus diam est volutpat aliquet aliquet purus sit. Mattis auctor hendrerit in eu neque adipiscing lacinia. Sit in volutpat molestie quis.</p>
            
            <div class="buttons">
                <button class="btn-sec">Retour</button>
                <button class="btn-main" on:click={() => {formDone = true;}}>Définir le parcours</button>
                <button class="btn-special" on:click={() => {doQuizz = true; formDone = true;}}>Passer le test</button>
            </div>
        </div>

    {:else if currentQuestionId != null && doQuizz}
        <div class="quizz-cont" in:transitionIn={{easing:quintInOut, distance: 500, duration: 550, delay: 100}} out:transitionOut={{easing:quintInOut, distance: 500, duration: 550}}>
            <div class="quizz-part">
                <div class="subjects-cont">
                    {#each quizzSections as quizzSection}
                        {#if quizzSection.ids.includes(currentQuestionId)}
                            <h1 in:transitionIn={{easing:quintInOut, distance: 500, duration: 550, delay: 320}} out:transitionOut={{easing:quintInOut, distance: 500, duration: 550, delay: 300}} class="subject">{quizzSection.subject}</h1>
                        {/if}
                    {/each}    
                </div>
                
                <div class="questions-cont">
                    {#each quizzList as quizz, i}
                        {#if currentQuestionId == i}
                            <div in:transitionIn={{easing:quintInOut, distance: 500, duration: 550, delay: 320}} out:transitionOut={{easing:quintInOut, distance: 500, duration: 550, delay: 300}}>
                                <Question bind:currentQuestionId bind:answeredList subject={quizz.subject} image={quizz.image == null ? "" : quizz.image } question={quizz.question} answers={quizz.answers} textSize={quizz.textSize}/>
                            </div>
                        {/if}
                    {/each}        
                </div>
                
            </div>
        </div>

        {#if currentQuestionId == quizzList.length}
            <div class="results" in:transitionIn={{easing:quintInOut, distance: 500, duration: 550, delay: 320}} out:transitionOut={{easing:quintInOut, distance: 500, duration: 550, delay: 300}}>
                <p class="results-title">Notre sélection recommandée</p>
                <div class="recommended-subjects">
                    {#each recommendedGames(answeredList, games) as recommendedGame,i}
                        <div class="reco-game-card r-{i}">
                            <p>{recommendedGame.title}</p>    
                            <p>{recommendedGame.text}</p>
                        </div>
                    {/each}
                </div>
                
            </div>
        {/if}

    {:else}
        <div in:transitionIn={{easing:quintInOut, distance: 500, duration: 550, delay: 100}} out:transitionOut={{easing:quintInOut, distance: 500, duration: 550, delay: 300}}>
            <div class="form-page-cont">
                <div class="checkboxes-cont">
                    {#each checkboxes as checkbox}
                        <label for={checkbox.id} class="checkbox {selection.includes(checkbox.id) ? 'checked' : 'test'}">
                                <input type="checkbox" name="" id="{checkbox.id}" value={checkbox.id} on:change={(event) => checkboxChanged(checkbox.id, event)}>
                                {checkbox.text}
                        </label>
                    {/each}    
                </div>

                <div class="level-slider-cont">
                    <label for="level" class="level-title">Niveau visé</label>
                    <p class="level-value">{getLevel(levelValue)}</p>
                    <input class="level-range" type="range" name="level" id="level" bind:value={levelValue} min="1" max="17">    
                </div>
                
                <div style="display: flex; flex-direction: column; gap: 20px;">
                    <button class="btn-sec">Retour</button>
                    <button class="btn-main">Confirmer ma sélection</button>    
                </div>    
            </div>
        </div>
    {/if}    
</div>
{/if}




<style>

    .form-page-cont {
        display: flex;
        flex-direction: column;
        gap: 44px;
        padding: 20px;
    }

    .level-slider-cont {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        gap: 16px;
    }

    .level-title {
        color: #242424;
        font-family: Inter;
        font-size: 24px;
        font-style: normal;
        font-weight: 600;
        line-height: normal;
        margin: 0;
    }

    .level-value {
        color: var(--purple);
        font-family: Inter;
        font-size: 24px;
        font-style: normal;
        font-weight: 600;
        line-height: normal;
        margin: 0;
    }

    .level-range {
        width: 80%;
        height: 4px;
    }

    .checkboxes-cont {
        display: flex;
        justify-content: center;
        align-items: center;
        align-content: center;
        gap: 12px;
        align-self: stretch;
        flex-wrap: wrap;
    }

    .checkbox {
            background-color: white;
            border-radius: 5px;
            display: flex;
            padding: 14px;
            justify-content: center;
            align-items: center;
            gap: 7px;
            width: max-content;
            color: var(--purple);
            border: 1px solid var(--purple);
            accent-color: var(--purple);
            font-weight: 500;
            transition: background-color 0.1s ease-out, color 0.1s ease-out;
            /* prevent select */
            -webkit-user-select: none; /* Safari */
            -ms-user-select: none; /* IE 10 and IE 11 */
            user-select: none; /* Standard syntax */
        }

    .checkbox.checked {
        background-color: var(--purple);
        color: white;
    }

    .level-range {
        accent-color: var(--purple);
    }

    .top-cont {
        display: grid;
        grid-template-columns: 1fr;
        grid-template-rows: 1fr;
    }

    .top-cont > * {
        grid-row: 1/2;
        grid-column: 1/2;
    }


    .form-steps-cont {
        padding: 20px;
    }

    .buttons {
        display: flex;
        flex-direction: column;
        gap: 10px;
    }

    .buttons>*:first-child {
        margin-bottom: 16px;
    }

    .quizz-cont {
        display: grid;
        grid-template-columns: 1fr;
        grid-template-rows: 1fr;
    }

    .quizz-cont, .results {
        padding: 20px;
    }

    .quizz-cont > * {
        grid-row: 1/2;
        grid-column: 1/2;
    }

    .questions-cont {
        display: grid;
        grid-template-columns: 1fr;
        grid-template-rows: 1fr;
    }
    .questions-cont > * {
        grid-row: 1/2;
        grid-column: 1/2;
    }

    .subjects-cont {
        display: grid;
        grid-template-columns: 1fr;
        grid-template-rows: 1fr;
    }

    .subjects-cont > * {
        grid-row: 1/2;
        grid-column: 1/2;
    }

    .subject {
        height: 60px;
        text-align: center;
        font-size: 24px;
        color: var(--purple);
    }

    .results-title {
        text-align: center;
        font-size: 24px;
        font-weight: 600;
        color: var(--purple);
    }

    .recommended-subjects {
        perspective: 1000px;
        display: flex;
        flex-direction: column;
        gap: 24px;
    }

    .reco-game-card {
        perspective: 1000px;
        transform-style: preserve-3d;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        text-align: center;
        background-color: white;
        border-radius: 24px;
        border-radius: 14px;
        border: 1px solid #1B065E;
        background: white;
        box-shadow: -8px 10px 0px 0px #1B065E;
        padding: 16px;
        gap: 12px;
        animation: appear-from-bot 0.45s ease-out backwards;
    }

    .r-0 {
        animation-delay: 1.15s;
    }
    .r-1 {
        animation-delay: 1.3s;        
    }
    .r-2 {
        animation-delay: 1.45s;   
    }

    @keyframes appear-from-bot {
        0% {
            transform: translateY(100px) rotateY(90deg);
            opacity: 0;
        }
        50% {
            opacity: 0.7;
        }
        100% {
            opacity: 1;
            transform: translateY(0px) rotateY(0deg);
        }
    }

    .reco-game-card > p:first-child {
        font-weight: 600;
        font-size: 20px;
        margin: 0;
    }
    .reco-game-card > p:not(:first-child) {
        margin: 0;
        font-weight: 500;
        font-size: 18px;
    }
</style>