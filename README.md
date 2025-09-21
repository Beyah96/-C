<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Progression Annuelle - Mathématiques 7ème SN</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet" />
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f8fafc; /* slate-50 */
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 400px;
            margin-left: auto;
            margin-right: auto;
            height: 300px;
            max-height: 400px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 350px;
            }
        }
        .week-box.active {
            transform: scale(1.05);
            box-shadow: 0 0 0 3px #0891b2; /* cyan-600 */
        }
        .week-box {
            min-height: 96px;
        }
    </style>
</head>
<body class="text-slate-800">

    <div class="container mx-auto p-4 sm:p-6 lg:p-8">
        <header class="text-center mb-8">
            <h1 class="text-3xl md:text-4xl font-bold text-slate-900">Progression Pédagogique Annuelle</h1>
            <p class="text-lg text-slate-600 mt-2">Mathématiques - Classe de 7ème SN (2025-2026)</p>
        </header>

        <main class="grid grid-cols-1 lg:grid-cols-3 gap-8">

            <section id="timeline-section" class="lg:col-span-2 bg-white p-6 rounded-2xl shadow-lg">
                <h2 class="text-2xl font-bold mb-4">Calendrier Interactif</h2>
                <p class="text-slate-600 mb-6">Cliquez sur une semaine pour afficher le détail du programme et les évaluations prévues. Le calendrier s'étend du 6 octobre 2025 au 30 juin 2026.</p>
                <div id="timeline-container" class="space-y-6">
                    <!-- Semaine blocs générés par JavaScript ici -->
                </div>
            </section>

            <aside class="space-y-8">
                <section id="details-section" class="bg-white p-6 rounded-2xl shadow-lg sticky top-8">
                    <h2 class="text-2xl font-bold mb-4" id="details-week-title">Semaine 1</h2>
                    <div id="details-content">
                        <h3 class="text-lg font-semibold text-cyan-700" id="details-chapter">Chargement...</h3>
                        <ul class="mt-3 space-y-2 text-slate-600 list-disc list-inside" id="details-topics"></ul>
                    </div>
                    <div id="details-evaluation" class="mt-4"></div>
                </section>

                <section class="bg-white p-6 rounded-2xl shadow-lg">
                    <h2 class="text-2xl font-bold mb-4 text-center">Répartition du Programme</h2>
                    <div class="chart-container">
                        <canvas id="domain-chart"></canvas>
                    </div>
                </section>
            </aside>

        </main>
    </div>

    <script>
        const scheduleData = {
            "octobre": [
                { week: 1, chapter: "Arithmétique (Ch. 1)", evaluation: "Début des cours le 6 octobre" },
                { week: 2, chapter: "Arithmétique (Ch. 1)", evaluation: "" },
                { week: 3, chapter: "Limites & Continuité (Ch. 5)", evaluation: "" },
                { week: 4, chapter: "Limites & Continuité (Ch. 5)", evaluation: "Devoir Mensuel" }
            ],
            "novembre": [
                { week: 5, chapter: "Dérivation & Primitives (Ch. 6)", evaluation: "" },
                { week: 6, chapter: "Dérivation & Primitives (Ch. 6)", evaluation: "" },
                { week: 7, chapter: "Étude de fonctions (Ch. 7)", evaluation: "" },
                { week: 8, chapter: "Systèmes linéaires (Ch. 2)", evaluation: "Devoir Mensuel" }
            ],
            "decembre": [
                { week: 9, chapter: "Nombres complexes 1 (Ch. 3)", evaluation: "" },
                { week: 10, chapter: "Nombres complexes 1 (Ch. 3)", evaluation: "Examen Trimestriel 1" },
                { week: 11, chapter: "Vacances de Noël", evaluation: "" },
                { week: 12, chapter: "Vacances de Noël", evaluation: "" }
            ],
            "janvier": [
                { week: 13, chapter: "Nombres complexes 2 (Ch. 4)", evaluation: "" },
                { week: 14, chapter: "Nombres complexes 2 (Ch. 4)", evaluation: "" },
                { week: 15, chapter: "Fonctions ln et exp (Ch. 8)", evaluation: "Devoir Mensuel" },
                { week: 16, chapter: "Fonctions ln et exp (Ch. 8)", evaluation: "" }
            ],
            "fevrier": [
                { week: 17, chapter: "Calcul vectoriel 1 (Ch. 12)", evaluation: "" },
                { week: 18, chapter: "Calcul vectoriel 1 (Ch. 12)", evaluation: "" },
                { week: 19, chapter: "Transformations 1 (Ch. 14)", evaluation: "" },
                { week: 20, chapter: "Transformations 1 (Ch. 14)", evaluation: "Devoir Mensuel" }
            ],
            "mars": [
                { week: 21, chapter: "Vacances de Printemps", evaluation: "" },
                { week: 22, chapter: "Vacances de Printemps", evaluation: "" },
                { week: 23, chapter: "Transformations 2 (Ch. 15)", evaluation: "" },
                { week: 24, chapter: "Transformations 2 (Ch. 15)", evaluation: "Examen Trimestriel 2" }
            ],
            "avril": [
                { week: 25, chapter: "Calcul intégral (Ch. 10)", evaluation: "" },
                { week: 26, chapter: "Calcul intégral (Ch. 10)", evaluation: "" },
                { week: 27, chapter: "Suites numériques (Ch. 9)", evaluation: "Devoir Mensuel" },
                { week: 28, chapter: "Suites numériques (Ch. 9)", evaluation: "" }
            ],
            "mai": [
                { week: 29, chapter: "Équations différentielles (Ch. 11)", evaluation: "" },
                { week: 30, chapter: "Équations différentielles (Ch. 11)", evaluation: "" },
                { week: 31, chapter: "Courbes paramétrées (Ch. 16)", evaluation: "Devoir Mensuel" },
                { week: 32, chapter: "Courbes paramétrées (Ch. 16)", evaluation: "" }
            ],
            "juin": [
                { week: 33, chapter: "Coniques (Ch. 17)", evaluation: "" },
                { week: 34, chapter: "Probabilités (Ch. 18)", evaluation: "" },
                { week: 35, chapter: "Révisions générales", evaluation: "Devoir Mensuel" },
                { week: 36, chapter: "Révisions générales", evaluation: "Examen Trimestriel 3" }
            ]
        };

        const chapterDetails = {
            "Arithmétique (Ch. 1)": ["Divisibilité dans Z", "Nombres premiers", "PGCD - PPCM", "Entiers premiers entre eux", "Congruences dans Z"],
            "Systèmes linéaires (Ch. 2)": ["Définitions", "Opérations élémentaires", "La méthode de Gauss"],
            "Nombres complexes 1 (Ch. 3)": ["Vision géométrique", "Forme algébrique", "Conjugaison", "Module", "Argument", "Forme trigonométrique", "Équations du second degré"],
            "Nombres complexes 2 (Ch. 4)": ["Argument d'un nombre non nul", "Notation exponentielle", "Interprétation géométrique", "Racine n-ième de l'unité", "Racines n-ième complexes"],
            "Limites & Continuité (Ch. 5)": ["Rappels sur les limites", "Opérations et formes indéterminées", "Théorèmes de comparaison", "Limite d'une fonction composée"],
            "Dérivation & Primitives (Ch. 6)": ["Dérivabilité", "Dérivées successives", "Dérivée d'une fonction composée", "Primitives"],
            "Étude de fonctions (Ch. 7)": ["Généralités (domaine, parité, etc.)", "Plan d'étude", "Fonction réciproque"],
            "Fonctions ln et exp (Ch. 8)": ["Fonction Logarithme Népérien", "Fonction Exponentielle", "Puissances et croissances comparées"],
            "Suites numériques (Ch. 9)": ["Généralités sur les suites", "Limite d'une suite", "Suites adjacentes"],
            "Calcul intégral (Ch. 10)": ["Notion d'intégrale", "Propriétés de l'intégrale", "Calculs d'intégrales (IPP)", "Calcul d'aires et de volumes"],
            "Équations différentielles (Ch. 11)": ["Équation y' + ay = 0", "Équation y'' + ay' + by = 0", "Applications physiques"],
            "Calcul vectoriel 1 (Ch. 12)": ["Angles orientés", "Cocyclicité", "Barycentre", "Déterminant dans le plan"],
            "Transformations 1 (Ch. 14)": ["Transformations dans le plan (Translation, Homothétie, Réflexion, Rotation)", "Transformations dans l'espace"],
            "Transformations 2 (Ch. 15)": ["Isométrie dans le plan", "Similitude directe"],
            "Courbes paramétrées (Ch. 16)": ["Définition et tangente", "Interprétation cinématique", "Construction d'une courbe"],
            "Coniques (Ch. 17)": ["Définition par foyer et directrice", "Équation cartésienne réduite", "Définition bifocale"],
            "Probabilités (Ch. 18)": ["Dénombrement (Rappel)", "Notion de probabilité", "Probabilité conditionnelle", "Variable aléatoire", "Loi binomiale"],
            "Vacances de Noël": ["Période de repos et de fêtes."],
            "Vacances de Printemps": ["Période de repos."],
            "Révisions générales": ["Préparation aux examens finaux", "Synthèse des chapitres clés", "Résolution d'exercices type bac"]
        };

        document.addEventListener('DOMContentLoaded', () => {
            const timelineContainer = document.getElementById('timeline-container');

            for (const [month, weeks] of Object.entries(scheduleData)) {
                const monthDiv = document.createElement('div');
                const monthTitle = document.createElement('h3');
                monthTitle.className = "text-xl font-semibold capitalize mb-3";

                // Calcule et affiche l'année selon le mois
                let yearDisplay = 2025;
                if (["decembre", "janvier", "fevrier", "mars", "avril", "mai", "juin"].includes(month)) {
                    yearDisplay = 2025;
                    if (["janvier", "fevrier", "mars", "avril", "mai", "juin"].includes(month)) {
                        yearDisplay = 2026;
                    }
                }

                monthTitle.textContent = month + " " + yearDisplay;

                const weeksGrid = document.createElement('div');
                weeksGrid.className = "grid grid-cols-2 sm:grid-cols-4 gap-4";

                weeks.forEach(weekInfo => {
                    const weekBox = document.createElement('div');
                    weekBox.className = "week-box border-2 border-slate-200 rounded-lg p-3 text-center cursor-pointer hover:border-cyan-500 hover:shadow-md transition-all duration-300";
                    weekBox.dataset.week = weekInfo.week;

                    let chapterText;
                    if (weekInfo.chapter.toLowerCase().includes('vacances')) {
                        weekBox.classList.add('bg-amber-50');
                        chapterText = "Vacances";
                    } else if (weekInfo.chapter.toLowerCase().includes('révisions')) {
                        weekBox.classList.add('bg-sky-50');
                        chapterText = "Révisions";
                    } else {
                        weekBox.classList.add('bg-slate-50');
                        chapterText = weekInfo.chapter;
                    }

                    let evaluationHtml = '';
                    if (weekInfo.evaluation) {
                       evaluationHtml = `<span class="block text-xs text-red-600 font-medium">● Éval.</span>`;
                    }

                    weekBox.innerHTML = `
                        <span class="font-bold text-lg text-slate-700">Sem. ${weekInfo.week}</span>
                        <p class="text-sm text-slate-500 mt-1">${chapterText}</p>
                        ${evaluationHtml}
                    `;

                    weekBox.addEventListener('click', () => updateDetails(weekInfo.week));
                    weeksGrid.appendChild(weekBox);
                });

                monthDiv.appendChild(monthTitle);
                monthDiv.appendChild(weeksGrid);
                timelineContainer.appendChild(monthDiv);
            }

            updateDetails(1);
            createDomainChart();
        });

        function updateDetails(weekNumber) {
            const allWeeks = document.querySelectorAll('.week-box');
            allWeeks.forEach(w => w.classList.remove('active'));
            const activeWeek = document.querySelector(`.week-box[data-week='${weekNumber}']`);
            if (activeWeek) {
                activeWeek.classList.add('active');
            }

            let weekData;
            for (const month in scheduleData) {
                const found = scheduleData[month].find(w => w.week === weekNumber);
                if (found) {
                    weekData = found;
                    break;
                }
            }

            if (!weekData) return;

            document.getElementById('details-week-title').textContent = `Semaine ${weekNumber}`;
            const chapterTitleEl = document.getElementById('details-chapter');
            const topicsListEl = document.getElementById('details-topics');
            const evaluationEl = document.getElementById('details-evaluation');

            chapterTitleEl.textContent = weekData.chapter;
            topicsListEl.innerHTML = '';

            const topics = chapterDetails[weekData.chapter] || ["Aucun détail de cours pour cette période."];
            topics.forEach(topic => {
                const li = document.createElement('li');
                li.textContent = topic;
                topicsListEl.appendChild(li);
            });

            if (weekData.evaluation) {
                evaluationEl.innerHTML = `<div class="bg-red-100 border-l-4 border-red-500 text-red-700 p-3 rounded-md" role="alert">
                                              <p class="font-bold">Évaluation Prévue</p>
                                              <p>${weekData.evaluation}</p>
                                          </div>`;
            } else {
                evaluationEl.innerHTML = '';
            }
        }

        function createDomainChart() {
            const ctx = document.getElementById('domain-chart').getContext('2d');
            const data = {
                labels: ['Analyse', 'Algèbre', 'Géométrie', 'Probabilités & Divers'],
                datasets: [{
                    label: 'Semaines par Domaine',
                    data: [17, 7, 6, 6],
                    backgroundColor: [
                        'rgba(14, 165, 233, 0.7)', // sky-500
                        'rgba(244, 63, 94, 0.7)', // rose-500
                        'rgba(249, 115, 22, 0.7)', // orange-500
                        'rgba(16, 185, 129, 0.7)' // emerald-500
                    ],
                    borderColor: [
                        'rgba(14, 165, 233, 1)',
                        'rgba(244, 63, 94, 1)',
                        'rgba(249, 115, 22, 1)',
                        'rgba(16, 185, 129, 1)'
                    ],
                    borderWidth: 1
                }]
            };

            new Chart(ctx, {
                type: 'doughnut',
                data: data,
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'top',
                        },
                        title: {
                            display: false,
                        }
                    }
                }
            });
        }
    </script>

</body>
</html>
