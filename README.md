# declaration_analysis.html
A rhetorical study of the " Declaration of Independence" 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Rhetoric of Revolution: Interactive Declaration Analysis</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        /* Chart Container Strict Styling */
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 600px;
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
        
        /* Typography */
        @import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@400;700&family=Lato:wght@300;400;700&display=swap');
        
        .font-heading { font-family: 'Cinzel', serif; }
        .font-body { font-family: 'Lato', sans-serif; }

        .parchment-bg {
            background-color: #fdf6e3;
            background-image: radial-gradient(#e6dfc8 1px, transparent 0);
            background-size: 20px 20px;
        }
    </style>
    <!-- Chosen Palette: Warm Neutral / Parchment -->

    <!-- Application Structure Plan: 
         Updated Structure: View Switching Dashboard
         1. Header/Navigation: Persistent header with buttons to switch between the two main views: "Rhetorical Analysis" and "Student Quizzes".
         2. View 1: Rhetorical Analysis (Original Content): Dashboard charts, filters, and the interactive text explorer.
         3. View 2: Student Quizzes (New Content): A progress tracker listing the four Gemini quizzes. Students click to open the quiz in a new tab, then return to mark their progress. A progress bar shows overall completion.
         
         WHY: The two core user tasks (analysis and assessment) are logically separated into distinct, full-screen views, simplifying navigation and preventing the page from becoming overly long or cluttered.
    -->

    <!-- Visualization & Content Choices:
         1. Rhetorical Device Distribution (Donut Chart): Goal: Inform/Compare. Method: Chart.js Donut. Interaction: Clicking a slice filters text. 
         2. The Narrative Arc (Line Chart): Goal: Change/Flow. Method: Chart.js Line.
         3. Interactive Text Cards: Goal: Inform/Context. Method: HTML/Tailwind Grid + JS filtering.
         4. Quiz Tracker: Goal: Assessment/Progress. Method: HTML/CSS Progress Bar + JS state tracking. Interaction: Button to open external link, button to mark complete.
    -->

    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
</head>
<body class="parchment-bg text-slate-800 font-body min-h-screen flex flex-col">

    <!-- Navbar / Header & Navigation -->
    <header class="bg-slate-900 text-amber-50 p-6 shadow-md border-b-4 border-amber-600 sticky top-0 z-10">
        <div class="max-w-7xl mx-auto flex flex-col md:flex-row justify-between items-center">
            <div>
                <h1 class="font-heading text-3xl md:text-4xl font-bold tracking-wider">The Rhetoric of Revolution</h1>
            </div>
            <div class="mt-4 md:mt-0 flex gap-3 text-sm">
                <button onclick="showView('analysis')" id="btn-analysis" class="nav-btn px-4 py-2 bg-amber-600 text-white rounded hover:bg-amber-500 transition active-view ring-2 ring-amber-400">
                    <span class="mr-2">📝</span> Rhetorical Analysis
                </button>
                <button onclick="showView('quizzes')" id="btn-quizzes" class="nav-btn px-4 py-2 bg-slate-700 text-amber-200 rounded hover:bg-slate-600 transition">
                    <span class="mr-2">📚</span> Student Quizzes
                </button>
            </div>
        </div>
    </header>

    <main class="flex-grow max-w-7xl mx-auto w-full p-4 md:p-8 space-y-12">
        
        <!-- =================================================================== -->
        <!-- VIEW 1: RHETORICAL ANALYSIS DASHBOARD -->
        <!-- =================================================================== -->
        <div id="analysis-view" class="view-content">
            <section class="bg-white/80 backdrop-blur-sm p-6 rounded-lg shadow-sm border border-amber-200">
                <h2 class="font-heading text-2xl mb-4 text-slate-900 border-b border-amber-300 pb-2">Analyzing the Argument</h2>
                <p class="mb-4 leading-relaxed">
                    This view presents a breakdown of the Declaration's persuasive strategy. Visualize the balance between 
                    <strong>Logic (Logos)</strong>, <strong>Emotion (Pathos)</strong>, and <strong>Credibility (Ethos)</strong> in the charts below, 
                    then use the explorer to see these devices in action, line by line.
                </p>
            </section>

            <!-- DASHBOARD: VISUAL ANALYTICS -->
            <section class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <div class="bg-white p-6 rounded-lg shadow-md border-t-4 border-blue-600">
                    <div class="mb-4">
                        <h3 class="font-heading text-xl font-bold">Rhetorical Arsenal</h3>
                        <p class="text-sm text-gray-600">Distribution of rhetorical devices identified in the text.</p>
                    </div>
                    <div class="flex flex-col items-center">
                        <div class="chart-container">
                            <canvas id="deviceChart"></canvas>
                        </div>
                    </div>
                    <div class="mt-4 text-xs text-center text-gray-500 italic">
                        Click a slice to filter the text explorer below.
                    </div>
                </div>

                <div class="bg-white p-6 rounded-lg shadow-md border-t-4 border-red-600">
                    <div class="mb-4">
                        <h3 class="font-heading text-xl font-bold">The Arc of Independence</h3>
                        <p class="text-sm text-gray-600">Tracing the intensity of appeals through the document's structure.</p>
                    </div>
                    <div class="flex flex-col items-center">
                        <div class="chart-container">
                            <canvas id="flowChart"></canvas>
                        </div>
                    </div>
                    <div class="mt-4 text-xs text-center text-gray-500 italic">
                        From calm axioms to heated grievances, ending in solemn pledge.
                    </div>
                </div>
            </section>

            <!-- EXPLORER: THE TEXT ANNOTATION -->
            <section id="explorer-section">
                <div class="flex flex-col md:flex-row justify-between items-end mb-6 border-b border-gray-300 pb-4">
                    <div>
                        <h2 class="font-heading text-2xl font-bold text-slate-800">Document Explorer</h2>
                        <p class="text-sm text-gray-600 mt-1">Select a lens to highlight specific rhetorical strategies in the text.</p>
                    </div>
                    
                    <!-- Filters -->
                    <div class="flex flex-wrap gap-2 mt-4 md:mt-0" id="filter-container">
                        <button onclick="filterContent('all')" class="filter-btn px-4 py-2 bg-slate-800 text-white rounded hover:bg-slate-700 transition active-filter ring-2 ring-slate-400" data-type="all">All</button>
                        <button onclick="filterContent('Logos')" class="filter-btn px-4 py-2 bg-blue-100 text-blue-800 rounded hover:bg-blue-200 transition" data-type="Logos">Logos (Logic)</button>
                        <button onclick="filterContent('Pathos')" class="filter-btn px-4 py-2 bg-red-100 text-red-800 rounded hover:bg-red-200 transition" data-type="Pathos">Pathos (Emotion)</button>
                        <button onclick="filterContent('Ethos')" class="filter-btn px-4 py-2 bg-yellow-100 text-yellow-800 rounded hover:bg-yellow-200 transition" data-type="Ethos">Ethos (Credibility)</button>
                        <button onclick="filterContent('Repetition')" class="filter-btn px-4 py-2 bg-purple-100 text-purple-800 rounded hover:bg-purple-200 transition" data-type="Repetition">Anaphora/Repetition</button>
                    </div>
                </div>

                <!-- Dynamic Content Area -->
                <div id="content-grid" class="grid grid-cols-1 gap-6">
                    <!-- Javascript will populate this -->
                </div>
            </section>
        </div>

        <!-- =================================================================== -->
        <!-- VIEW 2: STUDENT QUIZZES TRACKER -->
        <!-- =================================================================== -->
        <div id="quizzes-view" class="view-content hidden">
            <section class="bg-white/80 backdrop-blur-sm p-6 rounded-lg shadow-sm border border-amber-200 mb-8">
                <h2 class="font-heading text-2xl mb-4 text-slate-900 border-b border-amber-300 pb-2">Student Assessment Challenges</h2>
                <p class="mb-4 leading-relaxed">
                    Test your knowledge with these four interactive challenges hosted by Gemini. Click the "Start Quiz" button to open the challenge in a new tab. Once you complete the quiz, return here and mark it as complete to track your progress!
                </p>
                
                <!-- Progress Bar -->
                <div class="mt-6">
                    <h3 class="font-heading text-lg mb-2 text-slate-700">Overall Progress</h3>
                    <div class="w-full bg-gray-200 rounded-full h-4 relative">
                        <div id="progress-bar" class="h-4 rounded-full transition-all duration-500 bg-amber-600" style="width: 0%;"></div>
                        <span id="progress-text" class="absolute top-0 left-1/2 -translate-x-1/2 text-xs font-bold text-white leading-4">0/4 Complete</span>
                    </div>
                </div>
            </section>

            <!-- Quiz List -->
            <div id="quiz-list" class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <!-- Javascript will populate this -->
            </div>
        </div>

    </main>

    <footer class="bg-slate-900 text-slate-400 py-8 mt-12 border-t border-amber-600">
        <div class="max-w-7xl mx-auto text-center px-4">
            <p class="font-heading text-lg text-amber-100 mb-2">"We mutually pledge to each other our Lives, our Fortunes and our sacred Honor."</p>
            <p class="text-sm">Interactive Analysis generated based on rhetorical annotation report.</p>
        </div>
    </footer>

    <!-- LOGIC -->
    <script>
        // --- DATA STORAGE ---
        const analysisData = [
            { section: "Introduction", text: "When in the Course of human events, it becomes necessary for one people to dissolve the political bands which have connected them with another...", devices: ["Ethos", "Figurative Language"], detail: "**Ethos:** Appeals to 'Laws of Nature'. **Metaphor:** 'dissolve political bands'.", impact: "Establishes the action as grave and morally sanctioned, not rash.", intensity: 3 },
            { section: "Introduction", text: "a decent respect to the opinions of mankind requires that they should declare the causes which impel them to the separation.", devices: ["Logos"], detail: "**Logos:** Logical requirement to justify actions.", impact: "Positions colonists as rational and respectful of global opinion.", intensity: 2 },
            { section: "Preamble", text: "We hold these truths to be self-evident...", devices: ["Logos"], detail: "**Logos:** Axiomatic truth requiring no proof.", impact: "Commands immediate assent; undeniable foundation.", intensity: 4 },
            { section: "Preamble", text: "...that all men are created equal, that they are endowed by their Creator with certain unalienable Rights...", devices: ["Logos", "Pathos", "Repetition"], detail: "**Pathos/Logos:** Core premise of equality. **Parallelism:** 'that' clauses.", impact: "Appeals to self-worth and divine right to freedom.", intensity: 5 },
            { section: "Preamble", text: "That to secure these rights, Governments are instituted among Men, deriving their just powers from the consent of the governed,", devices: ["Logos"], detail: "**Logos:** Social Contract Theory.", impact: "Defines the purpose of government to judge the King against.", intensity: 3 },
            { section: "Preamble", text: "Prudence, indeed, will dictate that Governments long established should not be changed for light and transient causes...", devices: ["Ethos", "Figurative Language"], detail: "**Ethos:** Appeals to wisdom/caution.", impact: "Shows signers are not impulsive; builds credibility.", intensity: 2 },
            { section: "Preamble", text: "But when a long train of abuses and usurpations... it is their right, it is their duty, to throw off such Government...", devices: ["Pathos", "Repetition"], detail: "**Pathos:** Anger/Indignation. **Repetition:** 'right... duty'.", impact: "Emotional climax of preamble; moves audience to action.", intensity: 5 },
            { section: "Indictment", text: "He has refused his Assent to Laws, the most wholesome and necessary for the public good.", devices: ["Anaphora", "Logos"], detail: "**Anaphora:** 'He has'. **Logos:** Evidence of obstruction.", impact: "Frames King as the obstacle to good governance.", intensity: 3 },
            { section: "Indictment", text: "He has erected a multitude of New Offices, and sent hither swarms of Officers to harrass our people, and eat out their substance.", devices: ["Anaphora", "Pathos", "Figurative Language"], detail: "**Metaphor:** 'swarms... eat out substance'. **Pathos:** Oppression.", impact: "Vividly describes intrusion into daily life.", intensity: 5 },
            { section: "Indictment", text: "For imposing Taxes on us without our Consent:", devices: ["Anaphora", "Logos"], detail: "**Anaphora:** 'For...'. **Logos:** Constitutional argument.", impact: "Direct appeal to 'no taxation without representation'.", intensity: 4 },
            { section: "Indictment", text: "He has plundered our seas, ravaged our coasts, burnt our towns, and destroyed the lives of our people.", devices: ["Anaphora", "Pathos", "Repetition"], detail: "**Pathos:** Violent imagery. **Parallelism:** Violent verbs.", impact: "Emotional climax; paints King as a brutal invader.", intensity: 5 },
            { section: "Denunciation", text: "A Prince whose character is thus marked by every act which may define a Tyrant, is unfit to be the ruler of a free people.", devices: ["Logos", "Ethos"], detail: "**Logos:** Conclusion of evidence. **Ethos:** Moral judgment.", impact: "Formally disqualifies the King.", intensity: 4 },
            { section: "Conclusion", text: "And for the support of this Declaration... we mutually pledge to each other our Lives, our Fortunes and our sacred Honor.", devices: ["Pathos", "Ethos"], detail: "**Pathos/Ethos:** The Ultimate Pledge.", impact: "Seals the document with supreme sacrifice and gravity.", intensity: 5 }
        ];

        const quizData = [
            { id: 1, title: "The Preamble Challenge", url: "https://gemini.google.com/share/5b9d5c5ac965" },
            { id: 2, title: "Rhetorical Devices Deep Dive", url: "https://gemini.google.com/share/af9e5fea0349" },
            { id: 3, title: "Logos, Pathos, Ethos Check", url: "https://gemini.google.com/share/a831578b9480" },
            { id: 4, title: "Understanding the Indictment", url: "https://gemini.google.com/share/7c01abd51ad3" }
        ];
        
        // Simple state tracking for quizzes (simulated persistence)
        let quizCompletion = {}; // { 1: false, 2: false, ... }

        // --- CORE APPLICATION LOGIC ---

        function getDeviceStats() {
            const stats = { Logos: 0, Pathos: 0, Ethos: 0, Repetition: 0, Figurative: 0 };
            analysisData.forEach(item => {
                if (item.devices.some(d => d.includes("Logos"))) stats.Logos++;
                if (item.devices.some(d => d.includes("Pathos"))) stats.Pathos++;
                if (item.devices.some(d => d.includes("Ethos"))) stats.Ethos++;
                if (item.devices.some(d => d.includes("Repetition") || d.includes("Anaphora"))) stats.Repetition++;
                if (item.devices.some(d => d.includes("Figurative"))) stats.Figurative++;
            });
            return stats;
        }

        function initCharts() {
            const stats = getDeviceStats();
            
            const ctxDevice = document.getElementById('deviceChart').getContext('2d');
            new Chart(ctxDevice, {
                type: 'doughnut',
                data: {
                    labels: ['Logos (Logic)', 'Pathos (Emotion)', 'Ethos (Credibility)', 'Anaphora/Repetition', 'Figurative Lang'],
                    datasets: [{
                        data: [stats.Logos, stats.Pathos, stats.Ethos, stats.Repetition, stats.Figurative],
                        backgroundColor: ['#2980b9', '#e74c3c', '#f1c40f', '#8e44ad', '#27ae60'],
                        borderWidth: 2,
                        borderColor: '#ffffff'
                    }]
                },
                options: {
                    responsive: true, maintainAspectRatio: false,
                    plugins: { legend: { position: 'right', labels: { boxWidth: 12, padding: 15 } },
                        tooltip: { callbacks: { label: (context) => context.label + ': ' + context.raw + ' instances' } }
                    },
                    onClick: (e, activeElements) => {
                        if (activeElements.length > 0) {
                            const index = activeElements[0].index;
                            const label = ['Logos', 'Pathos', 'Ethos', 'Repetition', 'Figurative'][index];
                            filterContent(label);
                        }
                    }
                }
            });

            const arcLabels = analysisData.map((d, i) => i + 1);
            const arcData = analysisData.map(d => d.intensity);
            const ctxFlow = document.getElementById('flowChart').getContext('2d');
            new Chart(ctxFlow, {
                type: 'line',
                data: {
                    labels: arcLabels,
                    datasets: [{
                        label: 'Rhetorical Intensity (1-5)',
                        data: arcData, borderColor: '#e67e22', backgroundColor: 'rgba(230, 126, 34, 0.1)', fill: true, tension: 0.4, pointRadius: 4, pointHoverRadius: 6
                    }]
                },
                options: {
                    responsive: true, maintainAspectRatio: false,
                    scales: { x: { display: false }, y: { min: 0, max: 6, display: true, title: { display: true, text: 'Intensity' } } },
                    plugins: {
                        tooltip: {
                            callbacks: {
                                title: (tooltipItems) => analysisData[tooltipItems[0].dataIndex].section,
                                label: (context) => {
                                    const item = analysisData[context.dataIndex];
                                    return `Intensity: ${item.intensity} | Devices: ${item.devices.join(", ")}`;
                                },
                                afterLabel: (context) => analysisData[context.dataIndex].text.substring(0, 50) + "..."
                            }
                        },
                        legend: { display: false }
                    }
                }
            });
        }

        // TEXT EXPLORER RENDERING
        const contentGrid = document.getElementById('content-grid');
        const filterBtns = document.querySelectorAll('.filter-btn');

        function parseMarkdown(text) {
            return text.replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>');
        }

        function renderCards(data) {
            contentGrid.innerHTML = '';
            if (data.length === 0) {
                contentGrid.innerHTML = '<div class="p-8 text-center text-gray-500">No matching sections found for this filter.</div>';
                return;
            }

            data.forEach((item) => {
                let borderClass = "border-l-4 border-gray-300";
                if (item.devices.some(d => d.includes("Logos"))) borderClass = "border-l-4 border-blue-500";
                else if (item.devices.some(d => d.includes("Pathos"))) borderClass = "border-l-4 border-red-500";
                else if (item.devices.some(d => d.includes("Ethos"))) borderClass = "border-l-4 border-yellow-500";
                else if (item.devices.some(d => d.includes("Repetition"))) borderClass = "border-l-4 border-purple-500";

                const badges = item.devices.map(dev => {
                    let color = "bg-gray-100 text-gray-800";
                    if(dev.includes("Logos")) color = "bg-blue-100 text-blue-800";
                    if(dev.includes("Pathos")) color = "bg-red-100 text-red-800";
                    if(dev.includes("Ethos")) color = "bg-yellow-100 text-yellow-800";
                    if(dev.includes("Repetition") || dev.includes("Anaphora")) color = "bg-purple-100 text-purple-800";
                    return `<span class="inline-block px-2 py-1 text-xs font-semibold rounded mr-1 mb-1 ${color}">${dev}</span>`;
                }).join('');

                contentGrid.innerHTML += `
                    <div class="bg-white rounded-lg shadow-sm hover:shadow-md transition-shadow duration-300 overflow-hidden ${borderClass}">
                        <div class="p-6 grid grid-cols-1 lg:grid-cols-12 gap-6">
                            <div class="lg:col-span-7">
                                <div class="flex items-center mb-2">
                                    <span class="text-xs font-bold text-amber-600 uppercase tracking-widest mr-2">${item.section}</span>
                                    <div class="flex flex-wrap">${badges}</div>
                                </div>
                                <p class="font-heading text-lg text-slate-800 leading-relaxed">"${item.text}"</p>
                            </div>
                            <div class="lg:col-span-5 bg-slate-50 p-4 rounded-md border border-slate-100 flex flex-col justify-center">
                                <h4 class="text-sm font-bold text-slate-500 uppercase mb-2">Rhetorical Impact</h4>
                                <div class="text-sm text-slate-700 mb-2">${parseMarkdown(item.detail)}</div>
                                <div class="mt-2 pt-2 border-t border-slate-200">
                                    <span class="text-xs font-bold text-slate-400 uppercase">Audience Effect:</span>
                                    <p class="text-sm text-slate-600 italic mt-1">${item.impact}</p>
                                </div>
                            </div>
                        </div>
                    </div>
                `;
            });
        }

        function filterContent(type) {
            filterBtns.forEach(btn => {
                btn.classList.remove('ring-2', 'ring-slate-400');
                if (btn.dataset.type === type) btn.classList.add('ring-2', 'ring-slate-400');
            });

            if (type === 'all') {
                renderCards(analysisData);
            } else {
                const filtered = analysisData.filter(item => {
                    if (type === 'Repetition') return item.devices.some(d => d.includes("Repetition") || d.includes("Anaphora"));
                    if (type === 'Figurative') return item.devices.some(d => d.includes("Figurative"));
                    return item.devices.some(d => d.includes(type));
                });
                renderCards(filtered);
            }
        }
        
        // --- QUIZ TRACKER LOGIC ---

        function updateProgress() {
            const total = quizData.length;
            const completed = Object.values(quizCompletion).filter(c => c).length;
            const percentage = (completed / total) * 100;

            document.getElementById('progress-bar').style.width = `${percentage}%`;
            document.getElementById('progress-text').textContent = `${completed}/${total} Complete`;

            // If fully complete, change color
            if (completed === total) {
                document.getElementById('progress-bar').classList.add('bg-green-600');
                document.getElementById('progress-bar').classList.remove('bg-amber-600');
            } else {
                document.getElementById('progress-bar').classList.remove('bg-green-600');
                document.getElementById('progress-bar').classList.add('bg-amber-600');
            }
        }

        function markComplete(id) {
            quizCompletion[id] = true;
            renderQuizzes();
            updateProgress();
        }

        function renderQuizzes() {
            const list = document.getElementById('quiz-list');
            list.innerHTML = '';
            
            quizData.forEach(quiz => {
                const isCompleted = quizCompletion[quiz.id];
                const buttonClass = isCompleted 
                    ? 'bg-green-500 hover:bg-green-600 text-white' 
                    : 'bg-amber-500 hover:bg-amber-600 text-white';

                const completeButtonText = isCompleted ? 'Completed' : 'Mark Complete';
                const completeButtonAction = isCompleted ? '' : `markComplete(${quiz.id})`;
                const completeButtonDisabled = isCompleted ? 'disabled' : '';

                list.innerHTML += `
                    <div class="bg-white p-6 rounded-lg shadow-md border-t-4 ${isCompleted ? 'border-green-600' : 'border-amber-600'}">
                        <div class="flex items-center justify-between mb-4">
                            <h4 class="font-heading text-xl text-slate-800">${quiz.title}</h4>
                            ${isCompleted ? '<span class="text-green-600 font-bold">✔</span>' : '<span class="text-amber-600 font-bold">...</span>'}
                        </div>
                        <p class="text-sm text-gray-600 mb-4">An external Gemini challenge to test your knowledge of this topic. Open the link to begin.</p>
                        <div class="flex gap-3">
                            <a href="${quiz.url}" target="_blank" class="px-4 py-2 bg-blue-600 text-white rounded hover:bg-blue-700 transition">
                                <span class="mr-1">🚀</span> Start Quiz (New Tab)
                            </a>
                            <button onclick="${completeButtonAction}" class="px-4 py-2 rounded transition ${buttonClass}" ${completeButtonDisabled}>
                                ${completeButtonText}
                            </button>
                        </div>
                    </div>
                `;
            });
        }
        
        // --- VIEW SWITCHING ---

        function showView(viewId) {
            document.querySelectorAll('.view-content').forEach(view => {
                view.classList.add('hidden');
            });
            document.getElementById(`${viewId}-view`).classList.remove('hidden');

            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('bg-amber-600', 'text-white', 'ring-2', 'ring-amber-400');
                btn.classList.add('bg-slate-700', 'text-amber-200');
            });

            document.getElementById(`btn-${viewId}`).classList.remove('bg-slate-700', 'text-amber-200');
            document.getElementById(`btn-${viewId}`).classList.add('bg-amber-600', 'text-white', 'ring-2', 'ring-amber-400');
        }

        // --- INITIALIZATION ---
        window.addEventListener('DOMContentLoaded', () => {
            // Set initial completion status to false
            quizData.forEach(quiz => quizCompletion[quiz.id] = false);

            initCharts();
            renderCards(analysisData);
            renderQuizzes();
            updateProgress();
            
            // Default to the analysis view
            showView('analysis');
        });

    </script>
</body>
</html>
