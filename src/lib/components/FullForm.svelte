<script>
	import CheckboxGroup from './CheckboxGroup.svelte';
    import { quintInOut } from "svelte/easing";
    import Question from "./Question.svelte";
    import { fade, slide } from 'svelte/transition';
    import { onMount, createEventDispatcher } from "svelte";

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
                    isRight: false
                },
                {
                    text: "8Kg",
                    isRight: false
                },
                {
                    text: "4Kg",
                    isRight: true
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
            title: "Grammaire renforcée",
            text: "Plongez dans l'univers fascinant de la langue française ! Ce module apporte une touche ludique à l'apprentissage de la grammaire, avec des jeux captivants qui renforcent les compétences linguistiques de manière amusante et interactive"
        },
        {
            recommendedFor: [maths],
            title: "Réflexion",
            text: "Aiguisez votre esprit avec des défis de logique et de réflexion ! Conçu pour stimuler l'intellect, ce module propose des puzzles, des casse-têtes et des énigmes qui garantissent des heures de réflexion intense et de plaisir."
        },
        {
            recommendedFor: [maths],
            title: "Tables de multiplications",
            text: "Maîtrisez les tables de multiplication comme jamais auparavant ! Ce module transforme l'apprentissage des multiplications en une aventure captivante, avec des jeux dynamiques et des activités engageantes qui rendent les mathématiques excitantes."
        },
        {
            recommendedFor: [geo, history],
            title: "Renforcement en Géographie",
            text: "Voyagez autour du globe depuis votre salon ! Ce module enrichit les connaissances géographiques à travers des jeux de stratégie basé sur la carte du monde pour vous captiver et éveiller vptre curiosité envers notre monde."
        },
        {
            recommendedFor: [history],
            title: "Quizzs histoire",
            text: "Revivez les moments clés de l'histoire ! Parcourez différentes époques à travers des quizzs captivants et des jeux de rôle historiques. Ce module offre une manière immersive et divertissante d'apprendre l'histoire mondiale."
        },
        {
            recommendedFor: [en],
            title: "Jeux en anglais",
            text: "Perfectionnez votre anglais tout en vous amusant ! Avec une variété de jeux et d'activités en anglais, ce module est parfait pour améliorer la compréhension et l'expression dans une ambiance ludique et éducative."
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
        // Selecting the top 3 subjects with the worst ratio
        return result
    }


    let recommendedGamesSaved;
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
        let result = scoredGames.slice(0, 3);
        recommendedGamesSaved = result;
        localStorage.setItem('recommendedGames', JSON.stringify(result));
        return result;
    }


    onMount(() => {
        quizzSections = getQuizzSections();
        let storedAnswers = localStorage.getItem('answers');
        let storedProgressId = localStorage.getItem('currStep');
        let recomendedGames_ = localStorage.getItem('recommendedGames');
        let storedFormStep = localStorage.getItem('formStep');
        let storedQuizzDone = localStorage.getItem('quizzDone');
        let storedFormStartDone = localStorage.getItem('formStartDone');
        let storedDoQuizz = localStorage.getItem('doQuizz');
        let storedSelectedBoard = localStorage.getItem('selectedBoard');
        let storedOrder = localStorage.getItem('order');
        let storedOrderConfirmed = localStorage.getItem('orderConfirmed');

        if (storedOrder) {
            order = JSON.parse(storedOrder);
        }
        if (storedOrderConfirmed) {
            orderConfirmed = JSON.parse(storedOrderConfirmed);
        }
        if (storedSelectedBoard) {
            selectedBoard = JSON.parse(storedSelectedBoard);
        }
        if (storedDoQuizz) {
            doQuizz = JSON.parse(storedDoQuizz);
        }
        if (storedFormStartDone) {
            formDone = JSON.parse(storedFormStartDone);
        }
        if (storedQuizzDone) {
            quizzDone = JSON.parse(storedQuizzDone);
        }
        if (storedFormStep) {
            formStep = JSON.parse(storedFormStep);
        }
        if (recomendedGames_) {
            recommendedGamesSaved = JSON.parse(recomendedGames_);
        }
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
    let quizzDone = false;
    let formStep = 0;

    let group = 1;
	let selection = [];
    let selectionGameTypes = [];

    let checkboxes = [
        {id: "maths", text: "Mathématiques"},
        {id: "french", text: "Français"},
        {id: "science", text: "Sciences"},
        {id: "problems", text: "Résolution de problèmes"},
        {id: "english", text: "Anglais"},
        {id: "info", text: "Informatique"},
    ];

    let checkboxes2 = [
        {id: "dexterity", text: "Jeux d'adresse"},
        {id: "strat", text: "Jeux de stratégie"},
        {id: "adventure", text: "Jeux d'aventure"},
        {id: "classicBoard", text: "Jeux de plateaux classiques"},
        {id: "quizz", text: "Quizz"},
    ];

    function checkboxChanged(checkId, event, checkboxListId) {
        if (checkboxListId == 0) {
            const index = selection.indexOf(checkId);
            if (event.target.checked) {
                selection = [...selection, checkId];
            }
            else if (index > -1) {
                selection.splice(index, 1);
                selection = selection;
            }    
        }
        else {
            const index = selectionGameTypes.indexOf(checkId);
            if (event.target.checked) {
                selectionGameTypes = [...selectionGameTypes, checkId];
            }
            else if (index > -1) {
                selectionGameTypes.splice(index, 1);
                selectionGameTypes = selectionGameTypes;
            }    
        }
        
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
            case 9: return "6ème";
            case 10: return "5ème";
            case 11: return "4ème";
            case 12: return "3ème";
            case 13: return "Seconde";
            case 14: return "Première";
            case 15: return "Terminale";
            case 16: return "Licence";
            case 17: return "Master";
        }
    }

    // const dispatch = createEventDispatcher();

    // function changePage() {
    //     setTimeout(() => {
    //         let topCont = document.getElementById('topCont');
    //         dispatch('changedPage', topCont.scrollHeight);
    //     }, 700);
    // }

    function endQuizz() {
        quizzDone = true;
        setFormStep(2);
        localStorage.setItem('quizzDone', JSON.stringify(true));
    }

    function previousFormStepWithCheck() {
        if (doQuizz) {
            setFormDone(false);
        }
        else {
            previousFormStep();
        }
    }

    function previousFormStep() {
        if (formStep == 0) {
            formDone = false;
            return;
        }
        setFormStep(formStep-1);
    }

    function nextFormStep() {
        setFormStep(formStep+1);
    }

    function setFormStep(stepId) {
        formStep = stepId;
        localStorage.setItem('formStep', JSON.stringify(formStep));
    }

    function setFormDone(bool) {
        formDone = bool;
        localStorage.setItem("formStartDone", JSON.stringify(bool));
    }

    function setDoQuizz(bool) {
        doQuizz = bool;
        if (doQuizz) {
            quizzDone = false;
            currentQuestionId = 0;
            localStorage.setItem('currStep', JSON.stringify(currentQuestionId));
            localStorage.setItem('quizzDone', JSON.stringify(false));
        }
        localStorage.setItem("doQuizz", JSON.stringify(bool));
    }

    let orderConfirmed = false;
    let order;
    function confirmOrder(_order) {
        order = _order;
        orderConfirmed = true;
        localStorage.setItem('order', JSON.stringify(_order));
        localStorage.setItem('orderConfirmed', JSON.stringify(true));
    }

    function cancelOrder() {
        orderConfirmed = false;
        localStorage.removeItem('order');
        localStorage.setItem('orderConfirmed', JSON.stringify(false));
    }

    let selectedBoard = null;
    function selectBoard(i) {
        selectedBoard = i;
        localStorage.setItem("selectedBoard", JSON.stringify(i));
    }
</script>


{#if loaded}
<div class="top-cont" id="topCont">
    {#if formDone == false}
        <div class="form-steps-cont" in:transitionIn={{easing:quintInOut, distance: 500, duration: 550}} out:transitionOut={{easing:quintInOut, distance: 500, duration: 550}}>
            <h1 class="purple-text text-center">Bienvenue sur le configurateur de jeu BrainBud !</h1>
            <p class="text-center sm-text" style="min-height: 200px; padding-bottom: 16px; padding-top:12px;">
                Prêt à créer votre aventure éducative personnalisée ? Avec BrainBud, chaque joueur devient le maître de son parcours d'apprentissage. Ce configurateur interactif est conçu pour vous guider pas à pas dans la création d'une expérience de jeu unique, adaptée à vos intérêts et à votre niveau. Que vous souhaitiez approfondir vos connaissances en histoire, maîtriser une nouvelle langue, ou devenir un as des mathématiques, BrainBud est là pour transformer l'apprentissage en une expérience ludique et captivante. Répondez à quelques questions simples pour commencer, et notre système de recommandation intelligent s'occupera du reste, en vous proposant des modules sur-mesure. Lancez-vous dans l'aventure BrainBud et découvrez le plaisir d'apprendre en jouant !
            </p>
            
            <div class="buttons">
                <a class="btn-sec" href="/">Retour</a>
                <button class="btn-main" on:click={() => {setFormDone(true);}}>Définir le parcours</button>
                <button class="btn-special" on:click={() => {setDoQuizz(true); setFormDone(true);}}>Passer le test</button>
            </div>
        </div>

    {:else if currentQuestionId != null && doQuizz && !quizzDone}
        {#if currentQuestionId != quizzList.length}
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
        {/if}

        {#if currentQuestionId == quizzList.length}
            <div class="results" in:transitionIn={{easing:quintInOut, distance: 500, duration: 550, delay: 320}} out:transitionOut={{easing:quintInOut, distance: 500, duration: 550}}>
                <p class="results-title">Notre sélection recommandée</p>
                <div class="recommended-subjects">
                    {#each recommendedGames(answeredList, games) as recommendedGame,i}
                        <div class="reco-game-card r-{i}">
                            <p>{recommendedGame.title}</p>    
                            <p>{recommendedGame.text}</p>
                        </div>
                    {/each}
                </div>
                
                <div class="buttons" style="display: flex; flex-direction: column; padding-top: 60px;">
                    <button class="btn-sec">Retour à l'accueil</button>
                    <button class="btn-main" on:click={() => endQuizz()}>Cette sélection me convient</button>
                </div>
            </div>
        {/if}

    {:else if !orderConfirmed}
        {#if formStep == 0}
            <div in:transitionIn={{easing:quintInOut, distance: 500, duration: 550, delay: 100}} out:transitionOut={{easing:quintInOut, distance: 500, duration: 550}}>
                <div class="form-page-cont">
                    <h1 class="purple-text text-center">Matières / disciplines à travailler</h1>
                    <p class="sm-text">Que vous soyez passionné par les défis linguistiques, la découverte géographique, les mystères de l'histoire, ou les énigmes mathématiques, chaque type de jeu est conçu pour stimuler votre curiosité et renforcer vos compétences.</p>
                    <div class="checkboxes-cont">
                        {#each checkboxes as checkbox}
                            <label for={checkbox.id} class="checkbox {selection.includes(checkbox.id) ? 'checked' : 'test'}">
                                    <input type="checkbox" name="" id="{checkbox.id}" value={checkbox.id} on:change={(event) => checkboxChanged(checkbox.id, event, 0)}>
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
                        <button class="btn-sec" on:click={() => previousFormStep()}>Retour</button>
                        <button class="btn-main" style="opacity: {selection.length >= 1 ? '1' : '0.5'}" on:click={() => {if (selection.length >= 1) {nextFormStep()}}}>Confirmer ma sélection</button>    
                    </div>    
                </div>
            </div>
        {:else if formStep == 1}
            <div in:transitionIn={{easing:quintInOut, distance: 500, duration: 550, delay: 100}} out:transitionOut={{easing:quintInOut, distance: 500, duration: 550}}>
                <div class="form-page-cont">
                    <h1 class="purple-text text-center">Types de jeux</h1>
                    <p class="sm-text">
                        Ici, vous avez la liberté de choisir le style de jeu qui correspond le mieux à votre personnalité et à vos préférences d'apprentissage. Ici, pas besoin de passer par un quiz, vous êtes le capitaine de votre parcours ludique et éducatif !
                    </p>
                    
                    <div class="checkboxes-cont">
                        {#each checkboxes2 as checkbox}
                            <label for={checkbox.id} class="checkbox {selectionGameTypes.includes(checkbox.id) ? 'checked' : 'test'}">
                                    <input type="checkbox" name="" id="{checkbox.id}" value={checkbox.id} on:change={(event) => checkboxChanged(checkbox.id, event, 1)}>
                                    {checkbox.text}
                            </label>
                        {/each}    
                    </div>

                    <div style="display: flex; flex-direction: column; gap: 20px;">
                        <button class="btn-sec" on:click={() => previousFormStep()}>Retour</button>
                        <button class="btn-main" style="opacity: {selectionGameTypes.length >= 1 ? '1' : '0.5'}" on:click={() => {if (selectionGameTypes.length >= 1) {nextFormStep()}}}>Confirmer ma sélection</button>    
                    </div>    
                </div>
            </div>
        {:else if formStep == 2}
            <div in:transitionIn={{easing:quintInOut, distance: 500, duration: 550, delay: 100}} out:transitionOut={{easing:quintInOut, distance: 500, duration: 550}}>
                <div class="form-page-cont">
                    <h1 class="purple-text text-center">Donnez vie à votre jeu !</h1>
                    <p class="sm-text">
                        Bienvenue dans l'univers créatif de BrainBud, où vous pouvez choisir le design de votre plateau de jeu ! Grâce à notre technologie de pointe basée sur l'IA, nous vous proposons une sélection unique de designs, tous plus captivants les uns que les autres. Que vous aimiez les motifs colorés, les thèmes inspirés de la nature, les univers fantastiques ou les designs futuristes, notre IA s'adapte à vos goûts pour vous offrir un choix personnalisé.
                    </p>
                    
                    <div class="boards">
                        {#each {length: 4} as _, i}
                            <!-- svelte-ignore a11y-click-events-have-key-events -->
                            <!-- svelte-ignore a11y-no-static-element-interactions -->
                            <div class="board-img-cont {selectedBoard == i ? 'selected' : ''}" on:click={() => selectBoard(i)}>
                                <img src="images/board-{i+1}.png" alt="">
                            </div>
                        {/each}
                    </div>

                    <div style="display: flex; flex-direction: column; gap: 20px;">
                        <button class="btn-sec" on:click={() => previousFormStepWithCheck()}>Retour</button>
                        <button class="btn-main" style="opacity: {selectedBoard != null ? '1' : '0.5'}" on:click={() => { if (selectedBoard !=  null) {nextFormStep()}}}>Confirmer ma sélection</button>    
                    </div>    
                </div>
            </div>
        {:else if formStep == 3}
            <div in:transitionIn={{easing:quintInOut, distance: 500, duration: 550, delay: 100}} out:transitionOut={{easing:quintInOut, distance: 500, duration: 550}}>
                <div class="form-page-cont">
                    <h1 class="purple-text text-center">Créez vos pions personnalisés</h1>                    
                    <div class="pawn-selection">
                        <iframe title="pawn-selector" src='https://my.spline.design/untitled-3757e644012462f564eff029328e4062/' frameborder='0' width='100%' height='100%'></iframe>
                    </div>

                    <div style="display: flex; flex-direction: column; gap: 20px;">
                        <button class="btn-sec" on:click={() => previousFormStep()}>Retour</button>
                        <button class="btn-main" on:click={() => nextFormStep()}>Confirmer ma sélection</button>    
                    </div>    
                </div>
            </div>
        {:else if formStep == 4}
            <div in:transitionIn={{easing:quintInOut, distance: 500, duration: 550, delay: 100}} out:transitionOut={{easing:quintInOut, distance: 500, duration: 550}}>
                <div class="form-page-cont">
                    <h1 class="purple-text text-center">Résumons</h1>                    
                    <div class="resume">
                        <div>
                            {#if recommendedGamesSaved}
                                <p class="md-text">Mes jeux recommandés</p>
                                <div>
                                    {#each recommendedGamesSaved as recommendedGame}
                                        <p>{recommendedGame.title}</p>
                                    {/each}
                                </div>
                            {:else}
                                <p class="md-text">Mes types de jeux choisis</p>
                                <p>Jeux d'adresse</p>
                                <p>Jeux d'aventure</p>
                            {/if}
                        </div>
                        <div>
                            <p class="md-text">Mes pions</p>
                            <div class="pions-img-cont">
                                <img src="images/pions.svg" alt="">    
                            </div>
                        </div>
                        <div>
                            <p class="md-text">Mon plateau de jeu</p>
                            <div class="selected-board-img-cont">
                                <img src="images/board-{selectedBoard+1}.png" alt="">    
                            </div>
                        </div>
                    </div>

                    <div style="display: flex; flex-direction: column; gap: 20px;">
                        <button class="btn-sec" on:click={() => previousFormStep()}>Retour</button>
                        <button class="btn-main" on:click={() => confirmOrder({board: selectedBoard, recommendedGames: recommendedGamesSaved})}>Valider et passer la commande</button> 
                    </div>    
                </div>
            </div>
        {/if}
    {:else}
        <div in:transitionIn={{easing:quintInOut, distance: 500, duration: 550, delay: 100}} out:transitionOut={{easing:quintInOut, distance: 500, duration: 550}}>
            <div class="form-page-cont">
                <h1 class="purple-text text-center">Commande confirmée !</h1>                    
                <p class="sm-text" style="font-size:20px; font-weight:500;">Votre commande à bien été prise en compte, vous pouvez suivre sa progression depuis votre compte !</p>

                <div style="display: flex; flex-direction: column; gap: 20px;">
                    <a class="btn-sec" href="/">Retourner à l'accueil</a>
                    <a class="btn-main" href="/account">Aller à mon compte</a> 
                </div>    
            </div>
        </div>
    {/if}    
</div>
{/if}




<style>

    .sm-text {
        font-weight: 400;
        font-size: 18px;
        text-align: center;
    }

    .md-text {
        font-weight: 600;
        font-size: 20px;
        color: var(--purple);
    }

    .selected-board-img-cont {
        width: 200px;
        height: 200px;
        overflow: hidden;
        border-radius: 20px;
        border: 1px solid rgba(0, 0, 0, 0.26);
    }

    .selected-board-img-cont > img {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

    .boards {
        display: flex;
        flex-direction: row;
        gap: 22px
    }
    @media screen and (min-width:681px) and (max-width:1080px) {
        .boards {
            flex-wrap: wrap !important;
        }
    }
    @media screen and (max-width:680px) {
        .boards {
            flex-direction: column !important;
            justify-content: center;
            align-items: center;
        }
    }

    .board-img-cont {
        width: 200px;
        height: 200px;
        border-radius: 16px;
        overflow: hidden;
        border: 2px solid rgba(0, 0, 0, 0.18);
        transition: all 0.15s ease-out;
        transform: scale(1);
    }

    .board-img-cont.selected {
        border: 2px solid var(--purple);
        transform: scale(1.03);
    }

    .board-img-cont > img {
        object-fit: cover;
        width: 100%;
        height: 100%;
        opacity: 0.8;
        transition: all 0.15s ease-out;
    }

    .board-img-cont.selected > img {
        opacity: 1;
    }

    .pions-img-cont {
        height: 200px;
        width: 240px;
        overflow: hidden;
    }

    .pions-img-cont > img {
        object-fit: contain;
        width: 100%;
        height: 100%;
    }

    .resume {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        gap: 26px;
        padding: 0px 40px;
    }

    @media screen and (max-width:800px) {
        .resume {
            flex-direction: column;
            align-items: center;
        }
        .resume > * {
            text-align: center;
        }
    }

    .pawn-selection {
        width: 100%;
        height: 380px
    }

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
        justify-content: center;
        align-items: center;
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
        max-width: 600px;
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
        font-size: 16px;
    }
</style>