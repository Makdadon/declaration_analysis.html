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
        
        /* Custom Scrollbar for inner content */
        .custom-scroll::-webkit-scrollbar {
            width: 8px;
        }
        .custom-scroll::-webkit-scrollbar-track {
            background: #f1f1f1; 
        }
        .custom-scroll::-webkit-scrollbar-thumb {
            background: #888; 
            border-radius: 4px;
        }
        .custom-scroll::-webkit-scrollbar-thumb:hover {
            background: #555; 
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
    <!-- Chosen Palette: Warm Neutral / Parchment
         Background: #fdf6e3 (Solarized Light Cream)
         Text: #2c3e50 (Dark Slate Blue - Ink color)
         Accents: 
            - Red (Pathos): #e74c3c
            - Blue (Logos): #2980b9
            - Gold (Ethos): #f1c40f
            - Purple (Repetition/Anaphora): #8e44ad
    -->

    <!-- Application Structure Plan: 
         1. Hero Section: Title and core mission of the app (deconstructing the document).
         2. Analytical Dashboard (Top): High-level view of the rhetorical strategy using charts (Device Distribution).
         3. Interactive Text Explorer (Main): The core interactive element.
            - Controls: Toggle buttons for "Lenses" (Ethos, Pathos, Logos, etc.).
            - View: A segmented list of the Declaration text.
            - Interaction: Clicking a lens highlights those sections in the text and reveals the specific analysis card.
         4. Grievance Tracker (Specific Section): A focused view on the "Indictment" section to handle the repetitive "He has" structure effectively using a scrollable, filterable list.
         5. Synthesis/Conclusion: A summary block reinforcing the impact.
         
         WHY: This structure moves from the abstract (data visualization) to the concrete (text analysis), allowing users to see the "big picture" strategy before diving into line-by-line nuances.
    -->

    <!-- Visualization & Content Choices:
         1. Rhetorical Device Distribution (Donut Chart): 
            - Goal: Inform/Compare. 
            - Method: Chart.js Donut. 
            - Interaction: Clicking a slice filters the text view below. 
            - Justification: Shows which rhetorical tool was most relied upon (Logos vs Pathos).
         2. The Narrative Arc (Line Chart):
            - Goal: Change/Flow.
            - Method: Chart.js Line.
            - Interaction: None (Visual guide).
            - Justification: Visualizes the emotional intensity shifting from calm Introduction (Logos) to heated Indictment (Pathos).
         3. Interactive Text Cards:
            - Goal: Inform/Context.
            - Method: HTML/Tailwind Grid.
            - Interaction: Hover/Click to reveal specific rhetorical analysis.
            - Justification: Allow users to read the original text first, then discover the hidden rhetorical layers.
    -->

    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
</head>
<body class="parchment-bg text-slate-800 font-body min-h-screen flex flex-col">

    <!-- Navbar / Header -->
    <header class="bg-slate-900 text-amber-50 p-6 shadow-md border-b-4 border-amber-600">
        <div class="max-w-7xl mx-auto flex flex-col md:flex-row justify-between items-center">
            <div>
                <h1 class="font-heading text-3xl md:text-4xl font-bold tracking-wider">The Rhetoric of Revolution</h1>
                <p class="text-sm md:text-base text-amber-200 mt-2 italic">Deconstructing the Declaration of Independence</p>
            </div>
            <div class="mt-4 md:mt-0 flex gap-4 text-sm">
                <div class="flex items-center"><span class="w-3 h-3 rounded-full bg-blue-500 mr-2"></span> Logos</div>
                <div class="flex items-center"><span class="w-3 h-3 rounded-full bg-red-500 mr-2"></span> Pathos</div>
                <div class="flex items-center"><span class="w-3 h-3 rounded-full bg-yellow-500 mr-2"></span> Ethos</div>
            </div>
        </div>
    </header>

    <main class="flex-grow max-w-7xl mx-auto w-full p-4 md:p-8 space-y-12">

        <!-- INTRO CONTEXT -->
        <section class="bg-white/80 backdrop-blur-sm p-6 rounded-lg shadow-sm border border-amber-200">
            <h2 class="font-heading text-2xl mb-4 text-slate-900 border-b border-amber-300 pb-2">Analyzing the Argument</h2>
            <p class="mb-4 leading-relaxed">
                The Declaration of Independence is not just a legal document; it is a masterclass in rhetorical persuasion. 
                This interactive tool breaks down the text into its component strategies. By exploring the sections below, 
                you can visualize how the authors balanced <strong>Logic (Logos)</strong>, <strong>Emotion (Pathos)</strong>, 
                and <strong>Credibility (Ethos)</strong> to justify a treasonous act to the world.
            </p>
        </section>

        <!-- DASHBOARD: VISUAL ANALYTICS -->
        <section class="grid grid-cols-1 md:grid-cols-2 gap-8">
            <!-- Chart 1: Device Frequency -->
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

            <!-- Chart 2: Emotional/Logical Flow -->
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
                    <p class="text-sm text-gray-600 mt-1">Select a lens to highlight specific rhetorical strategies.</p>
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
        // Data parsed from the source report provided by the user
        const analysisData = [
            // INTRODUCTION
            {
                section: "Introduction",
                text: "When in the Course of human events, it becomes necessary for one people to dissolve the political bands which have connected them with another...",
                devices: ["Ethos", "Figurative Language"],
                detail: "**Ethos:** Appeals to 'Laws of Nature'. **Metaphor:** 'dissolve political bands'.",
                impact: "Establishes the action as grave and morally sanctioned, not rash.",
                intensity: 3 // 1-5 scale for chart
            },
            {
                section: "Introduction",
                text: "a decent respect to the opinions of mankind requires that they should declare the causes which impel them to the separation.",
                devices: ["Logos"],
                detail: "**Logos:** Logical requirement to justify actions.",
                impact: "Positions colonists as rational and respectful of global opinion.",
                intensity: 2
            },
            // PREAMBLE
            {
                section: "Preamble",
                text: "We hold these truths to be self-evident...",
                devices: ["Logos"],
                detail: "**Logos:** Axiomatic truth requiring no proof.",
                impact: "Commands immediate assent; undeniable foundation.",
                intensity: 4
            },
            {
                section: "Preamble",
                text: "...that all men are created equal, that they are endowed by their Creator with certain unalienable Rights...",
                devices: ["Logos", "Pathos", "Repetition"],
                detail: "**Pathos/Logos:** Core premise of equality. **Parallelism:** 'that' clauses.",
                impact: "Appeals to self-worth and divine right to freedom.",
                intensity: 5
            },
            {
                section: "Preamble",
                text: "That to secure these rights, Governments are instituted among Men...",
                devices: ["Logos"],
                detail: "**Logos:** Social Contract Theory.",
                impact: "Defines the purpose of government to judge the King against.",
                intensity: 3
            },
            {
                section: "Preamble",
                text: "Prudence, indeed, will dictate that Governments long established should not be changed for light and transient causes...",
                devices: ["Ethos", "Figurative Language"],
                detail: "**Ethos:** Appeals to wisdom/caution.",
                impact: "Shows signers are not impulsive; builds credibility.",
                intensity: 2
            },
            {
                section: "Preamble",
                text: "But when a long train of abuses and usurpations... it is their right, it is their duty, to throw off such Government...",
                devices: ["Pathos", "Repetition"],
                detail: "**Pathos:** Anger/Indignation. **Repetition:** 'right... duty'.",
                impact: "Emotional climax of preamble; moves audience to action.",
                intensity: 5
            },
            // INDICTMENT (Sample selection for brevity but representative of the list)
            {
                section: "Indictment",
                text: "He has refused his Assent to Laws, the most wholesome and necessary for the public good.",
                devices: ["Anaphora", "Logos"],
                detail: "**Anaphora:** 'He has'. **Logos:** Evidence of obstruction.",
                impact: "Frames King as the obstacle to good governance.",
                intensity: 3
            },
            {
                section: "Indictment",
                text: "He has dissolved Representative Houses repeatedly, for opposing with manly firmness his invasions on the rights of the people.",
                devices: ["Anaphora", "Ethos"],
                detail: "**Ethos:** Praises 'manly firmness'.",
                impact: "Builds courage and credibility of colonial leaders.",
                intensity: 4
            },
            {
                section: "Indictment",
                text: "He has erected a multitude of New Offices, and sent hither swarms of Officers to harrass our people, and eat out their substance.",
                devices: ["Anaphora", "Pathos", "Figurative Language"],
                detail: "**Metaphor:** 'swarms... eat out substance'. **Pathos:** Oppression.",
                impact: "Vividly describes intrusion into daily life.",
                intensity: 5
            },
            {
                section: "Indictment",
                text: "For imposing Taxes on us without our Consent:",
                devices: ["Anaphora", "Logos"],
                detail: "**Anaphora:** 'For...'. **Logos:** Constitutional argument.",
                impact: "Direct appeal to 'no taxation without representation'.",
                intensity: 4
            },
            {
                section: "Indictment",
                text: "He has plundered our seas, ravaged our coasts, burnt our towns, and destroyed the lives of our people.",
                devices: ["Anaphora", "Pathos", "Repetition"],
                detail: "**Pathos:** Violent imagery. **Parallelism:** Violent verbs.",
                impact: "Emotional climax; paints King as a brutal invader.",
                intensity: 5
            },
            {
                section: "Indictment",
                text: "He has constrained our fellow Citizens... to become the executioners of their friends and Brethren...",
                devices: ["Anaphora", "Pathos"],
                detail: "**Pathos:** Horror of fratricide.",
                impact: "Most emotionally devastating charge.",
                intensity: 5
            },
            // DENUNCIATION
            {
                section: "Denunciation",
                text: "In every stage of these Oppressions We have Petitioned for Redress... answered only by repeated injury.",
                devices: ["Repetition", "Pathos"],
                detail: "**Repetition:** 'Repeated'.",
                impact: "Highlights suffering and King's cruelty.",
                intensity: 4
            },
            {
                section: "Denunciation",
                text: "A Prince whose character is thus marked by every act which may define a Tyrant, is unfit to be the ruler of a free people.",
                devices: ["Logos", "Ethos"],
                detail: "**Logos:** Conclusion of evidence. **Ethos:** Moral judgment.",
                impact: "Formally disqualifies the King.",
                intensity: 4
            },
            // CONCLUSION
            {
                section: "Conclusion",
                text: "We, therefore... solemnly publish and declare, That these United Colonies are, and of Right ought to be Free and Independent States...",
                devices: ["Ethos", "Logos"],
                detail: "**Ethos:** Authority of people. **Logos:** Final deduction.",
                impact: "The official legal decree of separation.",
                intensity: 5
            },
            {
                section: "Conclusion",
                text: "And for the support of this Declaration... we mutually pledge to each other our Lives, our Fortunes and our sacred Honor.",
                devices: ["Pathos", "Ethos"],
                detail: "**Pathos/Ethos:** The Ultimate Pledge.",
                impact: "Seals the document with supreme sacrifice and gravity.",
                intensity: 5
            }
        ];

        // Helper: Count devices for charts
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

        // --- CHARTS INITIALIZATION ---
        function initCharts() {
            const stats = getDeviceStats();
            
            // 1. Device Distribution (Donut)
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
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { position: 'right', labels: { boxWidth: 12, padding: 15 } },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    return context.label + ': ' + context.raw + ' instances';
                                }
                            }
                        }
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

            // 2. Emotional Arc (Line)
            // Mapping Data to Numeric Values for plotting
            const arcLabels = analysisData.map((d, i) => i + 1); // Sequence number
            const arcData = analysisData.map(d => d.intensity);
            const arcTooltips = analysisData.map(d => d.section);

            const ctxFlow = document.getElementById('flowChart').getContext('2d');
            new Chart(ctxFlow, {
                type: 'line',
                data: {
                    labels: arcLabels,
                    datasets: [{
                        label: 'Rhetorical Intensity (1-5)',
                        data: arcData,
                        borderColor: '#e67e22',
                        backgroundColor: 'rgba(230, 126, 34, 0.1)',
                        fill: true,
                        tension: 0.4,
                        pointRadius: 4,
                        pointHoverRadius: 6
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        x: { display: false, title: { display: true, text: 'Progression of Document' } },
                        y: { min: 0, max: 6, display: true, title: { display: true, text: 'Intensity' } }
                    },
                    plugins: {
                        tooltip: {
                            callbacks: {
                                title: (tooltipItems) => analysisData[tooltipItems[0].dataIndex].section,
                                label: (context) => {
                                    const item = analysisData[context.dataIndex];
                                    return `Intensity: ${item.intensity} | Devices: ${item.devices.join(", ")}`;
                                },
                                afterLabel: (context) => {
                                    const item = analysisData[context.dataIndex];
                                    return item.text.substring(0, 50) + "...";
                                }
                            }
                        },
                        legend: { display: false }
                    }
                }
            });
        }

        // --- CONTENT RENDERING LOGIC ---
        const contentGrid = document.getElementById('content-grid');
        const btns = document.querySelectorAll('.filter-btn');

        function renderCards(data) {
            contentGrid.innerHTML = '';
            
            if (data.length === 0) {
                contentGrid.innerHTML = '<div class="p-8 text-center text-gray-500">No matching sections found for this filter.</div>';
                return;
            }

            data.forEach((item, index) => {
                // Determine color border based on primary device
                let borderClass = "border-l-4 border-gray-300";
                if (item.devices.some(d => d.includes("Logos"))) borderClass = "border-l-4 border-blue-500";
                else if (item.devices.some(d => d.includes("Pathos"))) borderClass = "border-l-4 border-red-500";
                else if (item.devices.some(d => d.includes("Ethos"))) borderClass = "border-l-4 border-yellow-500";
                else if (item.devices.some(d => d.includes("Repetition"))) borderClass = "border-l-4 border-purple-500";

                const card = document.createElement('div');
                card.className = `bg-white rounded-lg shadow-sm hover:shadow-md transition-shadow duration-300 overflow-hidden ${borderClass}`;
                
                // Badges for devices
                const badges = item.devices.map(dev => {
                    let color = "bg-gray-100 text-gray-800";
                    if(dev.includes("Logos")) color = "bg-blue-100 text-blue-800";
                    if(dev.includes("Pathos")) color = "bg-red-100 text-red-800";
                    if(dev.includes("Ethos")) color = "bg-yellow-100 text-yellow-800";
                    if(dev.includes("Repetition") || dev.includes("Anaphora")) color = "bg-purple-100 text-purple-800";
                    return `<span class="inline-block px-2 py-1 text-xs font-semibold rounded mr-1 mb-1 ${color}">${dev}</span>`;
                }).join('');

                card.innerHTML = `
                    <div class="p-6 grid grid-cols-1 lg:grid-cols-12 gap-6">
                        <!-- Left: The Text -->
                        <div class="lg:col-span-7">
                            <div class="flex items-center mb-2">
                                <span class="text-xs font-bold text-amber-600 uppercase tracking-widest mr-2">${item.section}</span>
                                <div class="flex flex-wrap">${badges}</div>
                            </div>
                            <p class="font-heading text-lg text-slate-800 leading-relaxed">"${item.text}"</p>
                        </div>
                        
                        <!-- Right: The Analysis -->
                        <div class="lg:col-span-5 bg-slate-50 p-4 rounded-md border border-slate-100 flex flex-col justify-center">
                            <h4 class="text-sm font-bold text-slate-500 uppercase mb-2">Rhetorical Impact</h4>
                            <div class="text-sm text-slate-700 mb-2">${parseMarkdown(item.detail)}</div>
                            <div class="mt-2 pt-2 border-t border-slate-200">
                                <span class="text-xs font-bold text-slate-400 uppercase">Audience Effect:</span>
                                <p class="text-sm text-slate-600 italic mt-1">${item.impact}</p>
                            </div>
                        </div>
                    </div>
                `;
                contentGrid.appendChild(card);
            });
        }

        // Simple markdown parser for bolding
        function parseMarkdown(text) {
            return text.replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>');
        }

        function filterContent(type) {
            // Update buttons
            btns.forEach(btn => {
                btn.classList.remove('ring-2', 'ring-slate-400');
                if (btn.dataset.type === type) btn.classList.add('ring-2', 'ring-slate-400');
            });

            // Filter Data
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

        // Initialize
        window.addEventListener('DOMContentLoaded', () => {
            initCharts();
            renderCards(analysisData);
        });

    </script>
</body>
</html>
